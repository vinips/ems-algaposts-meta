# AlgaPosts
Sistema cuida da criação, busca e processamento de posts. Essas operações são realizadas pelos microsserviços Post-Service e Text-Processor-Service utilizando SpringBoot e comunicação assíncrona via RabbitMQ.

## Modelagem do Sistema

<img width="807" height="408" alt="Image" src="https://github.com/user-attachments/assets/886c605c-38fb-4902-97e0-30d22b333801" />


## Subindo Aplicação
### RabbitMQ com Docker
Na raiz do projeto, use os comandos a seguir para subir e encerrar o container.

Subindo RabbitMQ
```
docker-compose up
```

Encerrando RabbitMQ
```
docker-compose down
```

URL do painel de controle de monitoramento de filas do RabbitMQ
```
http://localhost:15672/
Login: rabbitmq
Senha: rabbitmq
```

### Subindo PostService
Em um novo Terminal
```
cd post-service
./gradlew bootRun
```

### Subindo TextProcessorService
Em um novo Terminal
```
cd text-processor-service
./gradlew bootRun
```

## End-points
### Create Post
POST em http://localhost:8080/api/posts

Body:
```
{
	"title": "Novo Post de apresentação",
	"body": " Esse é um post de apresentação para a criação do primeiro Post no produto AlgaPosts.",
	"author": "Marcus Aurélios Filho."
}
```
Retorno 201:
```
{
    "postId": "01985879-f430-739e-a7b7-66e0f5b6ce12",
    "title": "Novo Post de apresentação",
    "body": " Esse é um post de apresentação para a criação do primeiro Post no produto AlgaPosts.",
    "author": "Marcus Aurélios Filho.",
    "wordCount": null,
    "calculatedValue": null
}
```

### Find Post By Id
GET em http://localhost:8080/api/posts/:postId

onde :postId é um UUID.

Retorno 200:
```
{
    "postId": "01985879-f430-739e-a7b7-66e0f5b6ce12",
    "title": "Novo Post de apresentação",
    "body": " Esse é um post de apresentação para a criação do primeiro Post no produto AlgaPosts.",
    "author": "Marcus Aurélios Filho.",
    "wordCount": 15,
    "calculatedValue": 1.5
}
```

Retorno 404 com id invalido:
```
{
    "status": 404,
    "timestamp": "2025-07-29T20:22:10.064632-03:00",
    "type": "https://algaposts.com.br/entity-not-found",
    "title": "Entity Not Found",
    "detail": "Entity 01985879-f430-739e-a7b7-66e0f5b6ce11 Not Found",
    "userMessage": "Entity 01985879-f430-739e-a7b7-66e0f5b6ce11 Not Found",
    "fields": null
}
```

### Find Posts By Filter
GET em http://localhost:8080/api/posts/

Com a possibilidade de utilizar os parâmetros de filtro para paginar como "page=0&size=5&sort=id,desc"

Exemplo de URL completa: http://localhost:8080/api/posts?page=0&size=5&sort=id,desc

Retorno 200:
```
{
    "content": [
        {
            "postId": "01985879-f430-739e-a7b7-66e0f5b6ce12",
            "title": "Novo Post de apresentação",
            "summary": " Esse é um post de apresentação para a criação do primeiro Post no produto AlgaPosts.",
            "author": "Marcus Aurélios Filho."
        }
    ],
    "pageable": {
        "pageNumber": 0,
        "pageSize": 5,
        "sort": {
            "empty": false,
            "unsorted": false,
            "sorted": true
        },
        "offset": 0,
        "unpaged": false,
        "paged": true
    },
    "totalElements": 1,
    "totalPages": 1,
    "last": true,
    "size": 5,
    "number": 0,
    "sort": {
        "empty": false,
        "unsorted": false,
        "sorted": true
    },
    "numberOfElements": 1,
    "first": true,
    "empty": false
}
```



