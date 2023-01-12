- [Cloud Build ](https://cloud.google.com/build)
- [Documentação ](https://cloud.google.com/build/docs)
- [Guias](https://cloud.google.com/build/docs/build-push-docker-image)

Isso foi útil?



Envie comentários

# Como implantar no Cloud Run usando o Cloud Build

bookmark_border



Nesta página, explicamos como implantar automaticamente os serviços do Cloud Run e do Cloud Run for Anthos usando o Cloud Build. Se você não estiver familiarizado com o Cloud Build, leia as [guias de início rápido](https://cloud.google.com/build/docs/quickstarts) e a [visão geral da configuração da compilação](https://cloud.google.com/build/docs/build-config) primeiro.



O [Cloud Run](https://cloud.google.com/run/docs) e o [Cloud Run for Anthos](https://cloud.google.com/anthos/run/docs) são plataformas de computação que permitem executar contêineres sem estado em um ambiente sem servidor. Com o Cloud Build, é possível implantar imagens de contêiner do Container Registry e do Artifact Registry no Cloud Run. É possível implantar uma imagem existente, criar e implantar uma imagem ou automatizar a implantação.

## Antes de começar



- 

- 

- Ative as APIs Cloud Build, Cloud Run, Container Registry, and Resource Manager.

  [Ative as APIs](https://console.cloud.google.com/flows/enableapi?apiid=cloudbuild.googleapis.com,run.googleapis.com,containerregistry.googleapis.com,cloudresourcemanager.googleapis.com&redirect=https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run)

- Prepare o código-fonte do aplicativo. Seu código-fonte precisa ser armazenado em um repositório, como o Cloud Source Repositories, o GitHub ou o Bitbucket.
- Para executar os comandos `gcloud` desta página, instale a [Google Cloud CLI](https://cloud.google.com/sdk).

### Permissões do IAM obrigatórias

Se a imagem estiver armazenada no mesmo projeto do Cloud em que você quer implantar ou se a imagem for [pública](https://cloud.google.com/container-registry/docs/access-control#serving_images_publicly) no Container Registry, serão necessárias as seguintes permissões do IAM:

[Cloud Run](https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run#cloud-run)[Cloud Run for Anthos](https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run#cloud-run-for-anthos)

Para implantar no Cloud Run, conceda os papéis "Administrador do Cloud Run" e "Usuário da conta de serviço" à conta de serviço do Cloud Build:

1. Abra a página Configurações do Cloud Build no Console do Google Cloud.

   [Acessar a página de configurações do Cloud Build](https://console.cloud.google.com/cloud-build/settings)

2. No painel de permissões da conta de serviço, defina o status do papel *Administrador do Cloud Run* como **ATIVADO**:

   ![Captura de tela da página de permissões da conta de serviço](https://cloud.google.com/build/images/service-account-permissions.png)

3. No pop-up "Etapas adicionais", clique em **CONCEDER ACESSO A TODAS AS CONTAS DE SERVIÇO**.

**Observação:** para aumentar a segurança, conceda o papel **Usuário da conta de serviço** apenas à conta de serviço do Cloud Run Runtime. Para instruções sobre como fazer isso, consulte [Como usar permissões mínimas do IAM](https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run#continuous-iam).

## Como criar e implantar um contêiner

O Cloud Build permite que você crie a imagem de contêiner, armazene-a no Container Registry e implante a imagem no Cloud Run.

Para criar e implantar uma imagem de contêiner:

1. No diretório raiz do projeto, crie um arquivo de configuração chamado `cloudbuild.yaml`.

2. No arquivo de configuração de build, adicione etapas de build do `docker` para criar a imagem e enviá-la ao Container Registry e adicione uma etapa de build do `gcloud` para invocar o comando `gcloud run deploy` a fim de implantar a imagem no Cloud Run:

   [Cloud Run](https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run#cloud-run)[Cloud Run for Anthos](https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run#cloud-run-for-anthos)

   ```
   steps:
   # Build the container image
   - name: 'gcr.io/cloud-builders/docker'
     args: ['build', '-t', 'gcr.io/PROJECT_ID/IMAGE', '.']
   # Push the container image to Container Registry
   - name: 'gcr.io/cloud-builders/docker'
     args: ['push', 'gcr.io/PROJECT_ID/IMAGE']
   # Deploy container image to Cloud Run
   - name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
     entrypoint: gcloud
     args: ['run', 'deploy', 'SERVICE-NAME', '--image', 'gcr.io/PROJECT_ID/IMAGE', '--region', 'REGION']
   images:
   - gcr.io/PROJECT_ID/IMAGE
   ```

   Em que:

   - `SERVICE-NAME` é o nome do serviço do Cloud Run.
   - `REGION` é a região do serviço do Cloud Run em que você está implantando.
   - `PROJECT_ID` é o ID do projeto do Google Cloud em que a imagem está armazenada.
   - `IMAGE` é o nome da imagem no Container Registry.

3. Navegue até o diretório raiz do projeto e execute o seguinte comando, em que `REGION` é uma das [regiões de build compatíveis](https://cloud.google.com/build/docs/locations) para executar a versão:

   ```
    gcloud builds submit --region=REGION
   ```

Após a conclusão, uma mensagem de sucesso é exibida junto com o URL do serviço implantado.

## Implantação contínua

É possível automatizar a implantação do software no Cloud Run e no Cloud Run for Anthos criando gatilhos do Cloud Build. Configure os acionadores para criar e implantar imagens sempre que atualizar o código-fonte.

Para automatizar a implantação:

1. Na raiz do seu repositório, adicione um arquivo de configuração chamado `cloudbuild.yaml` com etapas para criar a imagem, enviá-la ao Container Registry e invocar o comando `gcloud run deploy`:

   [Cloud Run](https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run#cloud-run)[Cloud Run for Anthos](https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run#cloud-run-for-anthos)

   ```
    steps:
    # Build the container image
    - name: 'gcr.io/cloud-builders/docker'
      args: ['build', '-t', 'gcr.io/$PROJECT_ID/SERVICE-NAME:$COMMIT_SHA', '.']
    # Push the container image to Container Registry
    - name: 'gcr.io/cloud-builders/docker'
      args: ['push', 'gcr.io/$PROJECT_ID/SERVICE-NAME:$COMMIT_SHA']
    # Deploy container image to Cloud Run
    - name: 'gcr.io/google.com/cloudsdktool/cloud-sdk'
      entrypoint: gcloud
      args:
      - 'run'
      - 'deploy'
      - 'SERVICE-NAME'
      - '--image'
      - 'gcr.io/$PROJECT_ID/SERVICE-NAME:$COMMIT_SHA'
      - '--region'
      - 'REGION'
    images:
    - 'gcr.io/$PROJECT_ID/SERVICE-NAME:$COMMIT_SHA'
   ```

   Em que:

   - `SERVICE-NAME` é o nome do serviço do Cloud Run.
   - `REGION` é a região do serviço do Cloud Run que você está implantando.

   O uso da variável de substituição `$COMMIT_SHA` é preenchido pelo Cloud Build quando acionado a partir de um repositório git.

2. Crie um gatilho de compilação com o arquivo de configuração criado na etapa anterior:

   1. Acesse a página **Gatilhos**:

      [Acessar a página "Gatilhos"](https://console.cloud.google.com/cloud-build/triggers)

   2. Clique em **Criar gatilho**.

   3. No campo **Nome**, insira um nome para o gatilho.

   4. Em **Região**, selecione a [região](https://cloud.google.com/build/docs/locations) do gatilho.

      **Observação:** a região do gatilho precisa corresponder à região do serviço do Cloud Run em que você está implantando.

   5. Em **Evento**, selecione o evento do repositório para iniciar o gatilho.

   6. Em **Origem**, selecione o repositório e o nome da ramificação ou da tag que iniciará o gatilho. Para mais informações sobre como especificar quais ramificações precisam ser criadas automaticamente, consulte [Como criar um gatilho de compilação](https://cloud.google.com/build/docs/running-builds/automate-builds#build_trigger).

   7. Em **Configuração**, selecione **Arquivo de configuração do Cloud Build (YAML ou JSON)**.

   8. No campo **local do arquivo de configuração do Cloud Build**, digite `cloudbuild.yaml` depois do `/`.

   9. Clique em **Criar** para salvar o gatilho de compilação.

3. O processo foi concluído. De agora em diante, sempre que você enviar para seu repositório, um build e uma implantação do serviço serão chamados automaticamente.

**Observação**: para tornar o serviço implantado público, faça uma alteração única nas [configurações de IAM do serviço](https://cloud.google.com/run/docs/securing/managing-access#making_a_service_public).

Sempre que enviar um novo código para seu repositório, você acionará automaticamente uma versão e implantará seu serviço do Cloud Run.

Para mais informações sobre como criar gatilhos do Cloud Build, consulte [Como criar e gerenciar gatilhos de build](https://cloud.google.com/build/docs/automating-builds/create-manage-triggers).

## Como usar permissões mínimas do IAM

Quando um contêiner é implantado em um serviço do Cloud Run, ele é executado com a identidade da conta de serviço do ambiente de execução deste serviço do Cloud Run. Como o Cloud Build pode implantar novos contêineres automaticamente, o Cloud Build precisa ser capaz de *atuar como* a [conta de serviço do ambiente de execução do serviço do Cloud Run](https://cloud.google.com/run/docs/securing/service-identity).

Para conceder acesso limitado ao Cloud Build para implantar em um serviço do Cloud Run, siga estas etapas:

[Console](https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run#console)[gcloud](https://cloud.google.com/build/docs/deploying-builds/deploy-cloud-run#gcloud)

1. Acesse a página **Contas de serviço** do Console do Google Cloud:

   [Acessar Contas de serviço](https://console.cloud.google.com/iam-admin/serviceaccounts)

   

2. Clique no endereço de e-mail da conta de serviço do ambiente de execução do serviço do Cloud Run (por padrão, é `PROJECT_NUMBER-compute@developer.gserviceaccount.com`).

3. Clique na guia **Permissões.**.

4. Clique em person_add**CONCEDER ACESSO**.

5. Insira a conta de serviço do Cloud Build (`PROJECT_NUMBER@cloudbuild.gserviceaccount.com`)

6. Na lista suspensa **Selecionar um papel**, selecione o papel **Contas de serviço** > **Usuário da conta de serviço**.

7. Clique em **Save**.

Consulte [Permissões de implantação](https://cloud.google.com/run/docs/reference/iam/roles#additional-configuration) para mais informações.

## Exemplos de código

Veja alguns repositórios de amostra, cada um com um aplicativo de amostra e um arquivo de configuração de build para implantar o aplicativo no Cloud Run:

- [deploy-prebuilt](https://github.com/GoogleCloudPlatform/cloud-build-samples/tree/main/quickstart-deploy): um exemplo de código que mostra como implantar uma imagem pré-criada no Cloud Run.
- [run-example-builddeploy](https://github.com/GoogleCloudPlatform/cloud-build-samples/tree/main/run-example-builddeploy): um exemplo de código que mostra como criar e implantar uma imagem no Cloud Run.

## A seguir

- Saiba como [usar o Google Cloud Deploy para implantar no Cloud Run](https://cloud.google.com/deploy/docs/deploy-app-run).
- Saiba como [implantar no GKE](https://cloud.google.com/build/docs/deploying-builds/deploy-gke).
- Saiba como [implantar no Cloud Functions](https://cloud.google.com/build/docs/deploying-builds/deploy-functions).
- Saiba como [implantar no App Engine](https://cloud.google.com/build/docs/deploying-builds/deploy-appengine).
- Saiba como [implantar no Firebase](https://cloud.google.com/build/docs/deploying-builds/deploy-firebase).
- Saiba mais sobre o [Cloud Run](https://cloud.google.com/run/docs) ou o [Cloud Run for Anthos](https://cloud.google.com/anthos/run/docs).
- Saiba como [resolver erros de build](https://cloud.google.com/build/docs/troubleshooting).