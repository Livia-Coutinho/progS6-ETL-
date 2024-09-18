# LC-Prog-M11S6-ETL

## Grupo

- Felipe Gomes
- Livia Coutinho
- Luisa Leite
- Marcos Florêncio
- Mike Mouadeb
- Raissa Sabino
- Stefano Tinelli


# Guia do Processo ETL

## Organização do Projeto

O projeto é composto pelos seguintes arquivos principais:

- **data_cleaning.py**: Responsável pela limpeza e transformação dos dados, como remoção de valores nulos e normalização.
- **data_extraction.py**: Faz a extração dos dados a partir de arquivos CSV ou JSON, preparando-os para o processamento subsequente.
- **etl_process.py**: Coordena o processo ETL completo, carregando os dados tratados no ClickHouse e no Supabase.
- **clickhouse_utils.py**: Contém funções para conectar e armazenar dados no ClickHouse.
- **supabase_utils.py**: Inclui funções para armazenar dados no Supabase.
- **connections.py**: Gerencia as conexões com o banco de dados, incluindo tentativas de reconexão em caso de falhas.
- **retry.py**: Implementa a lógica de reexecução em situações de falhas temporárias.

## Dependências

Para instalar as dependências do projeto, execute:

```bash
pip install -r requirements.txt
```

## Configuração

Configure as variáveis de ambiente em um arquivo `.env` ou exporte-as no terminal. Exemplos:

```bash
SUPABASE_DB=<seu_banco_supabase>
SUPABASE_USER=<seu_usuario_supabase>
SUPABASE_PASSWORD=<sua_senha_supabase>
SUPABASE_HOST=<seu_host_supabase>
SUPABASE_PORT=<sua_porta_supabase>

CLICKHOUSE_DB=<seu_banco_clickhouse>
CLICKHOUSE_HOST=<seu_host_clickhouse>
CLICKHOUSE_PORT=<sua_porta_clickhouse>
```

## Fluxo do Processo ETL

### 1. Extração de Dados (data_extraction.py)

Este módulo é responsável pela extração de dados. Ele carrega dados de arquivos CSV e JSON e os transforma em DataFrames do Pandas para facilitar a manipulação.

- **Funções principais**:
  - `load_csv_data`: Carrega dados a partir de um arquivo CSV.
  - `load_json_data`: Carrega dados a partir de um arquivo JSON.

### 2. Limpeza de Dados (data_cleaning.py)

A etapa de limpeza e transformação dos dados ocorre aqui, garantindo que os dados estejam padronizados e prontos para análise.

- **Funções principais**:
  - `clean_data`: Remove valores nulos.
  - `normalize_data`: Padroniza strings para minúsculas e ajusta datas.
  - `aggregate_data`: Agrega dados, somando valores por categoria.
  - `filter_data`: Filtra dados com base em condições, como status ativo.
  - `convert_data_types`: Converte tipos de dados, como datas e números.

### 3. Carregamento de Dados (etl_process.py)

Este script coordena o processo ETL, garantindo que os dados tratados sejam carregados no ClickHouse e no Supabase. O carregamento é feito em lotes para evitar sobrecarga dos bancos de dados.

### 4. Utilitários de Banco de Dados

#### ClickHouse (clickhouse_utils.py)

Funções para interagir com o banco de dados ClickHouse:

- `store_data_in_clickhouse`: Insere dados em lotes no ClickHouse.
- `test_clickhouse_connection`: Verifica a conexão com o ClickHouse.

#### Supabase (supabase_utils.py)

Funções para interagir com o Supabase:

- `store_data_in_supabase`: Faz o upload dos dados limpos para o Supabase.
- `test_supabase_connection`: Verifica a conexão com o Supabase.

### 5. Gerenciamento de Erros e Tentativas (connections.py e retry.py)

- **Lógica de Tentativas**: Reexecuta operações em caso de falha de conexão, utilizando um backoff exponencial (tempo de espera crescente a cada tentativa).
- **Logs**: Todos os erros são registrados para facilitar a detecção e resolução de problemas.

### Registro e Rastreabilidade

O processo ETL é completamente registrado. Logs são gerados para cada etapa, desde a extração até o carregamento dos dados. Em caso de erro, o processo é revertido, o erro é registrado e a conexão é fechada corretamente, garantindo a segurança e facilitando a depuração.

## Execução

Para executar o processo, siga os passos:

```bash
pip install -r requirements.txt
cd src
python3 etl_process.py
```

## Conclusão

Este pipeline ETL foi projetado para ser modular e escalável. Cada etapa do processo é independente, o que facilita a manutenção e a identificação de problemas. A lógica de tentativas e o tratamento de erros asseguram a integridade dos dados e a segurança no manuseio de grandes volumes de dados.
