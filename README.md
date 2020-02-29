# Desafio Dito - Projeto com todos os entregáveis

Desafio da empresa Dito. Este repositório possui um docker-compose onde todos os entregáveis estão disponibilizados via imagens. Desta forma é bem fácil subir os serviços e validar o que foi feito em qualquer máquina que possua Docker e docker-compose instalado.

## Pré requisitos

1. Máquina deve ter no mínimo 4GB de RAM para aguentar rodar as ferramentas utilizadas.
2. Docker instalado
3. docker-compose instalado
4. Preferencialmente máquinas Linux (Ubuntu, Debian, CentOS)

## Instalação

1. Clonar o repositório
```bash
    https://github.com/lcastrooliveira/dito-docker-compose.git
```

2. Navegar até a pasta do projeto
```bash
    cd dito-docker-compose
```

3. Subir os serviços
```bash
    docker-compose up
```

## Endpoints do Desafio 1

Na [API coletora](https://github.com/lcastrooliveira/dito-producer) de eventos:

    POST localhost:8080/api/v1/events

Usado para endpoint utilizado para publicar um evento. Exemplo de evento:

```json
{
	"event": "buy",
	"timestamp": "2023-09-22T13:57:31.2311892-03:00",
	"revenue": 123.89
}
```
E na [API consumidora](https://github.com/lcastrooliveira/dito-consumer) de eventos:

    GET localhost:8081/api/v1/eventsapi/v1/events?eventName=<searchTerm>&page=<pageIndex>&size=<pageSize>

Usado para buscar as informações de eventos cadastradas (autocomplete). Parâmetros:
1. eventName (obrigatório): termo de busca para o campo `event`. Mínimo 2 caracteres.
2. Page (opcional): Número da página, caso não fornecido assume-se valor 0.
3. Size (opcional): Tamanho da página, caso não fornecido assume-se valor 20.

## Endpoint do Desafio 2

    POST localhost:8082/api/v1/timeline

Usado para endpoint utilizado para mandar uma lista de eventos, por exemplo:

```json
{
    "events":[
       {
          "event":"comprou-produto",
          "timestamp":"2016-09-22T13:57:32.2311892-03:00",
          "custom_data":[
             {
                "key":"product_name",
                "value":"Camisa Azul"
             },
             {
                "key":"transaction_id",
                "value":"3029384"
             },
             {
                "key":"product_price",
                "value":100
             }
          ]
       },
       {
          "event":"comprou",
          "timestamp":"2016-09-22T13:57:31.2311892-03:00",
          "revenue":250,
          "custom_data":[
             {
                "key":"store_name",
                "value":"Patio Savassi"
             },
             {
                "key":"transaction_id",
                "value":"3029384"
             }
          ]
       },
       {
          "event":"comprou-produto",
          "timestamp":"2016-09-22T13:57:33.2311892-03:00",
          "custom_data":[
             {
                "key":"product_price",
                "value":150
             },
             {
                "key":"transaction_id",
                "value":"3029384"
             },
             {
                "key":"product_name",
                "value":"Calça Rosa"
             }
          ]
       },
       {
          "event":"comprou-produto",
          "timestamp":"2016-10-02T11:37:35.2300892-03:00",
          "custom_data":[
             {
                "key":"transaction_id",
                "value":"3409340"
             },
             {
                "key":"product_name",
                "value":"Tenis Preto"
             },
             {
                "key":"product_price",
                "value":120
             }
          ]
       },
       {
          "event":"comprou",
          "timestamp":"2016-10-02T11:37:31.2300892-03:00",
          "revenue":120,
          "custom_data":[
             {
                "key":"transaction_id",
                "value":"3409340"
             },
             {
                "key":"store_name",
                "value":"BH Shopping"
             }
          ]
       }
    ]
 }
```

Resultado: Eventos agrupados por ID de transação. Em que cada item corresponde a uma compra realizada em
um estabelicimento. As infomrações são ordenadas em ordem decrescente:

```json
{
    "timeline": [
        {
            "timestamp": "2016-10-02T14:37:31.2300892Z",
            "revenue": 120,
            "transaction_id": "3409340",
            "store_name": "BH Shopping",
            "products": [
                {
                    "name": "Tenis Preto",
                    "price": 120
                }
            ]
        },
        {
            "timestamp": "2016-09-22T16:57:31.2311892Z",
            "revenue": 250,
            "transaction_id": "3029384",
            "store_name": "Patio Savassi",
            "products": [
                {
                    "name": "Camisa Azul",
                    "price": 100
                },
                {
                    "name": "Calça Rosa",
                    "price": 150
                }
            ]
        }
    ]
}
```

## Tecnologias utilizadas:

* Java 8 com Spring Boot 2
* Apache Kafka
* Elasticsearch
* Docker

### Referências

* [Dito Producer - API que publica os dados](https://github.com/lcastrooliveira/dito-producer)
* [Dito Consumer - API que consume os dados](https://github.com/lcastrooliveira/dito-consumer)
* [Dito Data Manipulation - API que cria uma timeline baseados nos eventos de entrada](https://github.com/lcastrooliveira/dito-data-manipulation)
* [Intro to Apache Kafka with Spring](https://www.baeldung.com/spring-kafka)
* [Intro to Apache Kafka with Spring](https://www.baeldung.com/spring-kafka)
* [Introduction to Spring Data Elasticsearch](https://www.baeldung.com/spring-data-elasticsearch-tutorial)
* [Fabric 8 Maven Plugin for Docker](https://dmp.fabric8.io/)

### 2020 - Lucas de Castro Oliveira