## Descrição

Projeto front-end Next.js e back-end Nest.js para o desafio técnico LuizaLabs com webserver Nginx.

## Rodando o back-end

### 1. Configure as variáveis de ambiente

Para rodar a aplicação, é necessário adicionar algumas variáveis de ambiente.

Crie um arquivo `.env` na pasta `bff`.

Adicione as variáveis abaixo preenchendo os valores para as variáveis `SPOTFY_CLIENT_ID`, `SPOTFY_CLIENT_SECRET` e `SPOTFY_REDIRECT_URI` com os dados da aplicção criada no dashboard do Spotfy.

```
# ./bff/.env

SPOTFY_CLIENT_ID=
SPOTFY_CLIENT_SECRET=
SPOTFY_REDIRECT_URI=http://127.0.0.1:3000/callback
SPOTFY_ACCOUNTS_URL=https://accounts.spotify.com
SPOTFY_API_URL=https://api.spotify.com/v1
PORT=3001
```

### 2. Crie uma rede no Docker

> A partir deste passo, será necessário ter o Docker instalado. Caso precise instalar, siga as [instruções de instalação](https://docs.docker.com/engine/install/) nos manuais oficiais do Docker. Após instalar, siga as instruções abaixo para construir e iniciar a aplicação.

Para que o bff se comunique com o front-end, é necessário criar uma rede para conectar os containers.

Crie a rede `spotfy-luizalabs` com o comando abaixo.

```bash
docker network create spotfy-luizalabs
```

### 3. Construa a imagem

Utilize o comando abaixo para construir a imagem com o nome de `spotfy-luizalabs-fe`.

```bash
docker build -t spotfy-luizalabs-bff ./bff
```

### 4. Inicie o container

Inicie um container com a imagem criada no passo anterior usando o comando a seguir.

```bash
docker run -d --name spotfy-luizalabs-bff \
  --network spotfy-luizalabs \
  -p 3001:3001 \
  --env-file ./bff/.env \
  spotfy-luizalabs-bff
```

## Rodando o front-end

### 1. Construa a imagem

Utilize o comando abaixo para construir a imagem com o nome de `spotfy-luizalabs-fe`.

```bash
docker build -t spotfy-luizalabs-fe ./fe
```

### 3. Inicie a aplicação

Inicie um container com a imagem criada no passo anterior usando o comando a seguir.

```bash
docker run -d --name spotfy-luizalabs-fe \
  --network spotfy-luizalabs \
  -p 3000:3000 \
  --env-file ./fe/.env.docker \
  spotfy-luizalabs-fe
```

### 4. Acesse a aplicação

Acesse [http://localhost:3000/](http://localhost:3000/)

## Rodando o webserver Nginx (opcional)

Esta etapa é opcional, ainda está incompleto.

### 1. Construa a imagem

```bash
docker build -t spotfy-luizalabs-nginx ./nginx
```

### Inicie o container do servidor

```bash
docker run -d --name spotfy-luizalabs-nginx \
  --network spotfy-luizalabs \
  -p 80:80 \
  -p 443:443 \
  spotfy-luizalabs-nginx
```

## Requisitos Obrigatórios do projeto

- [x] Segmentação de commits
- [x] Lint
- [x] Autenticação via Spotify
- [x] Listar artistas
- [x] Listar álbuns de um artista
- [ ] Utilizar paginação (scroll infinito ou não)
- [ ] Funcionamento offline
- [ ] Testes unitários
- [ ] Deploy da aplicação

### Bônus

- [ ] Testes E2E
- [ ] CI/CD
- [ ] Responsividade (celular e tablet)
- [ ] Qualidade de código (Sonarqube)
- [ ] PWA
