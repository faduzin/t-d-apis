# T&D API's

## 📌 Contexto
Este repositório faz parte de uma **atividade de treinamento e desenvolvimento** da equipe, com o objetivo de praticar **ingestão, tratamento e análise de dados meteorológicos** utilizando **PySpark** e a **arquitetura de medalhão** (Bronze, Silver, Gold).

## 🎯 Desafio
- Coletar **dados históricos de pelo menos 10 localidades** através da API [Open-Meteo](https://open-meteo.com/).
- Tratar e organizar os dados seguindo a arquitetura de medalhão:
  - **Bronze** → ingestão de dados crus.
  - **Silver** → tratamento e estruturação dos dados.
  - **Gold** → análises e geração de insights.

## 🔄 Pipeline Desenvolvido

### 1. Ingestão (Camada Bronze)
- Utilização da biblioteca **`requests`** para realizar chamadas à API.
- Definição dos parâmetros e URL da API para coletar as séries temporais.
- Resposta retornada em **formato JSON**.
- Salvamento do arquivo em formato JSON
- Leitura do JSON em um **DataFrame PySpark**, utilizando inferência automática de schema.

### 2. Tratamento (Camada Silver)
- O JSON retornado contém um **dicionário com arrays**, onde:
- Cada array representa uma variável meteorológica (chuva, temperatura, umidade, etc.).
- O último array contém os **timestamps** correspondentes às medições.
- Cada localidade aparecia inicialmente como **uma única linha**, com todas as medidas em arrays.
- Transformações aplicadas:
- Uso de **`arrays_zip`** para alinhar variáveis às suas respectivas timestamps.
- Uso de **`explode`** para transformar cada timestamp em uma **linha separada**.
- Inferência e ajuste dos tipos de cada coluna (`printSchema` para validação).
- Resultado: um DataFrame Silver, com uma linha por **cidade + timestamp + medidas**.

### 3. Desafios Encontrados
- O volume de dados necessário (10 anos de histórico para cada localidade) **excede o limite de uma única request** da API.
- Solução:
    - Criar uma **tabela Delta** para armazenamento incremental.
    - Implementar um script que:
        - Faça múltiplas chamadas à API.
        - Trate os dados no formato Silver.
        - Dê **append** na tabela Delta para acumular histórico.

### 4. Análises (Camada Gold)
A partir da tabela Gold (já consolidada), o objetivo é responder às seguintes questões:

- 🌡️ **A temperatura global vem aumentando?**  
- ☔ **Existe alguma tendência quanto à umidade e nível de chuvas? Há correlação entre eles?**  
- 🌱 **A temperatura do solo vem tendo alterações ou se mantém estável?**

Os resultados serão apresentados em **gráficos** gerados a partir da camada Gold.

## 📊 Próximos Passos
- Construção da **tabela Delta** (armazenamento incremental).  
- Automação da ingestão e tratamento via script.  
- Geração da **camada Gold** consolidada.  
- Criação de **gráficos e relatórios** com os achados.

---

✍️ *Este repositório é parte de um exercício prático de treinamento em engenharia de dados, com foco em arquitetura de medalhão e manipulação de dados meteorológicos.*
