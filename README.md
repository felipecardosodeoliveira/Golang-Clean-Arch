# Golang-Clean-Arch
Desafio Pós Goexpert - turma 4

## Como utilizar:

executar o comando docker compose up -d

execucar o comando make migrate

Acessar a pasta cmd e executar o comando: go run main.go wire_gen.go.

# Ele deve mostrar que os serviços disponíveis:

Starting web server on port :8000
Starting gRPC server on port 50051
Starting GraphQL server on port 8080


## Testar a API:

Acessar a pasta API:

Fazer a requisição para criar.

HTTP/1.1 200 OK
Date: Fri, 13 Sep 2024 18:51:18 GMT
Content-Length: 53
Content-Type: text/plain; charset=utf-8

{
  "id": "1",
  "price": 100,
  "tax": 2.5,
  "final_price": 102.5
}

Fazer a requisição para listar

HTTP/1.1 200 OK
Date: Fri, 13 Sep 2024 18:52:18 GMT
Content-Length: 55
Content-Type: text/plain; charset=utf-8

[
  {
    "id": "1",
    "price": 100,
    "tax": 2.5,
    "final_price": 102.5
  }
]

## Testar o gRPC :

Acessar a raiz do projeto e executar o seguinte comando: 

Golang-Clean-Arch$ evans --proto internal/infra/grpc/protofiles/order.proto --host localhost --port 50051

call CreateOrder
id (TYPE_STRING) => 2
price (TYPE_FLOAT) => 10.0
tax (TYPE_FLOAT) => 0.2
{
  "finalPrice": 10.2,
  "id": "2",
  "price": 10,
  "tax": 0.2
}


call ListOrders
{
  "orders": [
    {
      "finalPrice": 102.5,
      "id": "1",
      "price": 100,
      "tax": 2.5
    },
    {
      "finalPrice": 10.2,
      "id": "2",
      "price": 10,
      "tax": 0.2
    }
  ]
}


## Testar o GraphQL :

Acessar a porta: http://localhost:8080/

mutation createOrder {
  createOrder(input: {id:"3", Price: 10.5, Tax: 0.5}) {
    id
    Price
    Tax
  }
}

query queryOrders {
  listOrders {
    id
    Price
    Tax
    FinalPrice
  }
}

{
  "data": {
    "listOrders": [
      {
        "id": "1",
        "Price": 100,
        "Tax": 2.5,
        "FinalPrice": 102.5
      },
      {
        "id": "2",
        "Price": 10,
        "Tax": 0.2,
        "FinalPrice": 10.2
      },
      {
        "id": "3",
        "Price": 10.5,
        "Tax": 0.5,
        "FinalPrice": 11
      }
    ]
  }
}