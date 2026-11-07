# 📦 Exemplo API - 2º ADS

API RESTful completa desenvolvida para a disciplina de Programação / Desenvolvimento de Sistemas (2º semestre de Análise e Desenvolvimento de Sistemas). O projeto implementa um sistema de gestão de **Produtos**, **Clientes** e **Pedidos**, seguindo uma arquitetura em camadas (**Controller - Service - Repository**) e boas práticas de desenvolvimento.

---

## 🚀 Tecnologias Utilizadas

- **[Node.js](https://nodejs.org/)** (v18+)
- **[Express 5](https://expressjs.com/)** — Framework web moderno e robusto
- **[TypeScript](https://www.typescriptlang.org/)** — Tipagem estática e segurança em tempo de desenvolvimento
- **[SQLite3](https://www.sqlite.org/)** — Banco de dados relacional leve em arquivo local (`database.db`)
- **[Zod](https://zod.dev/)** — Validação e inferência de esquemas de dados em tempo de execução
- **[tsx](https://github.com/privatenumber/tsx)** — Execução e hot-reload ágil de TypeScript sem etapa prévia de build

---

## 🏛️ Arquitetura do Projeto

A aplicação está estruturada no padrão em três camadas, garantindo baixo acoplamento e separação de responsabilidades:

```text
exemplo_api_2ads/
├── database.db             # Banco de dados SQLite (gerado automaticamente)
├── requests.http           # Arquivo de testes para a extensão REST Client
├── package.json            # Dependências e scripts do projeto
├── tsconfig.json           # Configurações do TypeScript
└── src/
    ├── app.ts              # Configuração do Express e middlewares
    ├── server.ts           # Inicialização do servidor HTTP
    ├── config/
    │   └── database.ts     # Conexão e configuração do SQLite (PRAGMA foreign_keys = ON)
    ├── controllers/        # Recepção das requisições e respostas HTTP
    ├── database/
    │   ├── init.ts         # Script para criar as tabelas
    │   ├── schema.sql      # DDL SQL (tabelas, chaves e constraints)
    │   └── seed.ts         # Script para popular dados de teste
    ├── errors/             # Classes de erros customizados (AppError)
    ├── middlewares/        # Tratamento global de erros (Zod e AppError)
    ├── models/             # Tipagens e interfaces das entidades
    ├── repositories/       # Comunicação direta e queries com o banco de dados
    ├── routes/             # Definição dos endpoints da API
    ├── schemas/            # Schemas de validação de dados com Zod
    └── services/           # Regras de negócio da aplicação
```

---

## ⚙️ Pré-requisitos

- [Node.js](https://nodejs.org/) instalado na máquina.
- Gerenciador de pacotes `npm` (incluso com o Node.js).
- *(Opcional)* Extensão **REST Client** no VS Code para executar o arquivo `requests.http`.

---

## 🛠️ Instalação e Execução

### 1. Clonar o repositório

```bash
git clone https://github.com/Sil3lipe/exemplo_api_2ads.git
cd exemplo_api_2ads
```

### 2. Instalar as dependências

```bash
npm install
```

### 3. Inicializar o Banco de Dados

Execute o script para criar o arquivo `database.db` e as tabelas necessárias:

```bash
npm run db:init
```

### 4. Popular o Banco com Dados Iniciais (Opcional)

Para inserir clientes e produtos padrão para testes:

```bash
npm run db:seed
```

### 5. Iniciar o Servidor em Desenvolvimento

```bash
npm run dev
```

A API estará rodando em: `http://localhost:3000`

---

## 📜 Scripts Disponíveis

| Comando | Descrição |
| :--- | :--- |
| `npm run dev` | Inicia o servidor com hot-reload usando `tsx watch` |
| `npm run build` | Compila os arquivos TypeScript para a pasta `dist/` |
| `npm start` | Inicia a aplicação a partir do código compilado |
| `npm run db:init` | Executa o script que cria as tabelas do `schema.sql` no SQLite |
| `npm run db:seed` | Popula o banco com dados de exemplo iniciais |

---

## 🛣️ Rotas da API

### 🛍️ Produtos (`/products`)

| Método | Rota | Descrição | Status Sucesso |
| :--- | :--- | :--- | :--- |
| `GET` | `/products` | Lista todos os produtos | `200 OK` |
| `GET` | `/products/:id` | Retorna um produto pelo ID | `200 OK` |
| `POST` | `/products` | Cria um novo produto | `201 Created` |
| `PUT` | `/products/:id` | Atualiza dados de um produto | `200 OK` |
| `DELETE` | `/products/:id` | Remove um produto (se não estiver em pedidos) | `204 No Content` |

#### Exemplo de Payload para Criar Produto (`POST /products`)

```json
{
  "name": "Arroz Integral 1kg",
  "description": "Arroz integral tipo 1",
  "price": 8.50
}
```

---

### 👤 Clientes (`/customers`)

| Método | Rota | Descrição | Status Sucesso |
| :--- | :--- | :--- | :--- |
| `GET` | `/customers` | Lista todos os clientes | `200 OK` |
| `GET` | `/customers/:id` | Retorna um cliente pelo ID | `200 OK` |
| `POST` | `/customers` | Cria um novo cliente | `201 Created` |
| `PUT` | `/customers/:id` | Atualiza dados de um cliente | `200 OK` |
| `GET` | `/customers/:id/orders` | Lista os pedidos realizados pelo cliente | `200 OK` |
| `DELETE` | `/customers/:id` | Remove um cliente | `204 No Content` |

#### Exemplo de Payload para Criar Cliente (`POST /customers`)

```json
{
  "name": "Maria Silva",
  "email": "maria@email.com",
  "phone": "11988887777"
}
```

---

### 📑 Pedidos (`/orders`)

| Método | Rota | Descrição | Status Sucesso |
| :--- | :--- | :--- | :--- |
| `GET` | `/orders` | Lista todos os pedidos | `200 OK` |
| `GET` | `/orders/:id` | Retorna um pedido específico com seus itens | `200 OK` |
| `POST` | `/orders` | Cria um novo pedido calculando o total | `201 Created` |

#### Exemplo de Payload para Criar Pedido (`POST /orders`)

```json
{
  "customer_id": 1,
  "items": [
    { "product_id": 1, "quantity": 2 },
    { "product_id": 2, "quantity": 1 }
  ]
}
```

---

## 🧪 Testando com o arquivo `requests.http`

O projeto inclui o arquivo [`requests.http`](./requests.http) pronto para uso com a extensão **REST Client** do VS Code:

1. Instale a extensão **REST Client** (`humao.rest-client`) no VS Code.
2. Abra o arquivo `requests.http`.
3. Clique em **Send Request** acima de cada requisição para testar os endpoints diretamente sem precisar de Postman ou Insomnia.

---

## 🔒 Regras de Negócio e Tratamento de Erros

A API possui validação estrita com Zod e tratamento centralizado de erros:

- **400 Bad Request:** Retornado quando os dados enviados não cumprem os tipos ou campos obrigatórios especificados pelo Zod.
- **404 Not Found:** Retornado quando um ID solicitado (produto, cliente ou pedido) não existe no banco.
- **409 Conflict:**
  - Tentativa de cadastrar cliente com e-mail já existente.
  - Tentativa de deletar um produto associado a um pedido existente (**RN07**).
- **500 Internal Server Error:** Erros inesperados de infraestrutura ou banco de dados.

---

## 👨‍💻 Autor

Desenvolvido por **Filipe Matos Silva**  
GitHub: [@Sil3lipe](https://github.com/Sil3lipe)
