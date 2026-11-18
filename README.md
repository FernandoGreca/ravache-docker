# Ravache Docker

Projeto mínimo em Express que responde `Hello World!` na rota `/`.

## Executar localmente

```bash
npm ci
npm start
```

Acesse http://localhost:3000.

## Executar com Docker

```bash
docker build -t ravache-docker .
docker run --rm -p 3000:3000 ravache-docker
```

Ou use Docker Compose:

```bash
docker compose up --build
```

## Integração contínua

O GitHub Actions executa `npm ci` e constrói a imagem Docker em cada push para
`main` e em pull requests direcionados a `main`. O workflow fica em
`.github/workflows/ci.yml`.

## Deploy automático no Render

Para usar o serviço Render gratuito criado para este repositório, cadastre o
deploy hook como segredo `RENDER_DEPLOY_HOOK` em Settings → Secrets and
variables → Actions → Secrets. Depois de um push aprovado em `main`, o workflow
aciona esse hook para publicar a versão atual. Sem o segredo, a etapa informa
que o deploy ainda não foi habilitado.
