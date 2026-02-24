# Dashboard estático para pipeline do Azure DevOps

Uma página **simples, estática e com layout moderno** para exibir informações de uma pipeline do **Azure DevOps**.

Use como:
- artefato publicado na pipeline
- conteúdo de um **Azure Static Web App**
- página de status interna

## Estrutura

- `index.html` – markup da página
- `styles.css` – estilos (tema escuro moderno, responsivo)
- `package.json` – opcional, apenas para rodar localmente com `serve`

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

## Como usar na pipeline do Azure DevOps

### 1. Publicar como artefato

No seu YAML de pipeline, adicione um passo para publicar a pasta com a página:

```yaml
- task: PublishBuildArtifacts@1
  displayName: 'Publicar dashboard estático'
  inputs:
    PathtoPublish: '$(Build.SourcesDirectory)/app'
    ArtifactName: 'pipeline-dashboard'
    publishLocation: 'Container'
```

Isso gera um artefato com o conteúdo desta pasta (incluindo `index.html` e `styles.css`).

### 2. Publicar em um Azure Static Web App (opcional)

Você pode apontar um Static Web App ou um Azure App Service para essa pasta gerada pelo build/pipeline, por exemplo:

- usar outro job/stage para fazer o deploy do artefato `pipeline-dashboard`
- ou ter um repositório separado apenas com essa página

## Personalização rápida

Abra `index.html` e ajuste:

- **Nome da pipeline**: trecho `app-web-ci-cd`
- **Número do build**, branch, commit
- Textos dos cards de stages e da timeline

Abra `styles.css` para:

- trocar cores do tema
- ajustar espaçamentos, fontes e radius

Se quiser, posso te ajudar a integrar com dados reais da pipeline (via REST API do Azure DevOps) em uma próxima etapa.

