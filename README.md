**Conteúdo Inicial dos Arquivos:**

# Ecossistema Integrado para Registros e Emissão de ARTs

## Resumo do Projeto
Este projeto visa unificar o registro dos profissionais do Sistema Confea/Crea e Mútua em uma plataforma única, permitindo a atuação em todos os estados desde o primeiro registro e padronizando a emissão da Anotação de Responsabilidade Técnica (ART).

## Equipe Envolvida
- **Miguel Cavalcanti**
- **Felipe Santana**
- **Thauane Cordeiro**
- **Varner Reis**

## Tecnologias Utilizadas
- **Frontend:** React.js, HTML, CSS
- **Backend:** Node.js, Express.js
- **Banco de Dados:** MongoDB, Amazon Redshift (Data Warehouse)
- **Autenticação:** JWT (JSON Web Tokens)
- **Comunicação entre Sistemas:** API RESTful, WebSockets
- **Outras Ferramentas:** Docker, Kubernetes, Git

## Como Rodar o Projeto

### Pré-requisitos
- Node.js (versão 14 ou superior)
- Docker (para containerização)
- MongoDB (local ou em container Docker)
- Amazon Redshift (opcional para Data Warehouse)

### Instalação
1. **Clone o repositório:**
    ```bash
    git clone https://github.com/felipsantna/Time-15-Hackaton.git
    cd Time-15-Hackaton
    ```

2. **Instale as dependências do backend:**
    ```bash
    cd backend
    npm install
    ```

3. **Instale as dependências do frontend:**
    ```bash
    cd ../frontend
    npm install
    ```

4. **Configure o arquivo `.env` para o backend:**
    Crie um arquivo `.env` na pasta `backend` com as variáveis de ambiente necessárias.
    ```plaintext
    MONGO_URI=mongodb://localhost:27017/ecossistema
    JWT_SECRET=sua_chave_secreta
    REDSHIFT_URI=sua_uri_do_redshift
    ```

5. **Suba o banco de dados MongoDB com Docker:**
    ```bash
    docker run -d -p 27017:27017 --name mongodb mongo
    ```

6. **Inicie o backend:**
    ```bash
    cd ../backend
    npm start
    ```

7. **Inicie o frontend:**
    ```bash
    cd ../frontend
    npm start
    ```

8. **Configure o Data Warehouse:**
    Siga as instruções no arquivo `warehouse-setup.sql` na pasta `data-warehouse`.

## Benefícios
- **Eficiência:** Reduz a burocracia e o tempo necessário para registros e vistos estaduais.
- **Padronização:** Centraliza e padroniza o processo de emissão de ARTs em âmbito nacional.
- **Segurança:** Garante a conformidade com as normas do Sistema Confea/Crea e Mútua.
- **Facilidade de Uso:** Profissionais podem emitir ARTs para qualquer estado através de uma única plataforma.
- **Flexibilidade:** O ecossistema é opcional, permitindo que os profissionais que preferem o sistema atual continuem a usá-lo.
