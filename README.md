# Site estático (HTML/CSS) em Docker

Uma página **simples e estática** (apenas `HTML + CSS`) com **cara de template/landing page** (hero, serviços, planos, FAQ, CTA e contato).

Use como:
- site institucional simples (intranet)
- artefato publicado na pipeline
- container Docker em qualquer ambiente
- conteúdo de um **Azure Static Web App** / App Service (opcional)

## Estrutura

- `index.html` – página (seções: hero, quem somos, serviços, modelos, FAQ, CTA, contato e footer)
- `styles.css` – estilos (layout claro com gradientes, cards, responsivo)
- `Dockerfile` – Nginx servindo arquivos estáticos
- `package.json` – opcional, para rodar localmente com `serve`
- `.gitignore` – ignora `node_modules`, logs e arquivos de editor

## Como rodar localmente

No diretório do projeto (`c:\Users\Work\Documents\app`):

```bash
npm install
npm run start
```

Depois abra o endereço mostrado no terminal (por padrão `http://localhost:3000` ou similar).

Se preferir, você também pode só abrir o arquivo `index.html` direto no navegador (sem `npm`).

## Como rodar com Docker

No diretório do projeto:

```bash
docker build -t azure-pipeline-dashboard .
docker run --rm -p 8080:80 azure-pipeline-dashboard
```

Depois acesse no navegador:

```text
http://localhost:8080
```

### Recriar o container após mudanças

Se você alterou `index.html`/`styles.css`, basta rebuildar a imagem e subir de novo:

```bash
docker build -t azure-pipeline-dashboard .
docker run --rm -p 8080:80 azure-pipeline-dashboard
```

## Como usar na pipeline do Azure DevOps

### 1. Publicar como artefato

No seu YAML de pipeline, adicione um passo para publicar a pasta com a página:

```yaml
- task: PublishBuildArtifacts@1
  displayName: 'Publicar site estático'
  inputs:
    PathtoPublish: '$(Build.SourcesDirectory)/app'
    ArtifactName: 'static-site'
    publishLocation: 'Container'
```

Isso gera um artefato com o conteúdo desta pasta (incluindo `index.html`, `styles.css` e `Dockerfile`).

### 2. Publicar em um Azure Static Web App (opcional)

Você pode apontar um Static Web App ou um Azure App Service para essa pasta gerada pelo build/pipeline, por exemplo:

- usar outro job/stage para fazer o deploy do artefato `static-site`
- ou ter um repositório separado apenas com essa página

## Personalização rápida

Abra `index.html` e ajuste:

- **Hero**: título, subtítulo e botões
- **Serviços**: cards em “Serviços”
- **Modelos**: cards da seção “Modelos de serviço”
- **FAQ**: perguntas/respostas na seção “FAQ”
- **Contato**: textos e placeholders do formulário (ele é apenas visual)

Abra `styles.css` para:

- trocar paleta (variáveis `--primary`, `--primary-2`, `--primary-3`)
- ajustar espaçamentos, sombras e radius

## Referências visuais

- Site institucional (conteúdo base): `https://www.foursys.com.br/`
- Estilo de template/landing page (inspiração): `https://demo.templatemonster.com/pt-br/demo/363196.html`

Se quiser, posso:
- deixar o layout ainda mais próximo do template (seções/cores/tipografia), ou
- integrar o formulário com um backend (API) para envio real.

