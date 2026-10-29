# 🤖 Integração: Google Drive + Google Gemini API

Solução de automação que conecta o armazenamento em nuvem do **Google Drive** diretamente aos modelos de Inteligência Artificial do **Google Gemini**. O projeto permite ler, interpretar e extrair informações estruturadas de documentos armazenados no Drive de forma automatizada.

---

## 🎯 Objetivo do Projeto

Eliminar o trabalho manual de baixar arquivos, copiar textos e enviá-los individualmente para interfaces de IA. Com esta integração, o próprio script autentica na conta, localiza os documentos no Google Drive, envia o contexto para a API do Gemini e retorna a análise processada.

---

## ⚙️ Arquitetura e Fluxo de Execução

1. **Autenticação no Google Drive:** Conexão segura via credenciais de API para listar e acessar arquivos de uma pasta específica na nuvem.
2. **Extração de Conteúdo:** Leitura dos dados brutos ou documentos armazenados no Drive.
3. **Processamento com IA (Gemini API):** Envio do conteúdo extraído junto a instruções de *prompt* estruturadas para o modelo do Gemini.
4. **Saída / Resultado:** Retorno da análise, resumo ou extração de dados gerada pela IA diretamente para o fluxo de trabalho.

---

## 🛠️ Tecnologias e APIs Utilizadas

* **Linguagem:** [Ex: Python / Java / JavaScript - Google Apps Script]
* **Inteligência Artificial:** Google Gemini API (`google-generativeai` / Google AI Studio)
* **Integração em Nuvem:** Google Drive API / Google Workspace
* **Ambiente de Desenvolvimento:** VS Code / GitHub Codespaces

---

## 🔐 Configuração de Segurança e Variáveis de Ambiente

Por questões de segurança, as chaves de API e credenciais OAuth não estão incluídas neste repositório. Para executar o projeto localmente, configure:

1. Uma chave de API gerada no [Google AI Studio](https://aistudio.google.com/).
2. Credenciais de acesso à API do Google Drive (ou permissões do Apps Script).
3. Um arquivo `.env` na raiz do projeto seguindo o padrão:
   ```env
   GEMINI_API_KEY="sua_chave_aqui"
   DRIVE_FOLDER_ID="id_da_pasta_do_drive"
