# Entregas

Projeto de microsserviços para cadastro e acompanhamento de encomendas, com envio de notificações por e-mail.

## Serviços

- [Gerência de Entregas](./gerenciaentregas/README.md): API REST para moradores e encomendas.
- [Notificação de Entregas](./notificaentregas/README.md): consumidor RabbitMQ que envia e-mails aos destinatários.

## Executar todos os serviços com Docker Compose

Pré-requisitos: Docker e Docker Compose (plugin `docker compose`).

Na raiz do projeto, configure as credenciais de e-mail em um arquivo `.env`:

```dotenv
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USER=seu-email
MAIL_PASSWORD=sua-senha-de-aplicativo
MAIL_FROM=seu-email
```

Use uma senha de aplicativo ou credencial SMTP apropriada para o seu provedor. Não compartilhe nem versione o arquivo `.env`.

Inicie os bancos, o RabbitMQ e os dois serviços:

```bash
docker compose up --build
```

Para iniciar em segundo plano:

```bash
docker compose up --build -d
```

Para acompanhar os logs:

```bash
docker compose logs -f
```

Para parar os serviços:

```bash
docker compose down
```

O Compose sobe:

| Componente | Endereço/porta no host | Uso |
|---|---|---|
| API de gerência | `http://localhost:8080` | Endpoints REST de moradores e encomendas |
| Swagger UI da gerência | `http://localhost:8080/swagger-ui/index.html` | Documentação interativa da API |
| PostgreSQL de encomendas | `localhost:5432` | Banco da gerência |
| PostgreSQL de notificações | `localhost:5433` | Banco do serviço de notificações |
| RabbitMQ | `localhost:5672` | Mensageria entre os serviços |
| RabbitMQ Management | `http://localhost:15672` | Console de administração (usuário/senha padrão `guest`/`guest`) |

Ao cadastrar uma encomenda pela API, a gerência publica uma mensagem na fila RabbitMQ; o serviço de notificações a consome e envia o e-mail. A configuração SMTP precisa estar correta para que o envio seja concluído.

## Executar um serviço individualmente

Consulte os READMEs dos [serviços de gerência](./gerenciaentregas/README.md) e de [notificações](./notificaentregas/README.md) para instruções de execução local, configuração e exemplos.

## Autor

[Brenda Silva](https://www.linkedin.com/in/breudes/)

Este projeto é o projeto substitutivo da fase 4 da pós tech Java (FIAP).