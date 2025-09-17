# 🚀 CRUD Serverless - Cadastro de Produtos

Este projeto é uma aplicação **CRUD (Create, Read, Update, Delete)** de cadastro de produtos, totalmente **serverless** na AWS.

O objetivo foi desenvolver um sistema de cadastro simples de produtos, aplicando **boas práticas de arquitetura serverless** e utilizando diversos serviços da AWS para criar uma solução escalável, resiliente e de baixo custo.

![Arquitetura do Projeto](https://mermaid.ink/svg/eyJjb2RlIjoiZ3JhcGggVERcbiAgICBBW0Zyb250ZW5kIC0gUzMvSFRNTCtKU10gLS0-IEJbQVBJIEdhdGV3YXldXG4gICAgQiAtLT4gQ1tMYW1iZGEgQ1JVRF1cbiAgICBDIC0tPiBEW0R5bmFtb0RCIC0gUHJvZHV0b3NdXG4gICAgQyAtLT4gRVtDbG91ZFdhdGNoIC0gTG9nc10iLCJtZXJtYWlkIjp7InRoZW1lIjoiZGVmYXVsdCJ9LCJ1cGRhdGVFZGl0bXIiOmZhbHNlfQ)

---

## 📂 Estrutura do Projeto

A estrutura de arquivos do projeto está organizada da seguinte forma:

arquivos-lab-crud/
│
├── CRUD.zip          # Pacote da Função Lambda com as operações CRUD em Python
├── index.html        # Página principal da aplicação (frontend)
├── script.js         # Lógica JavaScript para comunicação com a API
└── style.css         # Folha de estilos para a interface do usuário


---

## ⚙️ Funcionalidades

O sistema permite realizar as seguintes operações básicas de gerenciamento de produtos:

-   ➕ **Cadastrar produtos** (ID, Nome, Preço e Quantidade).
-   📄 **Listar produtos** já cadastrados.
-   ✏️ **Atualizar produtos** existentes na base de dados.
-   ❌ **Excluir produtos** do catálogo.

---

## 🛠️ Tecnologias Utilizadas

| Categoria      | Tecnologia                                                                                               |
| :------------- | :------------------------------------------------------------------------------------------------------- |
| **Frontend** | `HTML`, `CSS`, `JavaScript`                                                                              |
| **Backend** | ![AWS Lambda](https://img.shields.io/badge/AWS-Lambda-orange?style=for-the-badge&logo=awslambda) (`Python`) |
| **Banco de Dados**| ![Amazon DynamoDB](https://img.shields.io/badge/Amazon-DynamoDB-blue?style=for-the-badge&logo=amazondynamodb) |
| **API** | ![Amazon API Gateway](https://img.shields.io/badge/Amazon-API_Gateway-purple?style=for-the-badge&logo=amazonapigateway) |
| **Hospedagem** | ![Amazon S3](https://img.shields.io/badge/Amazon-S3-red?style=for-the-badge&logo=amazons3)             |
| **Monitoramento**| ![Amazon CloudWatch](https://img.shields.io/badge/Amazon-CloudWatch-green?style=for-the-badge&logo=amazoncloudwatch) |

---

## 🧩 Etapas do Desenvolvimento

### 🔹 1. Criação da Função Lambda

-   Desenvolvi a função responsável pelas operações **CRUD** em Python.
-   Compactei o código-fonte em um arquivo `CRUD.zip`.
-   Realizei o upload do pacote para o **AWS Lambda**.
-   Configurei o *handler* da função como:
    ```python
    CRUD.lambda_handler
    ```
-   Associei uma **IAM Role** com as permissões necessárias para que a função pudesse interagir com o Amazon DynamoDB (`DynamoDB:PutItem`, `DynamoDB:Scan`, `DynamoDB:UpdateItem`, `DynamoDB:DeleteItem`).

### 🔹 2. Criação da Tabela no DynamoDB

-   Criei a tabela `Produtos-seunome` no **Amazon DynamoDB**.
-   Defini a chave de partição como `id` (String), garantindo a unicidade de cada produto.
-   Esta tabela foi utilizada como o banco de dados NoSQL para armazenar os registros dos produtos.

### 🔹 3. Configuração do API Gateway

-   Criei uma **API HTTP** no Amazon API Gateway para expor a função Lambda como um endpoint REST.
-   Configurei as seguintes rotas e métodos:
    -   `POST /produtos` → **Criar produto**
    -   `GET /produtos` → **Listar produtos**
    -   `PUT /produtos/{id}` → **Atualizar produto**
    -   `DELETE /produtos/{id}` → **Excluir produto**
-   Integrei cada rota com a função Lambda correspondente.
-   Habilitei o **CORS (Cross-Origin Resource Sharing)** para permitir que as requisições do frontend (hospedado no S3) fossem aceitas pela API.

### 🔹 4. Desenvolvimento do Frontend

-   Criei a interface de usuário no arquivo `index.html`.
-   Estilizei a página com o `style.css` para uma experiência de usuário limpa e funcional.
-   Implementei a lógica de cliente no `script.js` para realizar chamadas assíncronas (AJAX/Fetch) à API Gateway.
-   No `script.js`, configurei uma variável para armazenar a URL da API, facilitando a comunicação entre o frontend e o backend.

### 🔹 5. Hospedagem no Amazon S3

-   Criei um bucket no **Amazon S3**.
-   Habilitei a funcionalidade de **hospedagem de site estático**.
-   Realizei o upload dos arquivos `index.html`, `style.css` e `script.js` para o bucket.
-   A aplicação se tornou acessível publicamente através da URL fornecida pelo S3.

### 🔹 6. Monitoramento com CloudWatch

-   Utilizei o **Amazon CloudWatch Logs** para monitorar as execuções da função Lambda em tempo real.
-   Analisei os logs gerados para depurar erros, verificar as requisições recebidas pela API e otimizar o desempenho.
-   Essa etapa foi fundamental para validar o fluxo completo e garantir a estabilidade da aplicação.

---

## 🖼️ Demonstração

A interface final é um painel simples de cadastro de produtos hospedado estaticamente no Amazon S3, que interage de forma dinâmica com o banco de dados DynamoDB através da camada de serviços composta pelo API Gateway e Lambda.

---

## 📊 Arquitetura do Projeto

O diagrama abaixo ilustra o fluxo de dados e a interação entre os serviços AWS utilizados:

```mermaid
flowchart TD
    A[Frontend - S3/HTML+JS] --> B[API Gateway]
    B --> C[Lambda CRUD]
    C --> D[DynamoDB - Produtos]
    C --> E[CloudWatch - Logs]
