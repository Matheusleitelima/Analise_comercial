# 📊 Projeto de Dados Comercial

Este projeto tem como objetivo realizar a extração, tratamento e análise de dados comerciais utilizando SQL, Python (Pandas) e Power BI.

---

## 🗄️ 1. SQL – Extração de Dados

```sql
CREATE TABLE staging_comercial (
    cod_curso TEXT,
    curso TEXT,
    duracao INT,
    cliente TEXT,
    importe_cliente TEXT,
    professor TEXT,
    importe_professor TEXT,
    data_curso TEXT,
    turno TEXT,
    comercial TEXT,
    importe_comercial TEXT,
    pagamento_cliente TEXT,
    pais TEXT,
    cidade TEXT,
    pagamento_comercial TEXT
);

\copy staging_comercial 
FROM 'C:/temp/01.'
DELIMITER ',' 
CSV HEADER 
ENCODING 'WIN1252';
```

---

## 🐍 2. Python (Pandas) – Tratamento e Análise de Dados

```python
import pandas as pd

df = pd.read_csv(
    "C:/Users/ml982/OneDrive/Área de Trabalho/Dashboard_comercial/BASE_COMERCIAL.csv",
    encoding="cp1252"
)

# Padronização de colunas
df.columns = df.columns.str.strip().str.lower().str.replace(' ', '_')

# Tratamento de valores monetários
df['importe_comercial'] = df['importe_comercial'].str.replace('€', '', regex=False)\
                                                 .str.replace(',', '', regex=False)\
                                                 .str.strip().astype(float)

df['importe_cliente'] = df['importe_cliente'].str.replace('€', '', regex=False)\
                                               .str.replace(',', '', regex=False)\
                                               .str.strip().astype(float)

df['importe_professor'] = df['importe_professor'].str.replace('€', '', regex=False)\
                                                  .str.replace(',', '', regex=False)\
                                                  .str.strip().astype(float)

# Verificação de tipos
print(df['importe_comercial'].dtype)
print(df['importe_cliente'].dtype)
print(df['importe_professor'].dtype)

# Criação de métricas
df['lucro'] = df['importe_cliente'] - df['importe_professor'] - df['importe_comercial']
df['despesas'] = df['importe_professor'] + df['importe_comercial']

# Análises
df['lucro'].sum()
df['importe_professor'].sum()
df['importe_cliente'].sum()
df['importe_comercial'].sum()
df['despesas'].sum()

total_linhas = len(df)
print(total_linhas)

# Exportação
df.to_excel('base_formatada_comercial.xlsx', index=True)
```

---

## 📊 3. Power BI – Modelagem e Dashboard

```DAX
Lucro_total = SUM(data_base_comercial[lucro])

Total de despesas = 
SUMX(
    data_base_comercial,
    data_base_comercial[importe_professor] + data_base_comercial[importe_comercial]
)

Total matriculas = COUNTROWS(data_base_comercial)

Valor total de vendas = SUM(data_base_comercial[importe_cliente])
```

---

---

## 🛠️ Tecnologias Utilizadas

- SQL (PostgreSQL)
- Python (Pandas)
- Power BI
