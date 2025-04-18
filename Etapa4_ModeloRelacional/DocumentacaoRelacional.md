# Etapa 4 – Projeto Lógico do Banco de Dados (Modelo Relacional)

1. O **esquema relacional completo** – tabelas, atributos, tipos, chaves, restrições.  
2. A **análise de normalização** (1FN → BCNF).  

![Diagrama relacional](esquema_relacional_projeto1.png)

---

## 1. Convenções

| Símbolo | Significado |
|---------|-------------|
| **(PK)** | chave primária |
| **(FK)** | chave estrangeira (tipo omitido – usa o da referência) |
| **(U)**  | restrição de unicidade |

---

## 2. Esquema Relacional

> Cada bloco SQL já reflete fielmente os nomes/relacionamentos do diagrama.

### 2.1 EMPRESA
```sql
EMPRESA (
    cnpj   CHAR(14)  PRIMARY KEY,
    nome   VARCHAR(120) NOT NULL
);
```

### 2.2 FUNCIONARIO (e subclasses)
```sql
FUNCIONARIO (
    cpf    CHAR(11)  PRIMARY KEY,
    cnpj   (FK),                -- → EMPRESA
    nome   VARCHAR(120) NOT NULL,
    salario DECIMAL(10,2) NOT NULL
);

TECNICO       (cpf (FK,PK));
ADMINISTRADOR (cpf (FK,PK));
OPERADOR      (cpf (FK,PK));

MOTORISTA (
    cpf             (FK,PK),
    cod_habilitacao CHAR(9) (NOT NULL, U)
);

COBRADOR (cpf (FK,PK));
```

### 2.3 ESCALA e CUMPRIDA_POR
```sql
ESCALA (
    cod_escala  INT PRIMARY KEY,
    hora_inicio TIMESTAMP,
    hora_fim    TIMESTAMP,
    dia_semana  INT CHECK (dia_semana BETWEEN 0 AND 6)
);

CUMPRIDA_POR (
    cpf        (FK),
    cod_escala (FK),
    PRIMARY KEY (cpf, cod_escala)
);
```

### 2.4 USUARIO e BILHETE
```sql
USUARIO (
    cpf             CHAR(11) PRIMARY KEY,
    nome            VARCHAR(120) NOT NULL,
    data_nascimento DATE,
    genero          CHAR(1) CHECK (genero IN ('M','F','O'))
);

BILHETE (
    cod_cartao   INT PRIMARY KEY,
    saldo        DECIMAL(10,2) NOT NULL,
    tipo_cartao  INT NOT NULL,
    cpf          (FK),           -- → USUARIO
    cpf_cobrador (FK) (U)        -- → COBRADOR
);
```

### 2.5 LINHA • ROTA • PARTIDA_PREVISTA
```sql
LINHA (
    cod_linha   INT PRIMARY KEY,
    nome_linha  VARCHAR(50) NOT NULL,
    data_criacao TIMESTAMP NOT NULL DEFAULT NOW()
);

ROTA (
    cod_rota       INT PRIMARY KEY,
    texto_letreiro VARCHAR(120) NOT NULL,
    cod_linha      (FK)
);

PARTIDA_PREVISTA (
    cod_rota         (FK),
    horario_previsto TIME NOT NULL,
    dia_semana       INT CHECK (dia_semana BETWEEN 0 AND 6),
    PRIMARY KEY (cod_rota, dia_semana, horario_previsto)
);
```

### 2.6 Rede de Pontos e Trechos
```sql
PONTO (
    cod_ponto  INT PRIMARY KEY,
<<<<<<< HEAD
    latitude   FLOAT NOT NULL,
    longitude  FLOAT NOT NULL,
    nome       VARCHAR(120)
=======
    latitude   FLOAT,
    longitude  FLOAT,
    nome       VARCHAR(120),
    (latitude, longitude) (U)
>>>>>>> a85743364fa04ef3d86943ef2f17410f379c9e97
);

TRECHO (
    cod_ponto_inicio (FK),   -- → PONTO
    cod_ponto_fim    (FK),
    comprimento      FLOAT,
    tempo_medio      INTERVAL NOT NULL,
    geometria        OBJECT NOT NULL,
    PRIMARY KEY (cod_ponto_inicio, cod_ponto_fim)
);

PERCORRE (                           -- rota × trecho
    cod_rota         (FK),
    ordem            INT NOT NULL,
    cod_ponto_inicio (FK),
    cod_ponto_fim    (FK),
    PRIMARY KEY (cod_rota, ordem),
    FOREIGN KEY (cod_ponto_inicio, cod_ponto_fim)
        REFERENCES TRECHO(cod_ponto_inicio, cod_ponto_fim)
);
```

### 2.7 Frota, Modelos e Garagens
```sql
FABRICANTE (
    fabricante VARCHAR(60),
    cod_modelo (FK)
    (fabricante, cod_modelo) PRIMARY KEY
);

MODELO (
    cod_modelo INT PRIMARY KEY,
<<<<<<< HEAD
    tipo       VARCHAR(30) NOT NULL,
=======
    tipo       VARCHAR(30), -- tipo não pode estar relacionado a capacidade
>>>>>>> a85743364fa04ef3d86943ef2f17410f379c9e97
    capacidade INT,
    fabricante (FK)
);

GARAGEM (
    cod_garagem   INT PRIMARY KEY,
    logradouro    VARCHAR(120) NOT NULL,
    numero        INT NOT NULL,
    cidade        VARCHAR(80) NOT NULL,
    cep           CHAR(8) NOT NULL,
    capacidade    INT,
    estoque_diesel FLOAT,
    num_eletropostos INT
);

VEICULO (
<<<<<<< HEAD
    cod_veiculo        INT PRIMARY KEY,
    latitude           FLOAT NOT NULL,
    longitude          FLOAT NOT NULL,
    data_inicio_operacao DATE NOT NULL,
    placa              CHAR(7) (U) NOT NULL,
    status             INT NOT NULL,
    ultima_atualizacao TIMESTAMP NOT NULL,
=======
    cod_veiculo        INT PRIMARY KEY, -- pq não placa?
    latitude           FLOAT,
    longitude          FLOAT,
    data_inicio_operacao DATE,
    placa              CHAR(7) (U),
    status             INT,
    ultima_atualizacao TIMESTAMP,
>>>>>>> a85743364fa04ef3d86943ef2f17410f379c9e97
    cod_garagem        (FK),
    cod_modelo         (FK)
);
```

### 2.8 Operações
```sql
MANUTENCAO (
    cod_servico INT PRIMARY KEY,
    data        TIMESTAMP NOT NULL,
    descricao   CLOB NOT NULL,
    cpf         (FK),        -- técnico
    cod_veiculo (FK)
);

VIAGEM (
    cod_veiculo     (FK),
    datahora_inicio TIMESTAMP NOT NULL,
    cod_rota        (FK),
    cpf             (FK),    -- motorista
    datahora_fim    TIMESTAMP,
    PRIMARY KEY (cod_veiculo, cod_rota, datahora_inicio)
);

OCORRENCIA (
    cod_ocorrencia  INT PRIMARY KEY,
    tipo            INT  NOT NULL,
    descricao       CLOB  NOT NULL,
    data_hora       TIMESTAMP  NOT NULL,
    datahora_inicio (FK),
    cod_rota        (FK),
    cod_veiculo     (FK)
);

ENTRADA (
    horario         TIMESTAMP,
    cod_cartao      (FK),
    tarifa          DECIMAL(6,2)  NOT NULL,
    datahora_inicio TIMESTAMP,
    cod_rota        (FK),
    cod_veiculo     (FK),
    PRIMARY KEY (horario, datahora_inicio, cod_veiculo, cod_rota cod_cartao)
);
```

---

## 3. Normalização (até BCNF)
Vamos avaliar a adequação do modelo a cada Forma Normal.

- **1FN** – todos os atributos são atômicos e não repetitivos. 
Neste caso, precisamos observar especialmente atributos multivalorados. No esquema EER, foram especificados três atributos compostos (`VEICULO.localizacao`, `GARAGEM.endereco` e `PONTO.localizacao`), e outros dois atributos multivalorados (`MODELO.fabricante` e `ROTA.viagem_prevista`). Os atributos compostos foram representados de forma atômica no esquema relacional, e os atributos multivalorados foram convertidos em relações separadas. Assim, o esquema está na 1FN.

- **2FN** – nenhuma coluna não‑chave depende somente de parte de uma PK composta. 
Neste caso, vamos observar as tabelas com chaves primárias compostas.
    - ENTRADA: O atributo `tarifa` depende dos dados da viagem (`datahora_inicio`, `cod_rota`, `cod_veiculo`), do horário (`horario`) e do cartão utilizado (`cod_cartao`). Portanto, não há dependência parcial.
    - VIAGEM: Um motorista (representado pelo atributo `cpf`) conduz uma viagem em um veículo (`cod_veiculo`) num horário (`datahora_inicio`) e rota (`cod_rota`) específicos. A data de término de uma viagem também tem dependência funcional da combinação desses atributos. Assim, não há dependência parcial. 
    - PARTIDA_PREVISTA: Todos os atributos compõem a chave, de forma que não pode haver dependência parcial de um atributo não principal.
    - TRECHO: Todos os atributos dependem funcionalmente do ponto de início e fim (`cod_ponto_inicio`, `cod_ponto_fim`) simultaneamente, pois o trecho é definido por um intervalo entre pontos. Assim, não há dependência parcial.
    - FABRICANTE_MODELO: Todos os atributos compõem a chave e, portanto, não há dependência parcial de um atributo não principal.
Portanto, o esquema está na 2FN.

- **3FN** – nenhum atributo não‑chave depende de outro não‑chave.
Neste caso, precisamos observar as dependências funcionais de tabelas com vários atributos não principais.
    - ESCALA
    - FUNCIONÁRIO
    - USUÁRIO
    - BILHETE
    - MANUTENÇÃO
    - VIAGEM
    - VEICULO
    - GARAGEM
    - MODELO
    - OCORRENCIA 
    - PERCORRE
    - TRECHO
    - PONTO
    - LINHA

- **FNBC** – depois de separar `FABRICANTE` de `MODELO`, todo determinante é chave candidata.
Detalhamos a justificativa sobre a adequação de cada tabela à FNBC abaixo.

<<<<<<< HEAD

| Tabela | Forma normal | Observação principal |
|--------|--------------|----------------------|
| ENTIDADES com PK simples | BCNF | somente dependências triviais |
| ASSOCIAÇÕES (PK composta) | BCNF | não há atributos extras fora da PK |
| MODELO | BCNF | `cod_modelo` → resto |
| FABRICANTE | BCNF | atributo único |

---

## 4. Índices Recomendados

1. Índice em **todas as FKs** ↔ velocidade de junções.  
2. `VEICULO(placa)` (já UNIQUE).  
3. `BILHETE(cpf)` e `ENTRADA(horario)`.  
4. Índices espaciais em `PONTO(latitude, longitude)` se o SGBD suportar GIS.  
5. `(cod_veiculo, datahora_inicio)` já é PK de **VIAGEM**, serve como índice primário.

---

## 6. Visões Úteis

1. **View_ViagemCompleta** – une viagem + rota + linha + motorista + veículo.  
2. **View_ManutencaoHistorico** – histórico por veículo.  
3. **View_OcorrenciasDia** – todas as ocorrências dentro de um intervalo.

---

## 7. Conclusão

O esquema logicamente derivado do diagrama está **inteiramente em BCNF**, reduzindo redundâncias e anomalias e pronto para implementação em PostgreSQL, MySQL ou Oracle.
=======
>>>>>>> a85743364fa04ef3d86943ef2f17410f379c9e97

