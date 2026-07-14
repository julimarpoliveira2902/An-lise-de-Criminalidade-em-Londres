# Análise de Criminalidade em Londres: Integração End-to-End com BigQuery, Python e Power BI

Este projeto demonstra a construção de um pipeline de dados integrado, desde a extração e validação de dados de segurança pública de Londres armazenados no Google Cloud Platform (GCP) até a visualização analítica em um dashboard interativo no Power BI.

## 🛠️ Tecnologias Utilizadas
* **Banco de Dados:** Google BigQuery (Google Cloud Platform)
* **Linguagem de Programação:** Python 3.13 (Bibliotecas: `google-cloud-bigquery`, `pandas`)
* **Visualização de Dados:** Power BI Desktop (Conexão via Import / Queries Otimizadas)
* **Controle de Versão:** Git & GitHub

## 📁 Estrutura do Projeto
* `conexao_bigquery_london_crime.py`: Script Python responsável pela autenticação via Conta de Serviço (Service Account) e validação da conexão com o BigQuery.
* `credenciais.json` (Ignorado no .gitignore): Chave de autenticação segura do GCP.
* `Dashboard_Crimes_Londres.pbix`: Arquivo do Power BI com o modelo de dados e relatórios.

## 🚀 Arquitetura e Implementação

### 1. Armazenamento e Validação (GCP & Python)
A base de dados pública de crimes de Londres foi hospedada no BigQuery. Para garantir o fluxo seguro e a integridade da conexão antes da etapa de BI, foi desenvolvido um script Python utilizando a biblioteca oficial do Google Cloud. O script realiza a autenticação por meio de um arquivo JSON de credenciais criptografadas e valida o esquema da tabela.

### 2. Otimização e Performance (Power BI & SQL)
Para garantir a máxima performance do dashboard e evitar o processamento desnecessário de dados na memória do Power BI, a importação foi realizada utilizando uma consulta SQL customizada com agregações (`SUM` e `GROUP BY`). Isso reduziu o volume de linhas importadas para o Power Query, otimizando o tempo de atualização.

```sql
SELECT 
    crime_date,
    borough,
    major_category,
    minor_category,
    SUM(crime_count) as total_crimes
FROM `modulo-crimes-londres.londres_seguranca.crime_londres_tabela`
GROUP BY 1, 2, 3, 4
ORDER BY crime_date DESC

📊 O Dashboard

![Demonstração do Dashboard](imagens/analise_crimes_em_londres.png)

O painel final oferece uma análise focada em:

Evolução Temporal: Identificação de tendências de aumento ou queda da criminalidade ao longo dos anos.

Distribuição Geográfica: Ranking de bairros mais críticos para apoiar a tomada de decisão e alocação de recursos.

Interatividade: Filtragem cruzada nativa para exploração detalhada por região.
