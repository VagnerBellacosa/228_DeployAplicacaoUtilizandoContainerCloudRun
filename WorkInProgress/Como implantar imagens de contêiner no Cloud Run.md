- [Cloud Run ](https://cloud.google.com/run)
- [Documentação ](https://cloud.google.com/run/docs)
- [Guias](https://cloud.google.com/run/docs/quickstarts/deploy-container)

Isso foi útil?



Envie comentários

# Como implantar imagens de contêiner no Cloud Run

bookmark_border



Esta página descreve como implantar novos serviços e novas revisões no Cloud Run.

## Permissões necessárias para implantar

Você precisa ter UM dos seguintes papéis:

- *Proprietário*
- *Editor*
- Os papéis *Administrador do Cloud Run* e *Usuário da conta de serviço*
- Qualquer papel personalizado que inclua esta [lista específica de permissões](https://cloud.google.com/run/docs/reference/iam/roles#additional-configuration)

## Registros e imagens de contêiner compatíveis

É possível usar imagens de contêiner armazenadas no [Container Registry](https://cloud.google.com/container-registry) ou no [Artifact Registry](https://cloud.google.com/artifact-registry/docs/overview). O Google recomenda o uso do Artifact Registry em vez do Container Registry.

É possível usar apenas os seguintes tipos de imagens de contêiner:

- Imagens de contêiner armazenadas no mesmo projeto em que você está criando o job ou o serviço
- Imagens de contêiner de outros projetos do Google Cloud ([desde que as permissões corretas do IAM estejam definidas](https://cloud.google.com/run/docs/deploying#other-projects)).
- [Imagens de contêiner públicas](https://cloud.google.com/container-registry/docs/access-control#serving_images_publicly) do Container Registry ou Artifact Registry.

Se você estiver armazenando imagens de contêiner em outro tipo de registro de contêiner, siga as instruções em [Como implantar imagens de registros não compatíveis](https://cloud.google.com/run/docs/deploying#other-registries).

## Como implantar um novo serviço

É possível especificar uma imagem de contêiner com uma tag (por exemplo, `gcr.io/my-project/my-image:latest`) ou com um resumo exato (por exemplo, `gcr.io/my-project/my-image@sha256:41f34ab970ee...`).

A implantação de um serviço pela primeira vez cria a primeira revisão dele. Observe que as revisões são imutáveis. Se você implantar a partir de uma tag de imagem de contêiner, ela será resolvida para um resumo, e a revisão sempre exibirá esse resumo específico.

É possível implantar um contêiner usando o console do Google Cloud, a linha de comando `gcloud` ou um arquivo de configuração YAML.

Clique na guia para ver instruções usando a ferramenta de sua preferência.

[Console](https://cloud.google.com/run/docs/deploying#console)[Linha de comando](https://cloud.google.com/run/docs/deploying#linha-de-comando)[YAML](https://cloud.google.com/run/docs/deploying#yaml)[Cloud Code](https://cloud.google.com/run/docs/deploying#cloud-code)[Terraform](https://cloud.google.com/run/docs/deploying#terraform)

Para implantar uma imagem de contêiner, realize as etapas a seguir:

1. [Acesse o Cloud Run](https://console.cloud.google.com/run)
2. Clique em **Criar serviço** para exibir o formulário *Criar serviço*.
   1. No formulário, selecione a opção de implantação:
      1. Se você quiser implantar um contêiner manualmente, selecione **Implantar uma revisão de uma imagem de contêiner atual** e especifique a imagem.
      2. Se você quiser automatizar a implantação contínua, selecione *Implantar continuamente novas revisões de um repositório de origem* e siga as [instruções para implantações contínuas](https://cloud.google.com/run/docs/continuous-deployment-with-cloud-build#setup-cd).
   2. Digite o nome do serviço desejado. Os nomes dos serviços precisam ter 49 caracteres ou menos e ser exclusivos por região e projeto. Não é possível alterar o nome de um serviço depois e ele fica visível publicamente.
   3. [Selecione a região](https://cloud.google.com/run/docs/deploying#before-you-begin) em que você quer que o serviço esteja localizado. O seletor de região indica o [nível de preço](https://cloud.google.com/run/pricing), a disponibilidade de [mapeamentos de domínio](https://cloud.google.com/run/docs/mapping-custom-domains) e destaca regiões com o [impacto de carbono mais baixo](https://cloud.google.com/sustainability/region-carbon#region-picker).
   4. Defina a [alocação de CPU e os preços](https://cloud.google.com/run/docs/configuring/cpu-allocation) como preferir.
   5. Em *Escalonamento automático*, especifique as instâncias [mínima](https://cloud.google.com/run/docs/configuring/min-instances) e [máxima](https://cloud.google.com/run/docs/configuring/max-instances).
   6. Defina as configurações de [*Entrada*](https://cloud.google.com/run/docs/securing/ingress) no formato conforme pretendido.
   7. Em *Autenticação*:
      - Se você estiver criando uma API ou um site público, selecione **Permitir invocações não autenticadas**. A seleção atribui o papel de chamador do IAM ao identificador especial `allUser`. É possível [usar o IAM para editar essa configuração](https://cloud.google.com/run/docs/securing/authenticating#service-to-service) depois de criar o serviço.
      - Se você quiser que um serviço seguro seja protegido por autenticação, selecione **Exigir autenticação**.
3. Clique em **Contêiner, conexões, segurança** para definir outras configurações opcionais nas guias apropriadas:
   - [Simultaneidade](https://cloud.google.com/run/docs/configuring/concurrency)
   - [Configuração do contêiner](https://cloud.google.com/run/docs/configuring/containers)
   - [Limites de CPU](https://cloud.google.com/run/docs/configuring/cpu)
   - [Limites de memória](https://cloud.google.com/run/docs/configuring/memory-limits)
   - [Tempo limite da solicitação](https://cloud.google.com/run/docs/configuring/request-timeout)
   - [Secrets](https://cloud.google.com/run/docs/configuring/secrets)
   - [Variáveis de ambiente](https://cloud.google.com/run/docs/configuring/environment-variables)
   - [Ambiente de execução](https://cloud.google.com/run/docs/configuring/execution-environments)
   - [HTTP/2](https://cloud.google.com/run/docs/configuring/http2)
   - [Marcadores](https://cloud.google.com/run/docs/configuring/labels)
   - [Contas de serviço](https://cloud.google.com/run/docs/configuring/service-accounts)
   - [Conexões do Cloud SQL](https://cloud.google.com/run/docs/configuring/connect-cloudsql)
   - [Conexão VPC](https://cloud.google.com/run/docs/configuring/connecting-vpc)
4. Após concluir a configuração do serviço, clique em **Criar** para implantar a imagem no Cloud Run e aguarde a conclusão da implantação.
5. Clique no link do URL exibido para abrir o endpoint exclusivo e estável do serviço implantado.

Durante a implantação, o [agente de serviço](https://cloud.google.com/iam/docs/service-agents) do Cloud Run precisa acessar o contêiner implantado, que é o caso por padrão.

Cada serviço tem um URL exclusivo e permanente que não muda ao longo do tempo à medida que você implanta novas revisões nele.

## Como implantar uma nova revisão de um serviço atual

É possível implantar uma nova revisão usando o Console do Google Cloud, a linha de comando `gcloud` ou um arquivo de configuração YAML.

A alteração de qualquer configuração resulta na criação de uma nova revisão, mesmo que não haja alterações na imagem do contêiner. Cada revisão criada é imutável.

Clique na guia para ver instruções usando a ferramenta de sua preferência.

[Console](https://cloud.google.com/run/docs/deploying#console)[Linha de comando](https://cloud.google.com/run/docs/deploying#linha-de-comando)[YAML](https://cloud.google.com/run/docs/deploying#yaml)[Cloud Code](https://cloud.google.com/run/docs/deploying#cloud-code)[Terraform](https://cloud.google.com/run/docs/deploying#terraform)

Para implantar uma nova revisão de um serviço atual, realize as etapas a seguir:

1. [Acessar o Cloud Run](https://console.cloud.google.com/run)
2. Localize o serviço que você quer atualizar na lista de serviços e clique nele para abrir os detalhes.
3. Clique em **EDITAR E IMPLANTAR NOVA REVISÃO** para exibir o formulário de implantação da revisão.
   1. Se necessário, forneça o URL para a nova imagem do contêiner que você quer implantar.
   2. [Configure o contêiner](https://cloud.google.com/run/docs/configuring/containers) conforme necessário.
   3. Defina a [alocação e os preços de CPU](https://cloud.google.com/run/docs/configuring/cpu-allocation) conforme necessário.
   4. Em Capacidade, especifique [limites de memória](https://cloud.google.com/run/docs/configuring/memory-limits) e [limites de CPU](https://cloud.google.com/run/docs/configuring/cpu).
   5. Especifique o [tempo limite da solicitação](https://cloud.google.com/run/docs/configuring/request-timeout) e a [simultaneidade](https://cloud.google.com/run/docs/configuring/concurrency) conforme necessário.
   6. Especifique o [ambiente de execução](https://cloud.google.com/run/docs/configuring/execution-environments) conforme necessário.
   7. Em *Escalonamento automático*, especifique as instâncias [mínima](https://cloud.google.com/run/docs/configuring/min-instances) e [máxima](https://cloud.google.com/run/docs/configuring/max-instances).
   8. Use as outras guias conforme necessário para configurar opcionalmente:
      - [Secrets](https://cloud.google.com/run/docs/configuring/secrets)
      - [Variáveis de ambiente](https://cloud.google.com/run/docs/configuring/environment-variables)
      - [HTTP/2](https://cloud.google.com/run/docs/configuring/http2)
      - [Marcadores](https://cloud.google.com/run/docs/configuring/labels)
      - [Contas de serviço](https://cloud.google.com/run/docs/configuring/service-accounts)
      - [Conexões do Cloud SQL](https://cloud.google.com/run/docs/configuring/connect-cloudsql)
      - [Conexão VPC](https://cloud.google.com/run/docs/configuring/connecting-vpc)
4. Para enviar todo o tráfego para a nova revisão, marque a caixa de seleção identificada como *Veicular esta revisão imediatamente*. Para lançar uma nova revisão gradualmente, desmarque essa caixa de seleção: isso resultará em uma implantação em que nenhum tráfego será enviado para a nova revisão. Siga as instruções de [lançamentos graduais](https://cloud.google.com/run/docs/rollouts-rollbacks-traffic-migration#gradual) após a implantação.
5. Clique em **IMPLANTAR** e aguarde a conclusão da implantação.

## Como implantar imagens de outros projetos do Google Cloud

É possível implantar imagens de contêiner de outros projetos do Google Cloud se você definir as permissões corretas do IAM:

1. No console do Google Cloud, abra o projeto do seu serviço do Cloud Run.

   [Acessar a página do IAM](https://console.cloud.google.com/iam-admin/iam)

2. Marque a caixa de seleção *Incluir concessões de papéis fornecidos pelo Google*.

3. Copie o e-mail do [agente de serviço](https://cloud.google.com/iam/docs/service-agents) do Cloud Run. Ele tem o sufixo **@serverless-robot-prod.iam.gserviceaccount.com**

4. Abra o projeto com o registro do contêiner que você quer usar.

   [Acessar a página do IAM](https://console.cloud.google.com/iam-admin/iam)

5. Clique em **Adicionar** para adicionar um novo principal.

6. Na caixa de texto **Novos principais**, cole o e-mail da conta de serviço que você copiou anteriormente.

7. Na lista suspensa *Selecionar um papel*, se você estiver usando o Container Registry, selecione o papel **Armazenamento -> Visualizador de objeto do armazenamento**. Se você estiver usando o Artifact Registry, selecione o papel **Artifact Registry -> Artifact Registry Reader**.

8. [Implante a imagem do contêiner](https://cloud.google.com/run/docs/deploying#deploying_a_new_service) no projeto que contém o serviço do Cloud Run.

   **Observação** :para aumentar a segurança, [conceda acesso apenas ao bucket do Cloud Storage ou ao repositório do Artifact Registry que contém as imagens de contêiner](https://cloud.google.com/artifact-registry/docs/access-control#gcp).

## Como implantar imagens de registros não compatíveis

Se você estiver armazenando imagens de contêiner em um registro de contêiner público ou privado não compatível, é possível enviá-las temporariamente para o [Artifact Registry](https://cloud.google.com/artifact-registry/docs/overview) usando `docker push` para implantá-las no Cloud Run. A imagem do contêiner é importada pelo Cloud Run quando implantada, de modo que, depois da implantação, é possível [excluir a imagem do Artifact Registry](https://cloud.google.com/artifact-registry/docs/docker/manage-images#deleting_images).

## A seguir

Depois de implantar um novo serviço, é possível realizar estas ações:

- [Reversões, lançamentos graduais e migração de tráfego](https://cloud.google.com/run/docs/rollouts-rollbacks-traffic-migration)
- [Visualizar registros de serviço](https://cloud.google.com/run/docs/logging)
- [Monitorar o desempenho do serviço](https://cloud.google.com/run/docs/monitoring)
- [Definir limites de memória](https://cloud.google.com/run/docs/configuring/memory-limits)
- [Definir as variáveis de ambiente](https://cloud.google.com/run/docs/configuring/environment-variables)
- [Alterar a simultaneidade do serviço](https://cloud.google.com/run/docs/configuring/concurrency)
- [Gerenciar o serviço](https://cloud.google.com/run/docs/managing/services)
- [Gerenciar revisões de serviço](https://cloud.google.com/run/docs/managing/revisions)
- [Implantar somente imagens confiáveis com a autorização binária](https://cloud.google.com/binary-authorization/docs/run/enabling-binauthz-cloud-run) ([Visualizar](https://cloud.google.com/products#product-launch-stages))

Automatize as compilações e as implantações dos serviços do Cloud Run usando os gatilhos do Cloud Build:

- [Configurar implantação contínua](https://cloud.google.com/run/docs/continuous-deployment)

Também é possível usar o Google Cloud Deploy para configurar um pipeline de entrega contínua que implanta serviços do Cloud Run em vários ambientes:

- [Implantar um app no Cloud Run usando o Google Cloud Deploy](https://cloud.google.com/deploy/docs/deploy-app-run)