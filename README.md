# localstack

## Pré Requisitos

- [ ] Docker
- [ ] AWS Cli

## Setup

### Docker

```yaml
version: "3.8"

services:
  localstack:
    container_name: "${LOCALSTACK_DOCKER_NAME:-localstack-main}"
    image: localstack/localstack
    ports:
      - "127.0.0.1:4566:4566"            # LocalStack Gateway
      - "127.0.0.1:4510-4559:4510-4559"  # external services port range
    environment:
      # LocalStack configuration: https://docs.localstack.cloud/references/configuration/
      - DEBUG=${DEBUG:-0}
    volumes:
      - "${LOCALSTACK_VOLUME_DIR:-./volume}:/var/lib/localstack"
      - "/var/run/docker.sock:/var/run/docker.sock"
```

> docker-compose up -d

### AWS Cli

```
aws configure set aws_access_key_id "dummy" --profile local
aws configure set aws_secret_access_key "dummy" --profile local
aws configure set region "eu-central-1" --profile local
aws configure set output "table" --profile local
```

## Execução

Criar a fila

> aws sqs create-queue --endpoint-url http://localhost:4566 --queue-name strd --profile local

Listar as filas

> aws sqs list-queues --endpoint-url http://localhost:4566 --profile local

Enviar uma mensagem

> aws sqs send-message --endpoint-url http://localhost:4566 --queue-url http://sqs.us-east-1.localhost.localstack.cloud:4566/000000000000/strd --message-body "Hello World" --profile local

Ler a mensagem

> aws sqs receive-message --endpoint-url http://localhost:4566 --queue-url http://sqs.us-east-1.localhost.localstack.cloud:4566/000000000000/strd --profile local
