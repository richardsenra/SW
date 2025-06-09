# Reprocessador em Lote para Bitrix24

Esta é uma ferramenta de interface web (front-end) projetada para iniciar workflows (processos de negócio) em massa para uma lista de IDs (Leads, Negócios, Contatos, etc.) no Bitrix24. A aplicação foi construída para ser segura, controlada e fácil de usar, permitindo que usuários não técnicos executem tarefas de reprocessamento sem a necessidade de interagir com scripts ou código.

A ferramenta é um único arquivo HTML que pode ser executado em qualquer navegador moderno.

---

## ✨ Funcionalidades Principais

* **Interface Gráfica Intuitiva:** Todas as configurações são feitas através de um formulário simples.
* **Upload de Arquivo:** Suporte para carregar planilhas com IDs nos formatos `.xlsx` e `.csv`.
* **Processamento em Lote (Batch):** Otimiza as chamadas à API do Bitrix24, enviando comandos em pacotes para maior eficiência.
* **Controle de Carga:** Permite configurar o tamanho do lote e o tempo de pausa entre eles para evitar sobrecarga no servidor.
* **Retomada de Processo:** Opção para pular um número específico de itens e continuar um trabalho que foi interrompido.
* **Monitoramento em Tempo Real:** Barra de progresso visual, contador de itens e estimativa de tempo para conclusão (ETA).
* **Tratamento de Erros Críticos:** O processo é interrompido automaticamente em caso de erros de **limite de tempo (`OPERATION_TIME_LIMIT`)** ou de **permissão (`insufficient_scope`)**, com um alerta claro para o usuário.
* **Logs Detalhados:**
    * **Log Simplificado:** Para um acompanhamento geral do progresso.
    * **Log Detalhado:** Registra o `payload` enviado e a `resposta` recebida da API para cada lote, ideal para depuração.
    * **Download de Logs:** Permite baixar o histórico completo da execução em arquivos `.txt`.

---

## 🚀 Como Usar

1.  **Abra o Arquivo:** Salve o arquivo `html` da aplicação em seu computador e abra-o com qualquer navegador (Google Chrome, Firefox, etc.).
2.  **Preencha as Configurações:**
    * **Seu Nome:** Digite seu nome para identificação nos logs.
    * **URL do Webhook:** Cole o URL completo do webhook de entrada do Bitrix24. A aplicação extrairá o nome da conta (`ex: minhaempresa`) a partir deste URL para os logs.
    * **ID do Template de Workflow:** Insira o número de identificação do workflow que você deseja iniciar.
    * **Tipo da Entidade:** Selecione o módulo correspondente aos IDs (Negócio, Lead, etc.).
    * **Arquivo com IDs:** Clique para selecionar a planilha (`.xlsx` ou `.csv`) que contém a lista de IDs a serem processados.
    * **Nome da Coluna com IDs:** Confirme o nome da coluna na sua planilha que contém os IDs (o padrão é "ID").
    * **(Opcional) Pular os Primeiros N Itens:** Se precisar continuar um processo que parou, insira aqui o número de itens que já foram processados com sucesso.
    * **Tamanho do Lote e Pausa:** Ajuste esses valores para controlar a carga na API. Recomenda-se começar com valores conservadores (ex: Lote de 15, Pausa de 2.5s).
3.  **Inicie o Processamento:** Clique no botão "Iniciar Processamento". Os campos do formulário serão desabilitados durante a execução.
4.  **Acompanhe o Progresso:**
    * A barra de progresso, o contador e a estimativa de tempo serão atualizados em tempo real.
    * Use as abas **"Log Simplificado"** e **"Log Detalhado"** para monitorar a execução.
5.  **Finalização ou Erro:**
    * Ao final, o status indicará que o processo foi concluído.
    * Se um erro crítico ocorrer, o processo será interrompido e um pop-up com instruções aparecerá.

---

## ⚠️ Tratamento de Erros Comuns

A aplicação foi projetada para parar automaticamente ao detectar os seguintes erros críticos:

### 1. Permissão Insuficiente (`insufficient_scope`)

* **Causa:** O webhook utilizado não possui as permissões necessárias para executar a ação. Para iniciar um workflow, a permissão **"Workflows (bizproc)"** é obrigatória.
* **Solução:**
    1.  Vá até a seção de "Aplicações" -> "Webhooks" no seu Bitrix24.
    2.  Edite o webhook em questão.
    3.  Na seção de permissões, marque a opção **"Workflows (bizproc)"**.
    4.  Salve as alterações e tente executar o processo novamente na aplicação.

### 2. Limite de Tempo da Operação (`OPERATION_TIME_LIMIT`)

* **Causa:** O Bitrix24 não conseguiu processar todos os comandos de um lote dentro do tempo máximo permitido pelo servidor. Isso geralmente acontece quando o workflow é complexo e o **Tamanho do Lote** é muito grande.
* **Solução:**
    1.  Reduza o valor no campo **"Tamanho do Lote"** no formulário (ex: de 20 para 10).
    2.  Você pode usar o campo **"Pular os Primeiros N Itens"** para continuar de onde parou.
    3.  Tente executar o processo novamente.