O **Pixel do TikTok** é uma ferramenta de rastreamento que permite coletar dados sobre as ações dos visitantes em seu site. Ele é utilizado para **otimizar campanhas de anúncios, medir conversões e construir públicos personalizados** com base no comportamento dos usuários. A funcionalidade é semelhante ao pixel do Facebook, mas adaptada à plataforma TikTok.

### **Como funciona o Pixel do TikTok**
1. **Coleta de Dados**: 
   - O pixel rastreia ações específicas (eventos) realizadas pelos visitantes em seu site, como cliques, compras, preenchimento de formulários, entre outros.
   
2. **Acompanhamento de Conversões**: 
   - Ele permite identificar quais ações os usuários tomam após interagirem com seus anúncios, ajudando a medir a **eficácia da campanha**.

3. **Otimização de Anúncios**: 
   - Com os dados coletados, o TikTok ajusta a exibição dos anúncios para direcioná-los a pessoas mais propensas a converter.

4. **Criação de Públicos Personalizados**: 
   - O pixel permite criar públicos com base no comportamento dos visitantes do seu site, como pessoas que visitaram uma página específica ou adicionaram produtos ao carrinho.

---

### **Como Configurar o Pixel do TikTok**

1. **Acesse o TikTok Ads Manager**:
   - Vá para o Ads Manager e selecione **Ativos > Eventos > Web Events**.

2. **Crie o Pixel**:
   - Clique em **Configurar** e depois em **Pixel do TikTok**.
   - Dê um nome ao pixel e escolha entre:
     - **Modo de instalação manual**: para adicionar o script você mesmo.
     - **Modo via integração**: com plataformas como Shopify ou WooCommerce.

3. **Instale o Código do Pixel**:
   - Copie o código do pixel e cole-o **antes da tag de fechamento `<head>`** em todas as páginas do seu site.

4. **Configure Eventos**:
   - Você pode rastrear eventos automaticamente (como cliques em links) ou configurar **eventos personalizados**, como:
     - **ViewContent**: quando alguém visita uma página.
     - **AddToCart**: ao adicionar um produto ao carrinho.
     - **Purchase**: após uma compra.

---

### **Tipos de Eventos**
- **Eventos padrão**: pré-definidos pelo TikTok (como compra, cadastro, etc.).
- **Eventos personalizados**: você pode definir ações específicas para o seu site.

---

### **Teste e Verificação**
- Use a **Extensão do Pixel Helper do TikTok** no navegador para verificar se o pixel está instalado corretamente e funcionando.

---

### **Privacidade e Consentimento**
- Como o Pixel coleta dados pessoais, é essencial seguir as leis de privacidade, como a **LGPD** no Brasil, garantindo que os usuários aceitem os cookies e o rastreamento de dados.

Com o pixel bem configurado, você pode maximizar o desempenho de suas campanhas no TikTok, atraindo o público certo e aumentando as conversões.


Criar uma plataforma em **Node.js** e **React** para exibir métricas de campanhas com base nos dados coletados pelo **Pixel do TikTok** é um projeto ambicioso e altamente útil para otimizar suas campanhas publicitárias. A seguir, apresento um guia detalhado para ajudá-lo a desenvolver essa plataforma:

## **1. Visão Geral da Arquitetura**

### **Frontend: React**
- Interface de usuário interativa para visualizar métricas.
- Autenticação e gerenciamento de usuários.
- Gráficos e dashboards para exibir dados de campanhas.

### **Backend: Node.js**
- API para comunicação com o frontend.
- Integração com a API do TikTok (se disponível) ou processamento de dados do Pixel.
- Banco de dados para armazenar métricas e informações das campanhas.

### **Banco de Dados**
- **Relacional** (como PostgreSQL) ou **NoSQL** (como MongoDB) para armazenar dados das campanhas, usuários e métricas.

### **Serviços Adicionais**
- **Autenticação**: JWT ou OAuth para segurança.
- **Hospedagem**: Serviços como AWS, Heroku, Vercel, etc.
- **Monitoramento e Logs**: Ferramentas como Loggly, Sentry, etc.

## **2. Coleta e Integração dos Dados do Pixel do TikTok**

### **2.1. Entendendo o Pixel do TikTok**
O Pixel do TikTok coleta dados sobre as interações dos usuários com seu site, como visualizações de página, adições ao carrinho, compras, etc. Esses dados são essenciais para medir o desempenho das campanhas e otimizar os anúncios.

### **2.2. Opções de Integração**

#### **a. Utilizando a API do TikTok**
Verifique se o TikTok oferece uma API para acessar os dados coletados pelo Pixel. Caso positivo:
1. **Autenticação**: Configure OAuth ou outro método de autenticação fornecido pelo TikTok.
2. **Endpoints**: Utilize os endpoints da API para recuperar métricas e dados de conversão.
3. **Rate Limits**: Atente-se aos limites de requisições da API para evitar bloqueios.

#### **b. Processando Dados do Pixel Manualmente**
Caso não haja uma API direta, você pode:
1. **Enviar Dados para seu Backend**: Modifique o código do Pixel para enviar eventos diretamente para seu servidor.
2. **Webhooks**: Utilize webhooks para receber dados em tempo real.
3. **Armazenamento**: Armazene os dados recebidos no banco de dados para processamento posterior.

### **2.3. Implementação do Pixel Personalizado**
Para enviar dados do Pixel diretamente para seu backend:
1. **Adicionar Código Personalizado**: No script do Pixel, adicione chamadas para enviar dados para uma API no seu servidor Node.js.
2. **Segurança**: Garanta que apenas dados válidos e autenticados sejam processados.
3. **Validação**: Valide os dados recebidos para evitar inconsistências.

## **3. Desenvolvimento do Backend com Node.js**

### **3.1. Configuração Inicial**
1. **Inicializar o Projeto**:
   ```bash
   mkdir tiktok-metrics-platform
   cd tiktok-metrics-platform
   npm init -y
   ```
2. **Instalar Dependências**:
   ```bash
   npm install express mongoose dotenv cors
   npm install --save-dev nodemon
   ```

### **3.2. Estrutura de Pastas**
Organize o projeto da seguinte forma:
```
tiktok-metrics-platform/
├── backend/
│   ├── controllers/
│   ├── models/
│   ├── routes/
│   ├── utils/
│   ├── app.js
│   └── server.js
├── frontend/
│   └── [Projeto React]
├── .env
├── package.json
└── README.md
```

### **3.3. Configuração do Servidor Express**

**backend/app.js**
```javascript
const express = require('express');
const cors = require('cors');
const mongoose = require('mongoose');
require('dotenv').config();

const app = express();

// Middleware
app.use(cors());
app.use(express.json());

// Conectar ao MongoDB
mongoose.connect(process.env.MONGO_URI, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
})
.then(() => console.log('MongoDB conectado'))
.catch(err => console.log(err));

// Rotas
const campaignRoutes = require('./routes/campaignRoutes');
app.use('/api/campaigns', campaignRoutes);

// Rota de teste
app.get('/', (req, res) => {
    res.send('API do TikTok Metrics Platform');
});

module.exports = app;
```

**backend/server.js**
```javascript
const app = require('./app');
const PORT = process.env.PORT || 5000;

app.listen(PORT, () => {
    console.log(`Servidor rodando na porta ${PORT}`);
});
```

### **3.4. Modelagem de Dados**

**backend/models/Campaign.js**
```javascript
const mongoose = require('mongoose');

const CampaignSchema = new mongoose.Schema({
    name: { type: String, required: true },
    tiktokCampaignId: { type: String, required: true },
    metrics: {
        impressions: { type: Number, default: 0 },
        clicks: { type: Number, default: 0 },
        conversions: { type: Number, default: 0 },
        // Adicione outras métricas conforme necessário
    },
    createdAt: { type: Date, default: Date.now },
});

module.exports = mongoose.model('Campaign', CampaignSchema);
```

### **3.5. Rotas e Controladores**

**backend/routes/campaignRoutes.js**
```javascript
const express = require('express');
const router = express.Router();
const { getCampaigns, getCampaignById, createCampaign, updateMetrics } = require('../controllers/campaignController');

// Obter todas as campanhas
router.get('/', getCampaigns);

// Obter campanha por ID
router.get('/:id', getCampaignById);

// Criar nova campanha
router.post('/', createCampaign);

// Atualizar métricas
router.post('/update-metrics', updateMetrics);

module.exports = router;
```

**backend/controllers/campaignController.js**
```javascript
const Campaign = require('../models/Campaign');

// Obter todas as campanhas
exports.getCampaigns = async (req, res) => {
    try {
        const campaigns = await Campaign.find();
        res.json(campaigns);
    } catch (err) {
        res.status(500).json({ message: err.message });
    }
};

// Obter campanha por ID
exports.getCampaignById = async (req, res) => {
    try {
        const campaign = await Campaign.findById(req.params.id);
        if (!campaign) return res.status(404).json({ message: 'Campanha não encontrada' });
        res.json(campaign);
    } catch (err) {
        res.status(500).json({ message: err.message });
    }
};

// Criar nova campanha
exports.createCampaign = async (req, res) => {
    const { name, tiktokCampaignId } = req.body;
    const campaign = new Campaign({
        name,
        tiktokCampaignId,
    });
    try {
        const newCampaign = await campaign.save();
        res.status(201).json(newCampaign);
    } catch (err) {
        res.status(400).json({ message: err.message });
    }
};

// Atualizar métricas
exports.updateMetrics = async (req, res) => {
    const { tiktokCampaignId, metrics } = req.body;
    try {
        const campaign = await Campaign.findOne({ tiktokCampaignId });
        if (!campaign) return res.status(404).json({ message: 'Campanha não encontrada' });

        campaign.metrics = {
            impressions: (campaign.metrics.impressions || 0) + (metrics.impressions || 0),
            clicks: (campaign.metrics.clicks || 0) + (metrics.clicks || 0),
            conversions: (campaign.metrics.conversions || 0) + (metrics.conversions || 0),
            // Atualize outras métricas conforme necessário
        };

        const updatedCampaign = await campaign.save();
        res.json(updatedCampaign);
    } catch (err) {
        res.status(400).json({ message: err.message });
    }
};
```

### **3.6. Variáveis de Ambiente**

Crie um arquivo `.env` na raiz do projeto com as seguintes variáveis:

```
PORT=5000
MONGO_URI=seu_mongodb_uri_aqui
```

## **4. Desenvolvimento do Frontend com React**

### **4.1. Inicialização do Projeto React**

Dentro da pasta raiz do projeto:

```bash
npx create-react-app frontend
cd frontend
npm install axios chart.js react-chartjs-2
```

### **4.2. Estrutura de Pastas**

Organize o frontend da seguinte forma:

```
frontend/
├── src/
│   ├── components/
│   │   ├── Dashboard.js
│   │   ├── CampaignList.js
│   │   ├── CampaignDetail.js
│   │   └── [Outros Componentes]
│   ├── services/
│   │   └── api.js
│   ├── App.js
│   ├── index.js
│   └── [Outros Arquivos]
├── package.json
└── README.md
```

### **4.3. Configuração da API**

**frontend/src/services/api.js**
```javascript
import axios from 'axios';

const API_URL = 'http://localhost:5000/api';

export const getCampaigns = () => axios.get(`${API_URL}/campaigns`);

export const getCampaignById = (id) => axios.get(`${API_URL}/campaigns/${id}`);

export const createCampaign = (campaignData) => axios.post(`${API_URL}/campaigns`, campaignData);

export const updateMetrics = (metricsData) => axios.post(`${API_URL}/campaigns/update-metrics`, metricsData);
```

### **4.4. Componentes Principais**

#### **a. Lista de Campanhas**

**frontend/src/components/CampaignList.js**
```javascript
import React, { useEffect, useState } from 'react';
import { getCampaigns } from '../services/api';
import { Link } from 'react-router-dom';

const CampaignList = () => {
    const [campaigns, setCampaigns] = useState([]);

    useEffect(() => {
        fetchCampaigns();
    }, []);

    const fetchCampaigns = async () => {
        try {
            const response = await getCampaigns();
            setCampaigns(response.data);
        } catch (error) {
            console.error('Erro ao buscar campanhas:', error);
        }
    };

    return (
        <div>
            <h2>Lista de Campanhas</h2>
            <ul>
                {campaigns.map(campaign => (
                    <li key={campaign._id}>
                        <Link to={`/campaign/${campaign._id}`}>{campaign.name}</Link>
                    </li>
                ))}
            </ul>
        </div>
    );
};

export default CampaignList;
```

#### **b. Detalhes da Campanha com Gráficos**

**frontend/src/components/CampaignDetail.js**
```javascript
import React, { useEffect, useState } from 'react';
import { getCampaignById } from '../services/api';
import { useParams } from 'react-router-dom';
import { Bar } from 'react-chartjs-2';

const CampaignDetail = () => {
    const { id } = useParams();
    const [campaign, setCampaign] = useState(null);

    useEffect(() => {
        fetchCampaign();
    }, [id]);

    const fetchCampaign = async () => {
        try {
            const response = await getCampaignById(id);
            setCampaign(response.data);
        } catch (error) {
            console.error('Erro ao buscar campanha:', error);
        }
    };

    if (!campaign) return <div>Carregando...</div>;

    const data = {
        labels: ['Impressões', 'Cliques', 'Conversões'],
        datasets: [
            {
                label: 'Métricas',
                data: [
                    campaign.metrics.impressions,
                    campaign.metrics.clicks,
                    campaign.metrics.conversions,
                ],
                backgroundColor: ['#36A2EB', '#FFCE56', '#FF6384'],
            },
        ],
    };

    return (
        <div>
            <h2>{campaign.name}</h2>
            <Bar data={data} />
            {/* Adicione mais detalhes e gráficos conforme necessário */}
        </div>
    );
};

export default CampaignDetail;
```

#### **c. Dashboard Principal**

**frontend/src/components/Dashboard.js**
```javascript
import React from 'react';
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import CampaignList from './CampaignList';
import CampaignDetail from './CampaignDetail';

const Dashboard = () => {
    return (
        <Router>
            <div>
                <h1>TikTok Metrics Dashboard</h1>
                <Routes>
                    <Route path="/" element={<CampaignList />} />
                    <Route path="/campaign/:id" element={<CampaignDetail />} />
                </Routes>
            </div>
        </Router>
    );
};

export default Dashboard;
```

### **4.5. Integração com App.js**

**frontend/src/App.js**
```javascript
import React from 'react';
import Dashboard from './components/Dashboard';

function App() {
    return (
        <div className="App">
            <Dashboard />
        </div>
    );
}

export default App;
```

## **5. Integração e Fluxo de Dados**

1. **Coleta de Dados pelo Pixel**:
   - O Pixel do TikTok está instalado em seu site e coleta eventos dos usuários.
   - Esses eventos são enviados para o backend Node.js via uma rota específica (por exemplo, `/api/campaigns/update-metrics`).

2. **Processamento no Backend**:
   - O backend recebe os dados dos eventos, valida e atualiza as métricas correspondentes no banco de dados.

3. **Exibição no Frontend**:
   - O frontend React faz requisições à API para obter os dados das campanhas.
   - As métricas são exibidas em gráficos e tabelas interativas para facilitar a análise.

## **6. Considerações de Segurança e Privacidade**

### **6.1. Autenticação e Autorização**
- Implemente um sistema de autenticação para garantir que apenas usuários autorizados possam acessar os dados das campanhas.
- Utilize bibliotecas como **JWT (JSON Web Tokens)** para gerenciar tokens de autenticação.

### **6.2. Proteção de Dados**
- Garanta que os dados enviados pelo Pixel sejam armazenados de forma segura.
- Utilize HTTPS para todas as comunicações entre frontend, backend e qualquer outra API externa.

### **6.3. Conformidade com Leis de Privacidade**
- Certifique-se de que a coleta e o processamento de dados estejam em conformidade com a **LGPD** no Brasil e outras legislações aplicáveis.
- Implemente mecanismos de consentimento para rastreamento de dados e forneça opções para os usuários optarem por não serem rastreados.

## **7. Testes e Implantação**

### **7.1. Testes**
- **Backend**: Utilize ferramentas como **Jest** ou **Mocha** para testes unitários e de integração.
- **Frontend**: Utilize **React Testing Library** ou **Enzyme** para testar componentes.
- **End-to-End**: Ferramentas como **Cypress** podem ser úteis para testes de fluxo completo.

### **7.2. Implantação**
- **Backend**: Hospede o servidor Node.js em plataformas como **Heroku**, **AWS Elastic Beanstalk**, ou **DigitalOcean**.
- **Frontend**: Hospede a aplicação React em **Vercel**, **Netlify**, ou na mesma plataforma do backend.
- **Banco de Dados**: Utilize serviços gerenciados como **MongoDB Atlas** ou **AWS RDS**.

### **7.3. Monitoramento**
- Implemente monitoramento para identificar e resolver problemas rapidamente.
- Utilize ferramentas como **New Relic**, **Datadog** ou **Sentry** para monitorar a performance e erros da aplicação.

## **8. Recursos e Referências Úteis**

- **Documentação do TikTok Pixel**: [TikTok for Business - Pixel](https://ads.tiktok.com/help/article?aid=10009432)
- **Express.js**: [Documentação Oficial](https://expressjs.com/)
- **React**: [Documentação Oficial](https://reactjs.org/)
- **Chart.js**: [Documentação Oficial](https://www.chartjs.org/)
- **React Chartjs 2**: [GitHub](https://github.com/reactchartjs/react-chartjs-2)
- **MongoDB**: [Documentação Oficial](https://docs.mongodb.com/)
- **Mongoose**: [Documentação Oficial](https://mongoosejs.com/)

## **9. Próximos Passos**

1. **Planejamento Detalhado**:
   - Defina claramente os requisitos da plataforma.
   - Crie wireframes e mockups da interface de usuário.

2. **Desenvolvimento Incremental**:
   - Comece implementando funcionalidades básicas e vá adicionando recursos gradualmente.
   - Teste cada componente antes de avançar para o próximo.

3. **Feedback e Iteração**:
   - Obtenha feedback dos usuários para melhorar a plataforma.
   - Itere sobre o design e as funcionalidades com base no feedback recebido.

4. **Escalabilidade**:
   - Planeje a arquitetura para suportar o crescimento futuro.
   - Considere otimizações de performance e escalabilidade desde o início.

Seguindo este guia, você estará bem encaminhado para desenvolver uma plataforma robusta que integra dados do Pixel do TikTok e oferece insights valiosos sobre suas campanhas publicitárias. Boa sorte no seu projeto!



Claro! Implementar uma funcionalidade que permite aos clientes pagar para ter múltiplos domínios associados às suas campanhas é uma excelente maneira de oferecer valor adicional e flexibilidade. Essa funcionalidade pode ser utilizada para diversos fins, como **balanceamento de carga**, **melhoria na entrega dos pixels de rastreamento** ou até mesmo **aumentar a resiliência** do serviço.

Neste guia, vamos implementar uma funcionalidade chamada **"Domínios Personalizados para Pixel"**, onde:

1. **Clientes podem adicionar múltiplos domínios** às suas campanhas.
2. **Pixels de rastreamento gerados** utilizarão aleatoriamente um dos domínios configurados.
3. **Esta funcionalidade será restrita** a clientes que tenham pago por ela.

Vamos implementar essa funcionalidade tanto no **backend** quanto no **frontend**, utilizando **TypeScript**, **Node.js**, **GraphQL**, **Apollo Server**, **Express**, **Mongoose** no backend e **React**, **Apollo Client** no frontend.

## **Índice**

1. [Visão Geral da Funcionalidade](#1-visão-geral-da-funcionalidade)
2. [Configuração do Backend](#2-configuração-do-backend)
    - [2.1. Atualizando o Modelo de Campanha](#21-atualizando-o-modelo-de-campanha)
    - [2.2. Atualizando o Schema GraphQL](#22-atualizando-o-schema-graphql)
    - [2.3. Implementando Resolvers para Domínios Personalizados](#23-implementando-resolvers-para-domínios-personalizados)
    - [2.4. Atualizando o Resolver de Geração de Pixel](#24-atualizando-o-resolver-de-geração-de-pixel)
3. [Configuração do Frontend](#3-configuração-do-frontend)
    - [3.1. Atualizando Types GraphQL no Frontend](#31-atualizando-types-graphql-no-frontend)
    - [3.2. Atualizando Queries e Mutations](#32-atualizando-queries-e-mutations)
    - [3.3. Criando Componentes para Gerenciar Domínios](#33-criando-componentes-para-gerenciar-domínios)
    - [3.4. Atualizando a Geração do Pixel](#34-atualizando-a-geração-do-pixel)
4. [Considerações de Segurança e Autorização](#4-considerações-de-segurança-e-autorização)
5. [Implementando a Restrição para Clientes Pagos](#5-implementando-a-restrição-para-clientes-pagos)
6. [Recursos e Referências Úteis](#6-recursos-e-referências-úteis)
7. [Conclusão](#7-conclusão)

---

## **1. Visão Geral da Funcionalidade**

A funcionalidade **"Domínios Personalizados para Pixel"** permitirá que os clientes adicionem múltiplos domínios às suas campanhas. Quando o script do pixel for gerado, ele selecionará aleatoriamente um dos domínios configurados para cada requisição, distribuindo a carga e aumentando a resiliência do serviço.

**Fluxo da Funcionalidade:**

1. **Cliente Adiciona Domínios:** O cliente adiciona um ou mais domínios à sua campanha.
2. **Gerar Pixel com Domínios Personalizados:** Ao gerar o script do pixel, ele incluirá um dos domínios configurados aleatoriamente.
3. **Rastreamento de Eventos:** O script do pixel utiliza o domínio escolhido para enviar eventos ao backend.
4. **Restrições:** Apenas clientes que pagaram pela funcionalidade podem adicionar múltiplos domínios.

---

## **2. Configuração do Backend**

### **2.1. Atualizando o Modelo de Campanha**

Primeiramente, precisamos atualizar o modelo de **Campaign** para incluir uma lista de **customDomains** e um indicador se a campanha possui essa funcionalidade ativa.

**Passos:**

1. **Atualizar o Modelo de Campanha (`src/models/Campaign.ts`):**

```typescript
import { Schema, model, Document } from 'mongoose';

interface IMetrics {
    impressions: number;
    clicks: number;
    conversions: number;
}

export interface ICampaign extends Document {
    name: string;
    tiktokCampaignId: string;
    pixelId: string;
    pixelSecret: string;
    metrics: IMetrics;
    customDomains: string[]; // Novidade
    isCustomDomainsEnabled: boolean; // Indica se a funcionalidade está ativa
    createdAt: Date;
}

const MetricsSchema = new Schema<IMetrics>({
    impressions: { type: Number, default: 0 },
    clicks: { type: Number, default: 0 },
    conversions: { type: Number, default: 0 },
}, { _id: false });

const CampaignSchema = new Schema<ICampaign>({
    name: { type: String, required: true },
    tiktokCampaignId: { type: String, required: true, unique: true },
    pixelId: { type: String, unique: true, required: true },
    pixelSecret: { type: String, unique: true, required: true },
    metrics: { type: MetricsSchema, default: () => ({}) },
    customDomains: { type: [String], default: [] }, // Inicializa como array vazio
    isCustomDomainsEnabled: { type: Boolean, default: false }, // Inicializa como falso
    createdAt: { type: Date, default: Date.now },
});

export default model<ICampaign>('Campaign', CampaignSchema);
```

2. **Migrar o Banco de Dados (Opcional):**

   Se você já possui campanhas existentes, pode ser necessário migrar os dados para incluir os novos campos. Utilize ferramentas como **Mongoose** para atualizar documentos ou scripts de migração conforme necessário.

### **2.2. Atualizando o Schema GraphQL**

Atualizamos o **schema GraphQL** para incluir os novos campos e mutations para gerenciar os domínios personalizados.

**Passos:**

1. **Atualizar o `src/schema/index.ts`:**

```typescript
import { gql } from 'apollo-server-express';

const typeDefs = gql`
    type Query {
        campaigns: [Campaign!]!
        campaign(id: ID!): Campaign
        userProfile: User
    }

    type Mutation {
        register(username: String!, email: String!, password: String!): AuthPayload
        login(email: String!, password: String!): AuthPayload
        createCampaign(name: String!, tiktokCampaignId: String!): Campaign!
        generatePixel(campaignId: ID!): PixelScript!
        addCustomDomain(campaignId: ID!, domain: String!): Campaign!
        removeCustomDomain(campaignId: ID!, domain: String!): Campaign!
        enableCustomDomains(campaignId: ID!): Campaign!
        disableCustomDomains(campaignId: ID!): Campaign!
    }

    type Subscription {
        campaignMetricsUpdated(campaignId: ID!): Metrics!
    }

    type Campaign {
        id: ID!
        name: String!
        tiktokCampaignId: String!
        pixelId: String!
        pixelSecret: String!
        metrics: Metrics!
        customDomains: [String!]! # Lista de domínios personalizados
        isCustomDomainsEnabled: Boolean! # Indica se a funcionalidade está ativa
        createdAt: String!
    }

    type Metrics {
        impressions: Int!
        clicks: Int!
        conversions: Int!
    }

    type User {
        id: ID!
        username: String!
        email: String!
        role: String!
    }

    type AuthPayload {
        token: String!
        user: User!
    }

    type PixelScript {
        script: String!
    }
`;

export default typeDefs;
```

### **2.3. Implementando Resolvers para Domínios Personalizados**

Criaremos resolvers para as novas mutations que permitem adicionar, remover e habilitar/desabilitar domínios personalizados.

**Passos:**

1. **Atualizar o `src/resolvers/index.ts`:**

```typescript
import { IResolvers } from '@apollo/server';
import bcrypt from 'bcryptjs';
import jwt from 'jsonwebtoken';
import Campaign, { ICampaign } from '../models/Campaign';
import User, { IUser } from '../models/User';
import { v4 as uuidv4 } from 'uuid';
import { PubSub } from 'graphql-subscriptions';

const pubsub = new PubSub();
const CAMPAIGN_METRICS_UPDATED = 'CAMPAIGN_METRICS_UPDATED';

interface AuthPayload {
    token: string;
    user: IUser;
}

interface Context {
    user?: {
        id: string;
        role: string;
    };
}

const resolvers: IResolvers = {
    Query: {
        campaigns: async (parent, args, context: Context): Promise<ICampaign[]> => {
            if (!context.user) throw new Error('Autenticação necessária');
            return await Campaign.find();
        },
        campaign: async (parent, { id }, context: Context): Promise<ICampaign | null> => {
            if (!context.user) throw new Error('Autenticação necessária');
            return await Campaign.findById(id);
        },
        userProfile: async (parent, args, context: Context): Promise<IUser | null> => {
            if (!context.user) throw new Error('Autenticação necessária');
            return await User.findById(context.user.id);
        },
    },
    Mutation: {
        register: async (parent, { username, email, password }): Promise<AuthPayload> => {
            const existingUser = await User.findOne({ email });
            if (existingUser) throw new Error('Usuário já existe com esse email');

            const hashedPassword = await bcrypt.hash(password, 10);
            const user = new User({ username, email, password: hashedPassword });
            await user.save();

            const token = jwt.sign({ id: user._id, role: user.role }, process.env.JWT_SECRET as string, { expiresIn: '1d' });

            return { token, user };
        },
        login: async (parent, { email, password }): Promise<AuthPayload> => {
            const user = await User.findOne({ email });
            if (!user) throw new Error('Usuário não encontrado');

            const valid = await bcrypt.compare(password, user.password);
            if (!valid) throw new Error('Senha incorreta');

            const token = jwt.sign({ id: user._id, role: user.role }, process.env.JWT_SECRET as string, { expiresIn: '1d' });

            return { token, user };
        },
        createCampaign: async (parent, { name, tiktokCampaignId }, context: Context): Promise<ICampaign> => {
            if (!context.user) throw new Error('Autenticação necessária');

            const existingCampaign = await Campaign.findOne({ tiktokCampaignId });
            if (existingCampaign) throw new Error('Campanha já existe com esse ID do TikTok');

            const campaign = new Campaign({
                name,
                tiktokCampaignId,
                pixelId: uuidv4(),
                pixelSecret: uuidv4(),
            });

            await campaign.save();
            return campaign;
        },
        generatePixel: async (parent, { campaignId }, context: Context): Promise<{ script: string }> => {
            if (!context.user) throw new Error('Autenticação necessária');

            const campaign = await Campaign.findById(campaignId);
            if (!campaign) throw new Error('Campanha não encontrada');

            let pixelEndpoint = process.env.PIXEL_ENDPOINT as string;

            // Se a campanha tiver domínios personalizados habilitados, selecione um aleatório
            if (campaign.isCustomDomainsEnabled && campaign.customDomains.length > 0) {
                const randomIndex = Math.floor(Math.random() * campaign.customDomains.length);
                const selectedDomain = campaign.customDomains[randomIndex];
                pixelEndpoint = `https://${selectedDomain}/api/pixel/event`;
            }

            const script = `
                (function() {
                    var pixelId = "${campaign.pixelId}";
                    var pixelSecret = "${campaign.pixelSecret}";
                    var endpoint = "${pixelEndpoint}";

                    function sendEvent(eventType, data) {
                        fetch(endpoint, {
                            method: "POST",
                            headers: {
                                "Content-Type": "application/json"
                            },
                            body: JSON.stringify({
                                pixelId: pixelId,
                                pixelSecret: pixelSecret,
                                event: eventType,
                                data: data,
                                timestamp: new Date().toISOString()
                            })
                        })
                        .then(response => response.json())
                        .then(data => {
                            console.log("Evento enviado com sucesso:", data);
                        })
                        .catch((error) => {
                            console.error("Erro ao enviar evento:", error);
                        });
                    }

                    // Rastrear finalização de compra
                    var originalFinalizePurchase = window.finalizePurchase;
                    window.finalizePurchase = function(orderData) {
                        sendEvent("purchase", orderData);
                        if (originalFinalizePurchase && typeof originalFinalizePurchase === "function") {
                            originalFinalizePurchase(orderData);
                        }
                    };

                    // Rastrear cliques em elementos com a classe 'track-click'
                    document.addEventListener('DOMContentLoaded', function() {
                        var trackableElements = document.querySelectorAll('.track-click');
                        trackableElements.forEach(function(element) {
                            element.addEventListener('click', function(event) {
                                var data = {
                                    elementId: event.target.id,
                                    elementClasses: event.target.className,
                                    elementText: event.target.innerText
                                };
                                sendEvent("click", data);
                            });
                        });
                    });
                })();
            `;

            return { script };
        },
        addCustomDomain: async (parent, { campaignId, domain }, context: Context): Promise<ICampaign> => {
            if (!context.user) throw new Error('Autenticação necessária');

            const campaign = await Campaign.findById(campaignId);
            if (!campaign) throw new Error('Campanha não encontrada');

            // Verifique se a funcionalidade está habilitada para a campanha
            if (!campaign.isCustomDomainsEnabled) {
                throw new Error('A funcionalidade de domínios personalizados não está habilitada para esta campanha');
            }

            // Verifique se o domínio já foi adicionado
            if (campaign.customDomains.includes(domain)) {
                throw new Error('Domínio já adicionado à campanha');
            }

            // Adicione o domínio
            campaign.customDomains.push(domain);
            await campaign.save();

            return campaign;
        },
        removeCustomDomain: async (parent, { campaignId, domain }, context: Context): Promise<ICampaign> => {
            if (!context.user) throw new Error('Autenticação necessária');

            const campaign = await Campaign.findById(campaignId);
            if (!campaign) throw new Error('Campanha não encontrada');

            // Verifique se o domínio existe na campanha
            if (!campaign.customDomains.includes(domain)) {
                throw new Error('Domínio não encontrado na campanha');
            }

            // Remova o domínio
            campaign.customDomains = campaign.customDomains.filter(d => d !== domain);
            await campaign.save();

            return campaign;
        },
        enableCustomDomains: async (parent, { campaignId }, context: Context): Promise<ICampaign> => {
            if (!context.user) throw new Error('Autenticação necessária');

            const campaign = await Campaign.findById(campaignId);
            if (!campaign) throw new Error('Campanha não encontrada');

            // Habilite a funcionalidade
            campaign.isCustomDomainsEnabled = true;
            await campaign.save();

            return campaign;
        },
        disableCustomDomains: async (parent, { campaignId }, context: Context): Promise<ICampaign> => {
            if (!context.user) throw new Error('Autenticação necessária');

            const campaign = await Campaign.findById(campaignId);
            if (!campaign) throw new Error('Campanha não encontrada');

            // Desabilite a funcionalidade e limpe os domínios personalizados
            campaign.isCustomDomainsEnabled = false;
            campaign.customDomains = [];
            await campaign.save();

            return campaign;
        },
    },
    Subscription: {
        campaignMetricsUpdated: {
            subscribe: async (parent, { campaignId }, context: Context) => {
                if (!context.user) throw new Error('Autenticação necessária');

                // Verifique se o usuário tem permissão para a campanha (opcional)

                return pubsub.asyncIterator(`${CAMPAIGN_METRICS_UPDATED}_${campaignId}`);
            },
        },
    },
};

export default resolvers;
```

**Explicações das Alterações:**

- **Campos Adicionados:**
    - `customDomains`: Lista de domínios personalizados associados à campanha.
    - `isCustomDomainsEnabled`: Indicador se a funcionalidade está ativa.

- **Mutations Implementadas:**
    - `addCustomDomain`: Adiciona um domínio personalizado à campanha.
    - `removeCustomDomain`: Remove um domínio personalizado da campanha.
    - `enableCustomDomains`: Habilita a funcionalidade de domínios personalizados para a campanha.
    - `disableCustomDomains`: Desabilita a funcionalidade e limpa os domínios personalizados.

- **Atualização no Resolver de Geração de Pixel:**
    - Se a campanha tiver domínios personalizados habilitados e possuir domínios na lista, seleciona aleatoriamente um domínio para construir o `pixelEndpoint`.

### **2.4. Atualizando o Resolver de Geração de Pixel**

No resolver de `generatePixel`, precisamos atualizar a lógica para selecionar um domínio aleatório se a funcionalidade estiver habilitada.

**Referência:**

Já atualizamos o resolver acima com essa lógica. Reforçando:

```typescript
generatePixel: async (parent, { campaignId }, context: Context): Promise<{ script: string }> => {
    if (!context.user) throw new Error('Autenticação necessária');

    const campaign = await Campaign.findById(campaignId);
    if (!campaign) throw new Error('Campanha não encontrada');

    let pixelEndpoint = process.env.PIXEL_ENDPOINT as string;

    // Se a campanha tiver domínios personalizados habilitados, selecione um aleatório
    if (campaign.isCustomDomainsEnabled && campaign.customDomains.length > 0) {
        const randomIndex = Math.floor(Math.random() * campaign.customDomains.length);
        const selectedDomain = campaign.customDomains[randomIndex];
        pixelEndpoint = `https://${selectedDomain}/api/pixel/event`;
    }

    const script = `
        (function() {
            var pixelId = "${campaign.pixelId}";
            var pixelSecret = "${campaign.pixelSecret}";
            var endpoint = "${pixelEndpoint}";

            function sendEvent(eventType, data) {
                fetch(endpoint, {
                    method: "POST",
                    headers: {
                        "Content-Type": "application/json"
                    },
                    body: JSON.stringify({
                        pixelId: pixelId,
                        pixelSecret: pixelSecret,
                        event: eventType,
                        data: data,
                        timestamp: new Date().toISOString()
                    })
                })
                .then(response => response.json())
                .then(data => {
                    console.log("Evento enviado com sucesso:", data);
                })
                .catch((error) => {
                    console.error("Erro ao enviar evento:", error);
                });
            }

            // Rastrear finalização de compra
            var originalFinalizePurchase = window.finalizePurchase;
            window.finalizePurchase = function(orderData) {
                sendEvent("purchase", orderData);
                if (originalFinalizePurchase && typeof originalFinalizePurchase === "function") {
                    originalFinalizePurchase(orderData);
                }
            };

            // Rastrear cliques em elementos com a classe 'track-click'
            document.addEventListener('DOMContentLoaded', function() {
                var trackableElements = document.querySelectorAll('.track-click');
                trackableElements.forEach(function(element) {
                    element.addEventListener('click', function(event) {
                        var data = {
                            elementId: event.target.id,
                            elementClasses: event.target.className,
                            elementText: event.target.innerText
                        };
                        sendEvent("click", data);
                    });
                });
            });
        })();
    `;

    return { script };
},
```

---

## **3. Configuração do Frontend**

### **3.1. Atualizando Types GraphQL no Frontend**

Atualize os tipos TypeScript no frontend para refletir os novos campos.

**Passos:**

1. **Atualizar `src/graphql/types.ts`:**

```typescript
// Tipos para o GraphQL
export interface User {
    id: string;
    username: string;
    email: string;
    role: string;
}

export interface AuthPayload {
    token: string;
    user: User;
}

export interface Metrics {
    impressions: number;
    clicks: number;
    conversions: number;
}

export interface Campaign {
    id: string;
    name: string;
    tiktokCampaignId: string;
    pixelId: string;
    pixelSecret: string;
    metrics: Metrics;
    customDomains: string[]; // Novidade
    isCustomDomainsEnabled: boolean; // Novidade
    createdAt: string;
}

export interface PixelScript {
    script: string;
}
```

### **3.2. Atualizando Queries e Mutations**

Adicione novas mutations para gerenciar domínios personalizados.

**Passos:**

1. **Atualizar `src/graphql/mutations.ts`:**

```typescript
import { gql } from '@apollo/client';
import { Campaign, AuthPayload, PixelScript, User } from './types';

export const REGISTER_USER = gql`
    mutation Register($username: String!, $email: String!, $password: String!) {
        register(username: $username, email: $email, password: $password) {
            token
            user {
                id
                username
                email
                role
            }
        }
    }
`;

export const LOGIN_USER = gql`
    mutation Login($email: String!, $password: String!) {
        login(email: $email, password: $password) {
            token
            user {
                id
                username
                email
                role
            }
        }
    }
`;

export const CREATE_CAMPAIGN = gql`
    mutation CreateCampaign($name: String!, $tiktokCampaignId: String!) {
        createCampaign(name: $name, tiktokCampaignId: $tiktokCampaignId) {
            id
            name
            tiktokCampaignId
            pixelId
            pixelSecret
            metrics {
                impressions
                clicks
                conversions
            }
            customDomains
            isCustomDomainsEnabled
            createdAt
        }
    }
`;

export const GENERATE_PIXEL = gql`
    mutation GeneratePixel($campaignId: ID!) {
        generatePixel(campaignId: $campaignId) {
            script
        }
    }
`;

// Novas mutations
export const ADD_CUSTOM_DOMAIN = gql`
    mutation AddCustomDomain($campaignId: ID!, $domain: String!) {
        addCustomDomain(campaignId: $campaignId, domain: $domain) {
            id
            customDomains
            isCustomDomainsEnabled
        }
    }
`;

export const REMOVE_CUSTOM_DOMAIN = gql`
    mutation RemoveCustomDomain($campaignId: ID!, $domain: String!) {
        removeCustomDomain(campaignId: $campaignId, domain: $domain) {
            id
            customDomains
            isCustomDomainsEnabled
        }
    }
`;

export const ENABLE_CUSTOM_DOMAINS = gql`
    mutation EnableCustomDomains($campaignId: ID!) {
        enableCustomDomains(campaignId: $campaignId) {
            id
            customDomains
            isCustomDomainsEnabled
        }
    }
`;

export const DISABLE_CUSTOM_DOMAINS = gql`
    mutation DisableCustomDomains($campaignId: ID!) {
        disableCustomDomains(campaignId: $campaignId) {
            id
            customDomains
            isCustomDomainsEnabled
        }
    }
`;
```

### **3.3. Criando Componentes para Gerenciar Domínios**

Crie componentes que permitam aos clientes adicionar, remover e habilitar/desabilitar domínios personalizados para suas campanhas.

**Passos:**

1. **Criar `src/components/Campaign/ManageCustomDomains.tsx`:**

```typescript
import React, { useState } from 'react';
import { useMutation } from '@apollo/client';
import { ADD_CUSTOM_DOMAIN, REMOVE_CUSTOM_DOMAIN, ENABLE_CUSTOM_DOMAINS, DISABLE_CUSTOM_DOMAINS } from '../../graphql/mutations';
import { Campaign } from '../../graphql/types';
import { TextField, Button, List, ListItem, ListItemText, IconButton, Typography, Switch, FormControlLabel, Alert } from '@mui/material';
import DeleteIcon from '@mui/icons-material/Delete';

interface ManageCustomDomainsProps {
    campaign: Campaign;
}

const ManageCustomDomains: React.FC<ManageCustomDomainsProps> = ({ campaign }) => {
    const [newDomain, setNewDomain] = useState<string>('');
    const [error, setError] = useState<string | null>(null);

    const [addCustomDomain] = useMutation(ADD_CUSTOM_DOMAIN, {
        onError: (err) => setError(err.message),
        onCompleted: () => setNewDomain(''),
    });

    const [removeCustomDomain] = useMutation(REMOVE_CUSTOM_DOMAIN, {
        onError: (err) => setError(err.message),
    });

    const [enableCustomDomains] = useMutation(ENABLE_CUSTOM_DOMAINS, {
        onError: (err) => setError(err.message),
    });

    const [disableCustomDomains] = useMutation(DISABLE_CUSTOM_DOMAINS, {
        onError: (err) => setError(err.message),
    });

    const handleAddDomain = () => {
        if (newDomain.trim() === '') {
            setError('O domínio não pode estar vazio.');
            return;
        }
        addCustomDomain({ variables: { campaignId: campaign.id, domain: newDomain.trim() } });
    };

    const handleRemoveDomain = (domain: string) => {
        removeCustomDomain({ variables: { campaignId: campaign.id, domain } });
    };

    const handleToggleCustomDomains = () => {
        if (campaign.isCustomDomainsEnabled) {
            disableCustomDomains({ variables: { campaignId: campaign.id } });
        } else {
            enableCustomDomains({ variables: { campaignId: campaign.id } });
        }
    };

    return (
        <div>
            <Typography variant="h6" gutterBottom>Domínios Personalizados</Typography>
            {error && <Alert severity="error">{error}</Alert>}
            <FormControlLabel
                control={
                    <Switch
                        checked={campaign.isCustomDomainsEnabled}
                        onChange={handleToggleCustomDomains}
                        color="primary"
                    />
                }
                label="Habilitar Domínios Personalizados"
            />
            {campaign.isCustomDomainsEnabled && (
                <>
                    <div style={{ display: 'flex', marginTop: '16px', marginBottom: '16px' }}>
                        <TextField
                            label="Novo Domínio"
                            value={newDomain}
                            onChange={(e) => setNewDomain(e.target.value)}
                            fullWidth
                        />
                        <Button variant="contained" color="primary" onClick={handleAddDomain} style={{ marginLeft: '8px' }}>
                            Adicionar
                        </Button>
                    </div>
                    <List>
                        {campaign.customDomains.map((domain) => (
                            <ListItem key={domain} secondaryAction={
                                <IconButton edge="end" aria-label="delete" onClick={() => handleRemoveDomain(domain)}>
                                    <DeleteIcon />
                                </IconButton>
                            }>
                                <ListItemText primary={domain} />
                            </ListItem>
                        ))}
                    </List>
                </>
            )}
        </div>
    );
};

export default ManageCustomDomains;
```

2. **Integrar o Componente no Detalhe da Campanha (`src/components/Campaign/CampaignDetail.tsx`):**

Atualize o componente de detalhe da campanha para incluir a gestão de domínios personalizados.

```typescript
import React, { useEffect, useState } from 'react';
import { useQuery, useSubscription } from '@apollo/client';
import { GET_CAMPAIGN } from '../../graphql/queries';
import { CAMPAIGN_METRICS_UPDATED } from '../../graphql/subscriptions';
import { useParams } from 'react-router-dom';
import { Typography, CircularProgress, Alert, Grid, Paper } from '@mui/material';
import { Bar, Pie } from 'react-chartjs-2';
import { Campaign, Metrics } from '../../graphql/types';
import ManageCustomDomains from './ManageCustomDomains';

interface GetCampaignData {
    campaign: Campaign;
}

interface GetCampaignVars {
    id: string;
}

interface MetricsUpdatedData {
    campaignMetricsUpdated: Metrics;
}

interface MetricsUpdatedVars {
    campaignId: string;
}

const CampaignDetail: React.FC = () => {
    const { id } = useParams<{ id: string }>();
    const { loading, error, data } = useQuery<GetCampaignData, GetCampaignVars>(GET_CAMPAIGN, { variables: { id: id! } });
    const [metrics, setMetrics] = useState<Metrics | null>(null);

    useEffect(() => {
        if (data && data.campaign) {
            setMetrics(data.campaign.metrics);
        }
    }, [data]);

    useSubscription<MetricsUpdatedData, MetricsUpdatedVars>(
        CAMPAIGN_METRICS_UPDATED,
        {
            variables: { campaignId: id! },
            onSubscriptionData: ({ subscriptionData }) => {
                if (subscriptionData.data) {
                    setMetrics(subscriptionData.data.campaignMetricsUpdated);
                }
            },
        }
    );

    if (loading) return <CircularProgress />;
    if (error) return <Alert severity="error">{error.message}</Alert>;

    const campaign = data?.campaign;

    if (!campaign) return <Alert severity="error">Campanha não encontrada</Alert>;

    if (!metrics) return <CircularProgress />;

    const barData = {
        labels: ['Impressões', 'Cliques', 'Conversões'],
        datasets: [
            {
                label: 'Métricas',
                data: [
                    metrics.impressions,
                    metrics.clicks,
                    metrics.conversions,
                ],
                backgroundColor: ['#36A2EB', '#FFCE56', '#FF6384'],
            },
        ],
    };

    const pieData = {
        labels: ['Impressões', 'Cliques', 'Conversões'],
        datasets: [
            {
                data: [
                    metrics.impressions,
                    metrics.clicks,
                    metrics.conversions,
                ],
                backgroundColor: ['#36A2EB', '#FFCE56', '#FF6384'],
            },
        ],
    };

    return (
        <div>
            <Typography variant="h4" gutterBottom>{campaign.name}</Typography>
            <Typography variant="subtitle1" gutterBottom>ID TikTok: {campaign.tiktokCampaignId}</Typography>
            <Grid container spacing={4}>
                <Grid item xs={12} md={6}>
                    <Paper sx={{ p: 2 }}>
                        <Typography variant="h6">Gráfico de Barras</Typography>
                        <Bar data={barData} />
                    </Paper>
                </Grid>
                <Grid item xs={12} md={6}>
                    <Paper sx={{ p: 2 }}>
                        <Typography variant="h6">Gráfico de Pizza</Typography>
                        <Pie data={pieData} />
                    </Paper>
                </Grid>
                <Grid item xs={12}>
                    <Paper sx={{ p: 2 }}>
                        <ManageCustomDomains campaign={campaign} />
                    </Paper>
                </Grid>
            </Grid>
        </div>
    );
};

export default CampaignDetail;
```

**Explicação:**

- **ManageCustomDomains Component:** Este componente permite que os clientes habilitem a funcionalidade de domínios personalizados, adicionem novos domínios, e removam domínios existentes.
- **Habilitar/Desabilitar:** Um switch (`FormControlLabel` com `Switch`) permite que o cliente habilite ou desabilite a funcionalidade. Ao habilitar, os campos para adicionar domínios aparecem.

### **3.4. Atualizando a Geração do Pixel**

Atualize o componente de geração do pixel para refletir a seleção de domínios personalizados.

**Passos:**

1. **Atualizar `src/components/Campaign/CreateCampaign.tsx`:**

Adicione um botão para habilitar a funcionalidade de domínios personalizados após a criação da campanha.

```typescript
import React, { useState } from 'react';
import { useMutation } from '@apollo/client';
import { CREATE_CAMPAIGN, GENERATE_PIXEL, ENABLE_CUSTOM_DOMAINS } from '../../graphql/mutations';
import { useNavigate } from 'react-router-dom';
import { TextField, Button, Container, Typography, Alert, Dialog, DialogTitle, DialogContent, DialogActions } from '@mui/material';
import { Campaign, PixelScript } from '../../graphql/types';

interface CreateCampaignData {
    createCampaign: Campaign;
}

interface GeneratePixelData {
    generatePixel: PixelScript;
}

const CreateCampaign: React.FC = () => {
    const [form, setForm] = useState({ name: '', tiktokCampaignId: '' });
    const [error, setError] = useState<string | null>(null);
    const navigate = useNavigate();
    const [open, setOpen] = useState(false);
    const [pixelScript, setPixelScript] = useState<string>('');

    const [createCampaign, { loading: creating }] = useMutation<CreateCampaignData>(CREATE_CAMPAIGN, {
        onCompleted: (data) => {
            enableCustomDomains({ variables: { campaignId: data.createCampaign.id } });
            generatePixel({ variables: { campaignId: data.createCampaign.id } });
        },
        onError: (err) => setError(err.message),
    });

    const [generatePixel, { loading: generating }] = useMutation<GeneratePixelData>(GENERATE_PIXEL, {
        onCompleted: (data) => {
            setPixelScript(data.generatePixel.script);
            setOpen(true);
        },
        onError: (err) => setError(err.message),
    });

    const [enableCustomDomains] = useMutation(ENABLE_CUSTOM_DOMAINS, {
        onError: (err) => setError(err.message),
    });

    const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
        setForm({ ...form, [e.target.name]: e.target.value });
    };

    const handleSubmit = async (e: React.FormEvent) => {
        e.preventDefault();
        setError(null);
        try {
            await createCampaign({ variables: form });
        } catch (err) {
            // Erros são tratados em onError
        }
    };

    const handleClose = () => {
        setOpen(false);
        navigate('/');
    };

    return (
        <Container maxWidth="sm" sx={{ mt: 4 }}>
            <Typography variant="h4" gutterBottom>Nova Campanha</Typography>
            {error && <Alert severity="error">{error}</Alert>}
            <form onSubmit={handleSubmit}>
                <TextField
                    label="Nome da Campanha"
                    name="name"
                    value={form.name}
                    onChange={handleChange}
                    fullWidth
                    margin="normal"
                    required
                />
                <TextField
                    label="ID da Campanha no TikTok"
                    name="tiktokCampaignId"
                    value={form.tiktokCampaignId}
                    onChange={handleChange}
                    fullWidth
                    margin="normal"
                    required
                />
                <Button type="submit" variant="contained" color="primary" fullWidth disabled={creating || generating} sx={{ mt: 2 }}>
                    {creating ? 'Criando...' : 'Criar'}
                </Button>
            </form>

            {/* Diálogo para exibir o script do pixel */}
            <Dialog open={open} onClose={handleClose} maxWidth="md" fullWidth>
                <DialogTitle>Script do Pixel Gerado</DialogTitle>
                <DialogContent>
                    <Typography variant="body1" gutterBottom>
                        Copie e cole o seguinte script no seu site, antes da tag de fechamento <code>&lt;/head&gt;</code>:
                    </Typography>
                    <textarea
                        readOnly
                        value={pixelScript}
                        style={{ width: '100%', height: '200px' }}
                    />
                </DialogContent>
                <DialogActions>
                    <Button onClick={handleClose} color="primary">Fechar</Button>
                </DialogActions>
            </Dialog>
        </Container>
    );
};

export default CreateCampaign;
```

**Explicação das Alterações:**

- **Habilitar Domínios Personalizados:** Após criar a campanha, automaticamente habilitamos a funcionalidade de domínios personalizados para essa campanha utilizando a mutation `enableCustomDomains`.
- **Geração do Pixel:** Gera o script do pixel e exibe no diálogo.

---

## **4. Considerações de Segurança e Autorização**

### **4.1. Autorização nas Mutations**

Garantir que apenas usuários autorizados possam gerenciar domínios personalizados.

**Passos:**

1. **Verifique se o usuário tem permissão para gerenciar a campanha:**

   Você pode implementar verificações adicionais nos resolvers para assegurar que apenas o proprietário da campanha ou um administrador possa adicionar/remover domínios.

2. **Exemplo de Verificação no Resolver:**

```typescript
// No Mutation resolvers
addCustomDomain: async (parent, { campaignId, domain }, context: Context): Promise<ICampaign> => {
    if (!context.user) throw new Error('Autenticação necessária');

    const campaign = await Campaign.findById(campaignId);
    if (!campaign) throw new Error('Campanha não encontrada');

    // Verifique se o usuário é o proprietário da campanha ou admin
    if (campaign.ownerId.toString() !== context.user.id && context.user.role !== 'admin') {
        throw new Error('Permissão negada');
    }

    // Restante da lógica...
},
```

**Nota:** Assegure-se de que o modelo `Campaign` inclui um campo `ownerId` que referencia o usuário que criou a campanha.

### **4.2. Validação de Domínios**

Antes de adicionar um domínio personalizado, é crucial validar que o domínio é válido e pertence ao cliente.

**Passos:**

1. **Verificar Formato do Domínio:**

   Utilize expressões regulares ou bibliotecas como **validator.js** para validar o formato do domínio.

2. **Verificar Propriedade do Domínio:**

   Uma abordagem comum é solicitar que o cliente adicione um registro TXT ao DNS do domínio para verificar a propriedade.

   **Passos:**

   - **Gerar um Token de Verificação:** Ao adicionar um domínio, gere um token único.
   - **Solicitar que o Cliente Adicione um Registro TXT com o Token:**
     - Instruções claras para o cliente sobre como adicionar o registro TXT.
   - **Verificar a Presença do Registro TXT:**
     - Utilize bibliotecas como **dns.promises** para consultar o DNS do domínio e verificar se o token está presente.

**Exemplo de Implementação:**

```typescript
import { Resolver, Mutation, Arg, Ctx, Authorized } from 'type-graphql';
import { Campaign } from '../models/Campaign';
import { Context } from '../types/Context';
import validator from 'validator';
import dns from 'dns/promises';

@Resolver()
export class CampaignResolver {
    @Mutation(() => Campaign)
    @Authorized()
    async addCustomDomain(
        @Arg('campaignId') campaignId: string,
        @Arg('domain') domain: string,
        @Ctx() context: Context
    ): Promise<Campaign> {
        // Validação do formato do domínio
        if (!validator.isFQDN(domain)) {
            throw new Error('Formato de domínio inválido');
        }

        // Verificar propriedade do domínio
        const verificationToken = `verify-${uuidv4()}`;
        // Armazene o token no banco de dados temporariamente associado ao domínio

        // Envie instruções ao cliente para adicionar o registro TXT:
        // Exemplo: "Adicione o registro TXT 'verify-<token>' ao DNS do seu domínio."

        // Após o cliente adicionar o registro, verifique:
        try {
            const records = await dns.resolveTxt(domain);
            const flatRecords = records.flat();
            if (!flatRecords.includes(verificationToken)) {
                throw new Error('Domínio não verificado. Por favor, adicione o registro TXT de verificação.');
            }
        } catch (error) {
            throw new Error('Erro ao verificar domínio: ' + error.message);
        }

        // Se verificado, adicione o domínio à campanha
        const campaign = await Campaign.findById(campaignId);
        if (!campaign) throw new Error('Campanha não encontrada');

        // Verifique se o usuário é o proprietário ou admin
        if (campaign.ownerId.toString() !== context.user.id && context.user.role !== 'admin') {
            throw new Error('Permissão negada');
        }

        // Adicione o domínio
        if (campaign.customDomains.includes(domain)) {
            throw new Error('Domínio já adicionado');
        }

        campaign.customDomains.push(domain);
        await campaign.save();

        return campaign;
    }
}
```

**Nota:** Este é um exemplo simplificado. Em um cenário real, você precisará gerenciar o armazenamento e a expiração dos tokens de verificação, além de automatizar a verificação após a confirmação do cliente.

### **4.3. Proteção Contra Abusos**

- **Rate Limiting:** Continuar utilizando `express-rate-limit` para proteger endpoints públicos.
- **Monitoramento e Logs:** Implementar logs detalhados para monitorar atividades suspeitas.
- **Limites de Domínios:** Impor um limite no número de domínios que um cliente pode adicionar para evitar abusos.

---

## **5. Implementando a Restrição para Clientes Pagos**

Para restringir o uso da funcionalidade **"Domínios Personalizados para Pixel"** apenas a clientes que pagaram por ela, precisamos implementar um sistema de **permissões** baseado em **assinaturas** ou **planos**.

### **5.1. Atualizando o Modelo de Usuário**

Adicione campos ao modelo de **User** para gerenciar as assinaturas ou permissões.

**Passos:**

1. **Atualizar `src/models/User.ts`:**

```typescript
import { Schema, model, Document } from 'mongoose';

export interface IUser extends Document {
    username: string;
    email: string;
    password: string;
    role: 'admin' | 'user';
    hasPremium: boolean; // Indica se o usuário possui uma assinatura premium
}

const UserSchema = new Schema<IUser>({
    username: { type: String, required: true, unique: true },
    email: { type: String, required: true, unique: true },
    password: { type: String, required: true },
    role: { type: String, enum: ['admin', 'user'], default: 'user' },
    hasPremium: { type: Boolean, default: false }, // Inicializa como falso
}, { timestamps: true });

export default model<IUser>('User', UserSchema);
```

### **5.2. Implementando Autorização Baseada em Permissões**

Atualize as **mutations** que gerenciam domínios personalizados para verificar se o usuário possui permissão.

**Passos:**

1. **Atualizar Resolvers para Verificar `hasPremium`:**

```typescript
addCustomDomain: async (parent, { campaignId, domain }, context: Context): Promise<ICampaign> => {
    if (!context.user) throw new Error('Autenticação necessária');

    const user = await User.findById(context.user.id);
    if (!user) throw new Error('Usuário não encontrado');

    if (!user.hasPremium) {
        throw new Error('Esta funcionalidade está disponível apenas para clientes premium');
    }

    const campaign = await Campaign.findById(campaignId);
    if (!campaign) throw new Error('Campanha não encontrada');

    // Verifique se o usuário é o proprietário da campanha ou admin
    if (campaign.ownerId.toString() !== context.user.id && context.user.role !== 'admin') {
        throw new Error('Permissão negada');
    }

    // Restante da lógica...
},
```

**Repita essa verificação para todas as mutations relacionadas a domínios personalizados.**

### **5.3. Gerenciando Assinaturas**

Implemente um sistema para que os usuários possam se inscrever em planos premium. Isso geralmente envolve:

1. **Integração com um Serviço de Pagamento:** Utilize serviços como **Stripe**, **PayPal**, etc., para gerenciar pagamentos e assinaturas.
2. **Atualizar o Status `hasPremium`:** Após a confirmação do pagamento, atualize o campo `hasPremium` do usuário para `true`.
3. **Gerenciar Renovação e Cancelamento:** Implemente lógica para renovar assinaturas automaticamente ou permitir que os usuários cancelem.

**Nota:** A implementação completa de um sistema de pagamento está fora do escopo deste guia, mas você pode encontrar documentação detalhada nos sites dos provedores de pagamento.

---

## **6. Recursos e Referências Úteis**

- **GraphQL Subscriptions:**
  - [GraphQL Subscriptions](https://graphql.org/blog/subscriptions-in-graphql-and-relay/)
  - [Apollo GraphQL Subscriptions](https://www.apollographql.com/docs/apollo-server/data/subscriptions/)
- **Validação de Domínios:**
  - [validator.js](https://github.com/validatorjs/validator.js)
- **Verificação de Propriedade de Domínio:**
  - [Node.js DNS Module](https://nodejs.org/api/dns.html)
- **Express Rate Limit:**
  - [express-rate-limit GitHub](https://github.com/nfriedly/express-rate-limit)
- **Gerenciamento de Assinaturas com Stripe:**
  - [Stripe Subscriptions Documentation](https://stripe.com/docs/billing/subscriptions)
- **TypeScript e Apollo:**
  - [TypeScript with Apollo](https://www.apollographql.com/docs/react/development-testing/typescript/)
- **Mongoose com TypeScript:**
  - [Mongoose TypeScript](https://mongoosejs.com/docs/typescript.html)
- **Material-UI (MUI):**
  - [MUI Documentation](https://mui.com/)

---

## **7. Conclusão**

A implementação da funcionalidade **"Domínios Personalizados para Pixel"** oferece aos seus clientes uma maneira flexível e poderosa de gerenciar os pixels de rastreamento, além de adicionar uma fonte adicional de receita através de assinaturas premium.

**Principais Benefícios:**

- **Flexibilidade:** Clientes podem distribuir a carga dos pixels de rastreamento através de múltiplos domínios.
- **Resiliência:** A utilização de múltiplos domínios aumenta a resiliência do serviço contra falhas ou bloqueios.
- **Monetização:** Ofereça funcionalidades avançadas como parte de um plano premium, aumentando o valor da sua plataforma.

**Próximos Passos Sugeridos:**

1. **Implementar Verificações de Propriedade de Domínio:**
   - Automatize a verificação de domínios para garantir que os clientes realmente possuam os domínios que estão adicionando.

2. **Integração Completa com Sistema de Pagamento:**
   - Permita que os clientes se inscrevam em planos premium diretamente através da sua plataforma.

3. **Testes e Validação:**
   - Realize testes abrangentes para garantir que a funcionalidade está funcionando corretamente em diferentes cenários.

4. **Documentação e Suporte ao Cliente:**
   - Forneça documentação detalhada e suporte para ajudar os clientes a utilizarem a nova funcionalidade de forma eficaz.

5. **Monitoramento e Logs:**
   - Implemente monitoramento robusto para rastrear o uso dos domínios personalizados e detectar possíveis abusos ou falhas.

**Finalizando**, esta funcionalidade não apenas enriquece sua plataforma, mas também cria oportunidades para escalar e oferecer mais valor aos seus clientes. Continue iterando e aprimorando com base no feedback dos usuários e nas melhores práticas de desenvolvimento.

Se tiver dúvidas específicas ou precisar de assistência adicional durante a implementação, sinta-se à vontade para perguntar!

Boa sorte no desenvolvimento da sua plataforma!