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

## Google Cloud Run

O Cloud Run pode hospedar este container e escalar a zero quando não recebe
requisições. É necessário criar uma conta/projeto Google Cloud e vincular uma
conta de faturamento para implantar. O serviço tem uma cota gratuita mensal,
mas o uso acima das cotas pode gerar cobranças; confira os preços e configure
alertas de orçamento antes de manter o serviço publicado. A cota gratuita é
compartilhada pela conta de faturamento e excedentes são cobrados conforme a
[tabela oficial de preços do Cloud Run](https://cloud.google.com/run/pricing).

Depois de configurar o projeto e instalar a Google Cloud CLI, faça login e
implante a partir deste diretório:

```bash
gcloud auth login
gcloud config set project ID_DO_PROJETO
gcloud run deploy ravache-docker --source . --region us-central1 --allow-unauthenticated --min 0 --max 1
```

O comando pede para habilitar APIs necessárias se ainda não estiverem ativas.
Revise qualquer etapa de faturamento e os preços atuais no Console do Google
Cloud antes de aceitá-la.

Após configurar o Workload Identity Federation do Google Cloud para o GitHub
Actions, cadastre estas variáveis no repositório (Settings → Secrets and
variables → Actions → Variables): `GCP_PROJECT_ID`,
`GCP_WORKLOAD_IDENTITY_PROVIDER`, `GCP_SERVICE_ACCOUNT` e, opcionalmente,
`GCP_REGION`. A documentação oficial de
[Workload Identity Federation para pipelines de implantação](https://cloud.google.com/iam/docs/workload-identity-federation-with-deployment-pipelines)
explica a configuração do vínculo sem salvar uma chave de longa duração no
GitHub. Quando as três primeiras variáveis estiverem definidas, cada push
validado em `main` também implanta o serviço público no Cloud Run;
`GCP_REGION` usa `us-central1` por padrão.
