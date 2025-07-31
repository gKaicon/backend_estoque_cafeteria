-----

# API - Estoque da Cafeteria

API RESTful desenvolvida em Laravel para o sistema de gerenciamento de estoque e vendas de uma cafeteria.

## ✨ Features

  * 🚀 Autenticação segura de usuários utilizando JWT (JSON Web Tokens).
  * 📦 Gerenciamento completo de **Produtos** (CRUD - Criar, Ler, Atualizar, Deletar).
  * 🛒 Gerenciamento de **Vendas** e seus itens.
  * 🚚 Gerenciamento de **Compras** de fornecedores e seus itens.
  * 📊 Atualização automática de estoque baseada nas operações de Compra e Venda.

## 🔧 Pré-requisitos

Antes de começar, garanta que você tem o seguinte ambiente configurado:

  * PHP \>= 8.2
  * Composer 2
  * Node.js e NPM
  * Um SGBD como MySQL, PostgreSQL ou SQLite.

## ⚙️ Instalação e Configuração

Siga os passos abaixo para configurar o ambiente de desenvolvimento local.

1.  **Clone o repositório:**

    ```bash
    git clone https://github.com/seu-usuario/sua-api-cafeteria.git
    cd sua-api-cafeteria
    ```

2.  **Instale as dependências:**

    ```bash
    composer install
    npm install
    ```

3.  **Configure o arquivo de ambiente:**

    ```bash
    cp .env.example .env
    ```

    Depois, abra o arquivo `.env` e configure suas variáveis de ambiente, principalmente a conexão com o banco de dados (`DB_*`).

4.  **Gere a chave da aplicação:**

    ```bash
    php artisan key:generate
    ```

5.  **Gere a chave secreta do JWT:**
    Este passo é **crucial** para a autenticação funcionar.

    ```bash
    php artisan jwt:secret
    ```

6.  **Execute as migrações do banco de dados:**
    Este comando criará todas as tabelas necessárias.

    ```bash
    php artisan migrate
    ```

7.  **(Opcional) Popule o banco com dados de teste:**
    Se você criou `Seeders`, rode o comando abaixo.

    ```bash
    php artisan db:seed
    ```

## ▶️ Executando a Aplicação

Para iniciar o servidor de desenvolvimento local, execute:

```bash
php artisan serve
```

A API estará disponível em `http://127.0.0.1:8000`.

## Endpoints da API

Todas as rotas protegidas exigem um token JWT no cabeçalho da requisição:
`Authorization: Bearer {seu_token}`

-----

### 🔑 Autenticação

  * **`POST /api/auth/login`**

      * **Descrição:** Autentica um usuário e retorna um token JWT.
      * **Corpo:** `{ "email": "user@example.com", "password": "password" }`

  * **`POST /api/auth/register`**

      * **Descrição:** Registra um novo usuário.
      * **Corpo:** `{ "name": "Nome do Usuário", "email": "user@example.com", "password": "password", "password_confirmation": "password" }`

  * **`POST /api/auth/logout`**

      * **Descrição:** Invalida o token do usuário autenticado. (Rota protegida)


### 📦 Produtos

  * **`GET /api/products`**: Lista todos os produtos.
  * **`POST /api/products`**: Cria um novo produto.
      * **Corpo:** `{ "name": "Café Especial", "description": "Grãos de Minas", "price": 25.50, "amount": 100 }`
  * **`GET /api/products/{id}`**: Exibe um produto específico.
  * **`PUT /api/products/{id}`**: Atualiza um produto.
  * **`DELETE /api/products/{id}`**: Deleta um produto.

*(Todas as rotas de produtos são protegidas)*

-----

### 🛒 Vendas

  * **`GET /api/sells`**: Lista todas as vendas.
  * **`POST /api/sells`**: Cria um novo registro de venda (inicia uma venda).
  * **`GET /api/sells/{id}`**: Exibe uma venda específica com seus itens.
  * **`POST /api/sells/{saleId}/items`**: Adiciona um produto a uma venda. A quantidade em estoque do produto é **diminuída** automaticamente.
      * **Corpo:** `{ "product_id": 1, "quantity": 2}`
  * **`DELETE /api/sells/{saleId}/items/{itemId}`**: Remove um item de uma venda. A quantidade em estoque do produto é **devolvida** automaticamente.

*(Todas as rotas de vendas são protegidas)*

-----

### 🚚 Compras (Entrada de Estoque)

  * **`GET /api/buys`**: Lista todas as compras de fornecedores.
  * **`POST /api/buys`**: Cria um novo registro de compra.
  * **`GET /api/buys/{id}`**: Exibe uma compra específica com seus itens.
  * **`POST /api/buys/{purchaseId}/items`**: Adiciona um produto a uma compra. A quantidade em estoque do produto é **aumentada** automaticamente.
      * **Corpo:** `{ "product_id": 1, "quantity": 50}`
  * **`DELETE /api/buys/{purchaseId}/items/{itemId}`**: Remove um item de uma compra. A quantidade em estoque do produto é **revertida** automaticamente.

*(Todas as rotas de compras são protegidas)*
