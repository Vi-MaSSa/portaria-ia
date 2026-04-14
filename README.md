# Portaria IA

## 1. Visão geral

Este projeto é um backend minimalista para autenticação de dispositivos de um sistema de interfone e servidor. O objetivo é permitir que um dispositivo solicite um token temporário, receba um JWT e utilize esse token em requisições subsequentes para se comunicar com endpoints protegidos.

## 2. Arquitetura atual (Dia 1)

O sistema usa o conceito de ticket temporário baseado em JWT. O fluxo de handshake atual é:

- Interfone envia dados para `/api/device/auth`

- O servidor valida o `secret` do dispositivo

- Se válido, o servidor gera um token JWT de curta duração

- O dispositivo usa esse token em chamadas seguintes para acessar rotas protegidas, como `/api/event`

Esse fluxo garante que apenas dispositivos autorizados consigam gerar tokens e que o token tenha validade limitada.

## 3. Tecnologias utilizadas

- Express.js

- jsonwebtoken

- Preparado para futura adição de:

- HTTPS

- WebSocket

## 4. Funcionalidades implementadas

- Autenticação de dispositivo via secret

- Geração de JWT com expiração curta

- Middleware de validação para requests de dispositivo

- Estrutura de rotas organizada por domínio

- Endpoint protegido preparado para `/event`

## 5. Estrutura de pastas

- `/routes`

- Define os endpoints principais da API

- `authRoutes.js`, `deviceRoutes.js`, `eventRoutes.js`, `index.js`

- `/middlewares`

- Contém validações e autenticação reutilizáveis

- `jwtAuth.js`, `deviceAuth.js`, `userAuth.js`, `validateDeviceSecret.js`

- `server.js`

- Configura o Express

- Registra JSON parser e o router principal

## 6. Como rodar o projeto

1. Instale as dependências:

```bash

npm  install

```

2. Inicie o servidor:

```bash

node  server.js

```

3. Acesse a API em:

```text

http://localhost:3000/api

```

4. Exemplo de requisição no Postman:

- URL: `POST http://localhost:3000/api/device/auth`

- Body (JSON):

```json
{
  "deviceId": "device-123",

  "secret": "123"
}
```

## 7. Exemplo de uso (request/response)

### Requisição

`POST /api/device/auth`

Body JSON:

```json
{
  "deviceId": "device-123",

  "secret": "123"
}
```

### Resposta

```json
{
  "token": "<JWT_TOKEN_AQUI>"
}
```

O token é gerado com expiração curta e deve ser usado no header `Authorization: Bearer <TOKEN>` nas próximas requisições.

## 8. Segurança (conceitos aplicados)

- Uso de JWT com expiração curta para reduzir janela de abuso.

- Validação de dispositivo via middleware antes de emitir token.

- Separação de responsabilidades entre rotas e middlewares, criando uma base mais segura para futura autenticação de eventos.

## 9. Próximos passos

- Validar JWT no endpoint `/api/event`

- Adicionar HTTPS com certificados

- Implementar WebSocket / WSS para comunicação em tempo real

- Integrar Redis ou fila para trilhas de conexão e controle de sessão

- Adicionar engine de decisão para verificar permissão e estado do dispositivo
