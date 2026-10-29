# 🤖 Enriquecimento de Dados em Lote: Google Drive + Gemini API

Pipeline automatizado em Python (desenvolvido para Google Colab) que conecta planilhas do **Google Drive** à **API do Google Gemini** para enriquecimento massivo de cadastros de peças automotivas e industriais. 

O sistema lê planilhas contendo apenas **Marca** e **Código do Fabricante** e preenche automaticamente especificações logísticas de cubagem (**Peso, Altura, Largura e Comprimento**) utilizando processamento paralelo e lotes otimizados.

---

## 🎯 Problema de Negócio Resolvido

No setor de distribuição automotiva e logística (ERP/WMS), o cadastro incompleto de peso e dimensões de embalagens impede o cálculo correto de frete e o planejamento de armazenamento. Preencher milhares de SKUs manualmente é inviável. 

Este projeto automatiza essa busca técnica em larga escala, processando centenas de itens simultaneamente com alta economia de tokens e tolerância a falhas.

---

## ⚡ Diferenciais de Engenharia do Código

* **Processamento Paralelo (`ThreadPoolExecutor`):** Executa múltiplas requisições simultâneas (`MAX_WORKERS = 5`), processando até 500 itens paralelamente.
* **Thread Safety (`threading.Lock`):** Controle de concorrência com *mutex lock* para garantir que múltiplas threads atualizem o DataFrame do Pandas e gravem no disco sem *race conditions* ou corrupção de dados.
* **Sistema de Checkpoint & Retomada Automática:** Verifica a existência de arquivos parcialmente processados ao iniciar e salva o progresso periodicamente no Google Drive (`SALVAR_A_CADA_X_LOTES`). Se a conexão cair, o processamento retoma exatamente de onde parou.
* **Otimização de Tokens (Batching & JSON Compacto):** Agrupa 100 produtos por requisição (`TAMANHO_LOTE = 100`) com serialização JSON minificada e saída estruturada (`response_mime_type="application/json"` e `temperature=0.0`).
* **Resiliência contra Rate Limit (HTTP 429):** Estratégia de *retry* com *backoff* progressivo para contornar limites de cota da API sem interromper a execução.
* **Segurança de Credenciais:** Integração nativa com o gerenciador de segredos do Google Colab (`google.colab.userdata`), mantendo a `API_KEY` fora do código-fonte.

---

## 🛠️ Tecnologias Utilizadas

* **Linguagem:** Python 3
* **Manipulação de Dados:** Pandas (`read_excel`, `to_excel`)
* **Inteligência Artificial:** SDK Oficial Google GenAI (`google-genai`)
* **Concorrência:** `concurrent.futures` & `threading`
* **Ambiente & Nuvem:** Google Colab & Google Drive

---

## ⚙️ Fluxo de Funcionamento

1. **Montagem e Leitura:** O script monta o Google Drive e verifica se há um processamento em andamento (`produtos_preenchidos02.xlsx`) ou se deve iniciar a partir da base bruta (`produtos.xls.xlsx`).
2. **Filtragem de Pendentes:** Identifica apenas as linhas onde `Peso_kg` ou `Altura_cm` estão vazios.
3. **Empacotamento em Lotes:** Divide os itens pendentes em pacotes de 100 registros contendo `id`, `marca` e `cod_fabricante`.
4. **Consulta Paralela ao Gemini:** Envia os lotes em paralelo exigindo retorno estrito em array JSON com `Peso_kg`, `Altura_cm`, `Largura_cm` e `Comprimento_cm`.
5. **Persistência Incremental:** Atualiza o DataFrame de forma segura e sincroniza a planilha atualizada no Google Drive a cada 5 lotes concluídos.

---

## 🚀 Como Executar no Google Colab

### 1. Pré-requisitos
* Conta Google com acesso ao **Google Drive** e **Google Colab**.
* Chave de API gerada no [Google AI Studio](https://aistudio.google.com/).

### 2. Configuração
1. No menu lateral esquerdo do Google Colab, clique no ícone de **Chave (Secrets)** e adicione um novo segredo chamado `Chave_api` contendo sua API Key do Gemini.
2. Suba sua planilha no Google Drive contendo ao menos as colunas `Marca` e `Cod_Fabricante`.
3. Instale o SDK atualizado do Gemini (caso necessário na célula inicial do Colab):
   ```bash
   !pip install google-genai pandas openpyxl
