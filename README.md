# Github V2

### **Conteúdo Inicial dos Arquivos:**

```other
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
```

### Estrutura de Diretório

```other
/Time-15-Hackaton/
├── backend/
│   ├── controllers/
│   │   └── professionalController.js
│   ├── models/
│   │   └── professionalModel.js
│   ├── routes/
│   │   └── professionalRoutes.js
│   ├── services/
│   │   └── professionalService.js
│   ├── utils/
│   │   └── db.js
│   ├── .env.example
│   ├── app.js
│   ├── package.json
│   └── README.md
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   └── ProfessionalForm.js
│   │   ├── pages/
│   │   │   └── HomePage.js
│   │   ├── services/
│   │   │   └── apiService.js
│   │   ├── App.js
│   │   ├── index.js
│   │   └── styles.css
│   ├── public/
│   ├── package.json
│   └── README.md
├── data-warehouse/
│   ├── scripts/
│   │   └── etlScript.js
│   ├── transformations/
│   ├── warehouse-setup.sql
│   └── README.md
├── documentation/
│   ├── api/
│   │   └── api-docs.md
│   ├── user-guides/
│   ├── architecture.md
│   ├── pitch-deck.pdf
│   └── README.md
├── .gitignore
├── docker-compose.yml
└── README.md
```

### Conteúdo Inicial dos Arquivos

#### backend/.env.example

```other
MONGO_URI=mongodb://localhost:27017/ecossistema
JWT_SECRET=sua_chave_secreta
```

#### backend/app.js

```javascript
const express = require('express');
const mongoose = require('mongoose');
const dotenv = require('dotenv');
const professionalRoutes = require('./routes/professionalRoutes');

dotenv.config();

const app = express();
app.use(express.json());

mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })
    .then(() => console.log('MongoDB connected'))
    .catch(err => console.log(err));

app.use('/api/professionals', professionalRoutes);

const PORT = process.env.PORT || 5000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

#### backend/controllers/professionalController.js

```javascript
const Professional = require('../models/professionalModel');

exports.registerProfessional = async (req, res) => {
    try {
        const newProfessional = new Professional(req.body);
        await newProfessional.save();
        res.status(201).json(newProfessional);
    } catch (err) {
        res.status(500).json({ error: err.message });
    }
};
```

#### backend/models/professionalModel.js

```javascript
const mongoose = require('mongoose');

const professionalSchema = new mongoose.Schema({
    name: String,
    cpf: String,
    state: String,
    registrationDate: Date,
    active: Boolean,
});

module.exports = mongoose.model('Professional', professionalSchema);
```

#### backend/routes/professionalRoutes.js

```javascript
const express = require('express');
const { registerProfessional } = require('../controllers/professionalController');

const router = express.Router();

router.post('/register', registerProfessional);

module.exports = router;
```

#### backend/services/professionalService.js

```javascript
// Aqui você pode adicionar funções de serviço relacionadas ao profissional, se necessário
```

#### backend/utils/db.js

```javascript
// Utilitário para configuração de conexão com o banco de dados, se necessário
```

#### frontend/src/App.js

```javascript
import React from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';
import HomePage from './pages/HomePage';
import './styles.css';

function App() {
    return (
        <Router>
            <div className="App">
                <Switch>
                    <Route path="/" component={HomePage} exact />
                </Switch>
            </div>
        </Router>
    );
}

export default App;
```

#### frontend/src/pages/HomePage.js

```javascript
import React from 'react';
import ProfessionalForm from '../components/ProfessionalForm';

function HomePage() {
    return (
        <div>
            <h1>Registro de Profissionais</h1>
            <ProfessionalForm />
        </div>
    );
}

export default HomePage;
```

#### frontend/src/components/ProfessionalForm.js

```javascript
import React, { useState } from 'react';
import apiService from '../services/apiService';

function ProfessionalForm() {
    const [formData, setFormData] = useState({
        name: '',
        cpf: '',
        state: '',
        registrationDate: '',
        active: true,
    });

    const handleChange = (e) => {
        setFormData({ ...formData, [e.target.name]: e.target.value });
    };

    const handleSubmit = async (e) => {
        e.preventDefault();
        try {
            const response = await apiService.registerProfessional(formData);
            console.log(response.data);
        } catch (err) {
            console.error(err);
        }
    };

    return (
        <form onSubmit={handleSubmit}>
            <input type="text" name="name" placeholder="Nome" onChange={handleChange} />
            <input type="text" name="cpf" placeholder="CPF" onChange={handleChange} />
            <input type="text" name="state" placeholder="Estado" onChange={handleChange} />
            <input type="date" name="registrationDate" onChange={handleChange} />
            <button type="submit">Registrar</button>
        </form>
    );
}

export default ProfessionalForm;
```

#### frontend/src/services/apiService.js

```javascript
import axios from 'axios';

const api = axios.create({
    baseURL: 'http://localhost:5000/api'
});
```

const registerProfessional = async (data) => {
return await api.post('/professionals/register', data);

};

export default {
registerProfessional

};

```other
#### data-warehouse/scripts/etlScript.js
```javascript
// Script para extrair, transformar e carregar dados para o Data Warehouse
```

#### data-warehouse/warehouse-setup.sql

```sql
-- Script para configurar o Data Warehouse
CREATE TABLE professionals (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    cpf VARCHAR(11),
    state VARCHAR(2),
    registrationDate DATE,
    active BOOLEAN
);
```

#### documentation/api/api-docs.md

```other
# Documentação da API

## Rotas

### POST /api/professionals/register
Registra um novo profissional.

#### Parâmetros
- `name` (string): Nome do profissional.
- `cpf` (string): CPF do profissional.
- `state` (string): Estado do registro.
- `registrationDate` (date): Data de registro.
- `active` (boolean): Status do profissional.

#### Exemplo de Requisição
```json
{
    "name": "João Silva",
    "cpf": "12345678901",
    "state": "SP",
    "registrationDate": "2024-07-24",
    "active": true
}
```

#### Exemplo de Resposta

```json
{
    "_id": "60f5a4f65b4c3d3f8c8f4d3e",
    "name": "João Silva",
    "cpf": "12345678901",
    "state": "SP",
    "registrationDate": "2024-07-24T00:00:00.000Z",
    "active": true,
    "__v": 0
}
```

---

Substituir `https://github.com/seu-usuario/ecossistema-integrado.git` pelo URL real do seu repositório.
