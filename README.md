# 🚀 Disparos - Automação de Prospecção via n8n

Este repositório contém um fluxo de automação desenvolvido em **n8n** para disparos de mensagens de prospecção no WhatsApp. O fluxo lê leads de uma planilha do Google Sheets, verifica se o número possui WhatsApp ativo, envia uma mensagem personalizada e atualiza a planilha com o status do envio.

## ⚙️ Como Funciona

A automação é acionada todos os dias às 08:00 e executa as seguintes etapas:

1. **Busca de Leads:** Conecta-se ao Google Sheets e filtra as linhas onde a coluna `envio` está marcada como `PENDENTE`.
2. **Loop de Disparo:** Separa os dados em lotes (Split In Batches) para processar um lead por vez.
3. **Validação de Número:** Utiliza a Evolution API para checar se o número do lead realmente está registrado no WhatsApp.
4. **Condicional (If):**
   - Se o número **existir**: Envia um texto de prospecção oferecendo uma IA para WhatsApp e atualiza a planilha para `DISPARADO`.
   - Se o número **não existir**: Atualiza a planilha para `NUMERO INCORRETO`.
5. **Sistema Anti-Ban:** Aplica um delay aleatório entre 30 e 90 segundos via código JavaScript antes de passar para o próximo lead, simulando comportamento humano.

## 🛠️ Pré-requisitos

Para rodar este fluxo no seu n8n, você precisará de:

* [n8n](https://n8n.io/) (versão compatível com a v1).
* [Evolution API](https://github.com/EvolutionAPI/evolution-api) configurada e rodando (para os disparos e checagem de números).
* Conta no Google Cloud com a **Google Sheets API** ativada.
* Uma planilha no Google Sheets contendo as seguintes colunas:
  * `nome` (Nome do Lead)
  * `numero` (Telefone com DDD, sem caracteres especiais)
  * `envio` (Status do envio: preencher com PENDENTE inicialmente)

## 📦 Instalação

1. Faça o download do arquivo `Disparos.json` deste repositório.
2. Abra o seu painel do n8n.
3. Crie um novo Workflow.
4. Clique na engrenagem no canto superior direito e selecione **Import from File...**
5. Escolha o arquivo JSON baixado.
6. Configure as suas credenciais:
   * **Evolution account**: Insira os dados da sua instância da Evolution API.
   * **Quattro (Google Sheets)**: Autentique sua conta do Google.
7. Atualize o ID da Planilha nos nós do Google Sheets para apontar para a sua planilha real.
8. Ative o fluxo (Active) no canto superior direito.

## ⚠️ Avisos e Boas Práticas

* **Delay Anti-Ban:** O script atual possui um atraso dinâmico (30s a 90s). Dependendo do "aquecimento" do seu chip, você pode querer alterar essas variáveis no nó `Code in JavaScript`.
* **Aquecimento de Chip:** Não utilize chips novos para disparos em massa. Certifique-se de que a instância conectada na Evolution API possui histórico de conversas para evitar bloqueios no WhatsApp.

---
*Desenvolvido por Lucas dos Santos lima*
