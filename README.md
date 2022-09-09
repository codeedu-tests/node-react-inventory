## Descrição

O projeto consiste na criação de um sistema de estoque de produtos

## Tecnologias e requisitos

* Back-end - Node.js com TypeScript (Pode ser usado qualquer lib ou framework)
* Front-end - React.js com TypeScript (Pode ser usado Create React App ou Next.js)
* Docker para orquestar o ambiente da aplicação

## Contexto do sistema

Usuários poderão cadastrar produtos e dar entrada/saída no estoque. O cadastro de produtos serão categorizados.
Somente usuários autenticados poderão manipular o sistema.

## Design do sistema

### Back-end - Node.js com TypeScript

Você deve criar uma API Rest que terá os seguintes endpoints:

* POST /auth - Gera um token JWT para autenticação (usuário e senha deverão ser fixo: admin@user.com e 123456. Se a credential não for valida, a API deverá retornar um erro. O tempo de vida de um token será de 5 min).
```json
{
    "email": "admin@user.com",
    "password": "123456"
}
```
* POST /auth/refresh - Renova o token de acesso atual por mais 5 min, será gerado um novo token. Se o token de acesso não for válido, a API deverá retornar um erro.
```json
{
    "access_token": "xxxxx.yyyyy.zzzzz"
}
```
* GET /categories - Listagem de categorias
```json
[
    {
        "id": 1,
        "name": "Category 1"
    },
    {
        "id": 2,
        "name": "Category 2"
    }
]
```
* POST /categories - Criação de categorias (se o nome não for passado, a API deverá informar um erro)
```json
{
    "name": "Awesome category"
}
```
* POST /products - Criação de um produto (se o nome não for passado ou a categoria não existir, a API deverá informar um erro)
```json
{
    "name": "Foo product",
    "category_id": 123
}
```
* GET /products?page=1 - Listar produtos (A listagem de produtos terá paginação com no máximo 15 registros por página)
```json
{
    "data": [
       {
            "id": 1,
            "name": "Product 1",
            "quantity": 120,
            "category_id": 1,
            "category_name": "Categoria 1"
        },
        {
            "id": 1,
            "name": "Product 2",
            "quantity": 60,
            "category_id": 2,
            "category_name": "Categoria 2"
        }
    ],
    "meta": {
        "page": 1
    }
}
```
* GET /products/:id - Mostrar um produto (Se o produto não existir, a API deve retornar um erro)
```json
{
    "id": 1,
    "name": "Product 1",
    "quantity": 120,
    "category_id": 1,
    "category_name": "Categoria 1"
}
```
* POST /inventory/add - Dar entrada de mais quantidades de um produto no estoque
```json
{
    "product_id": 1,
    "quantity": 1,
}
```
* POST /inventory/sub - Dar saída de mais quantidades de um produto no estoque (Se o produto não existir ou se o estoque do produto menos a quantidade a ser retirada for < 0, a API deve retornar um erro)
```json
{
    "product_id": 2,
    "quantity": 1,
}
```

Sempre quando uma entrada ou saída for realizada, deve-se registrar isto e também atualizar o estoque do produto.

Crie dados falsos para categorias e produtos. Crie pelo menos 5 categorias e 150 produtos já previamente cadastrados no banco de dados.

Criar a aplicação usando testes automatizados será um grande diferencial.

### Front-end - React.js

O front-end da aplicação deverá ter as seguintes páginas:

* Página de login de usuário - Somente usuários logados deverão ter acesso interno à aplicação. Quando o login acontecer, o token deverá ser armazenado no browser e a cada requisição HTTP para a API, se retornar um erro 401, será usado o endpoint de renovação do token para gerar um novo token e automaticamente deverá ser enviado a requisição novamente.
* Listar as categorias
* Cadastrar uma categoria - Antes de enviar os dados do formulário, estes deverão ser validados
* Listar os produtos - Esta listagem deverá se integrar com a paginação do backend e deverá usar algum cache local para que produtos já listados anteriormente (paginações de produtos já listadas) continuem na memória do browser e não seja chamada novamente a API Rest para capturar os produtos.
* Cadastrar um produto - Antes de enviar os dados do formulário, estes deverão ser validados
* Dar entrada no estoque
* Dar saída no estoque

Fique a vontade para organizar páginas ou modais e até usar libs de estilização para React.js. A componetização e composição serão muito
bem-vindos.

Criar a aplicação usando pirâmides de testes será muito bem vindo.

## Docker

Crie as duas aplicações montando-as com Docker de forma que ao fazer `docker-compose up` seja possível testar todo o ambiente. 
O Docker deve levantar back-end, front-end, banco de dados.

## Entrega

Subir a aplicação em um repositório Git remoto e disponibilizar o link.
