Guia de início rápido: implantar no Cloud Run

# Implantar no Cloud Run

bookmark_border

Nesta página, mostramos como implantar no Cloud Run um contêiner de exemplo que já foi enviado para um repositório do Artifact Registry.

## Antes de começar

1. 

2. 

3. 

4. Se você começou a usar o Google Cloud agora, [crie uma conta](https://console.cloud.google.com/freetrial) para avaliar o desempenho dos nossos produtos em situações reais. Clientes novos também recebem US$ 300 em créditos para executar, testar e implantar cargas de trabalho.

5. 

6. No console do Google Cloud, na página do seletor de projetos, selecione ou [crie um projeto do Google Cloud](https://cloud.google.com/resource-manager/docs/creating-managing-projects).

   **Observação**: se você não pretende manter os recursos criados neste procedimento, crie um projeto novo em vez de selecionar um que já existe. Depois de concluir essas etapas, é possível excluir o projeto. Para fazer isso, basta remover todos os recursos associados a ele.

   [Acessar o seletor de projetos](https://console.cloud.google.com/projectselector2/home/dashboard)

7. 

8. Verifique se o faturamento está ativado para seu projeto na nuvem. Saiba como [verificar se o faturamento está ativado em um projeto](https://cloud.google.com/billing/docs/how-to/verify-billing-enabled).

9. 

10. 

## Como implantar o contêiner de amostra

Para implantar um contêiner, siga estas etapas:

1. [Acesse o Cloud Run](https://console.cloud.google.com/run?enableapi=true)

2. Clique em **Criar serviço** para exibir o formulário *Criar serviço*.

   No formulário, siga estas etapas:

   1. Selecione **Implantar uma revisão de uma imagem de contêiner atual**.
   2. Clique em **Testar com um contêiner de amostra**.
   3. No menu suspenso *Região*, selecione a [região](https://cloud.google.com/run/docs/quickstarts/deploy-container#before-you-begin) onde o serviço será localizado.
   4. Em *Autenticação*, selecione **Permitir invocações não autenticadas**.
   5. Clique em **Criar** para implantar a imagem do contêiner de amostra no Cloud Run e aguarde a conclusão da implantação.

3. Clique no link do URL exibido para executar o contêiner implementado.

Parabéns! Você acabou de implantar no Cloud Run um contêiner que responde a solicitações da Web recebidas. O Cloud Run escalona horizontalmente o contêiner de maneira automática para lidar com as solicitações recebidas e escalona verticalmente quando a demanda diminui. Você paga apenas pela CPU, memória e rede consumida durante o processamento da solicitação.

## A seguir

Para aprender a compilar um contêiner da origem do código, enviar para o Container Registry e implantar, consulte estes tópicos:

- [Guia de início rápido: criar e implantar](https://cloud.google.com/run/docs/quickstarts/build-and-deploy)
- [Como criar contêineres](https://cloud.google.com/run/docs/building/containers)