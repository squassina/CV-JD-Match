# CV–JD Match

Aplicativo web de página única (HTML/CSS/JS, sem build ou backend) que compara
um currículo com uma descrição de vaga (JD), calcula uma pontuação de
compatibilidade e — opcionalmente — gera uma versão do currículo e uma carta
de apresentação adaptadas à vaga, preservando o estilo original do candidato.

Tudo roda no navegador: os textos do currículo e da vaga, assim como a chave de
API, são enviados diretamente do seu navegador ao provedor de LLM escolhido —
nenhuma informação passa por um servidor intermediário do autor do projeto.

## Principais recursos (novos / destacados)

- Upload ou arraste & solte: importe arquivos `.txt`, `.md` ou `.pdf` para o CV e para a JD.
- Extração de texto de PDFs via PDF.js (o conteúdo textual é extraído para análise).
- Suporte a múltiplos provedores de LLM: OpenAI, Anthropic, Google e um servidor local compatível com `llama.cpp`.
- Configuração por provedor: chaves de API e modelo selecionável por provedor.
- Opção para lembrar a chave de API no navegador (localStorage) ou mantê-la apenas na sessão da aba (sessionStorage).
- Campo "Special instructions" para instruções adicionais que serão incluídas no prompt (p.ex. "Limitar CV a 1 página", "tom executivo").
- Análise estruturada que retorna:
  - pontuação de compatibilidade (0.0–1.0),
  - lista de prós (pontos do CV que batem com a JD),
  - lista de contras (requisitos críticos da JD ausentes no CV).
- "Seal" visual com resumo do nível de match e tabela detalhada de prós/contras.
- Geração contínua: após a análise, botão para gerar o CV adaptado e a carta de apresentação.
- Saída em formato markdown com botões para copiar, download (.md) e visualização (preview / raw).
- Download de PDF do CV gerado (client-side) usando bibliotecas de renderização.
- Área de depuração que mostra o output bruto do modelo quando o retorno JSON está malformado.
- Robusta extração/normalize de JSON: o código tenta extrair o primeiro objeto/array JSON mesmo se o modelo incluir markdown, blocos de código ou pequenos erros de vírgula.
- Recomendações de privacidade e instruções para uso com servidor local `llama.cpp` (CORS, servir o arquivo via HTTP se necessário).

## Como usar

1. Abra `index.html` em um navegador moderno:
   - Duplo clique no arquivo ou arraste-o para uma aba do navegador.
   - Para uso com servidor local (`llama.cpp`) é recomendado servir a página por HTTP (ex.: `python3 -m http.server`) para evitar problemas de origem nula.
2. Cole ou importe o texto do currículo e da descrição da vaga nos campos correspondentes (ou arraste os arquivos `.txt` / `.md` / `.pdf`).
3. Escolha o provedor de IA e informe a chave de API (se aplicável). Se quiser, selecione um modelo customizado.
4. (Opcional) Preencha "Special instructions" para orientar a geração (p.ex. limite de linhas, tom, prioridades).
5. Clique em **Analyze match** para executar a análise.
6. Revise a pontuação, o selo e a tabela de prós/contras.
7. Clique em **Continue → Generate tailored CV & cover letter** para criar os documentos adaptados.
8. Use os botões para copiar, baixar (.md) ou baixar PDF (CV) e alternar entre preview e raw.

## Comportamento de privacidade e persistência

- As chaves (se marcadas) são salvas apenas no armazenamento local do seu navegador (`localStorage`) ou apenas na sessão (`sessionStorage`), dependendo da opção escolhida.
- Por padrão, nenhuma informação é enviada a servidores do projeto — somente ao provedor de LLM selecionado.
- Se preferir não salvar chaves, desmarque "Remember API key"; a chave será mantida apenas na memória da aba atual.
- Há botão para limpar chaves salvas ("Clear saved keys").

## Formato e restrições do prompt

- O sistema força que o modelo retorne um JSON válido (sem blocos de markdown) contendo campos definidos (por exemplo: `match_score`, `pros`, `cons`, ou para geração: `tailored_cv`, `presentation_letter`).
- Instruções críticas embutidas no prompt incluem:
  - Preservar o estilo do CV original (bullet points, tom, linguagem).
  - Manter o CV gerado dentro de um limite prático (o prompt usa uma indicação de "máximo de 60 linhas / 70 chars por linha").
- Caso o modelo não retorne JSON válido, a interface exibirá a saída bruta na aba de depuração para facilitar correção do prompt / escolha do modelo.

## Arquivos suportados

- Texto puro: `.txt`, `.md`
- PDF: `.pdf` (texto extraído com PDF.js; nem todos os PDFs digitalizados com imagem terão texto selecionável — nesses casos, a extração falhará ou retornará pouco conteúdo)

## Dependências (CDN inclusas no HTML)

O HTML já carrega as bibliotecas necessárias via CDN:

- pdf.js — extração de texto de PDFs
- marked — renderização de markdown (visualização)
- html2canvas / jsPDF / html2pdf.js — geração de PDF client-side a partir do HTML/Markdown
- (Outras utilidades incluídas inline no script do próprio `index.html`)

## Usando com `llama.cpp` / servidor local

- Para usar um servidor local (campo "llama.cpp — local model"), faça com que o servidor ofereça uma API compatível com o endpoint esperado e permita CORS para o domínio/origem do navegador.
- Problemas comuns:
  - Origem "null" quando a página é aberta via `file://` pode fazer com que o servidor rejeite a requisição — sirva a página via HTTP para evitar isso.
  - Verifique o campo "Base URL" e o modelo carregado no servidor local.
  - Mensagem de erro detalhada é mostrada quando o cliente não consegue alcançar o servidor local.

## Solução de problemas

- "Add both a CV and a job description before continuing.": adicione texto em ambos os campos.
- "Enter an API key for the selected provider before continuing.": informe a chave para provedores que exigem API key.
- "Request timed out" / rede: a requisição ao provedor pode ter estourado o tempo limite; tente novamente ou aumente o timeout no código se desejar.
- "The model did not return valid JSON": abra a seção de depuração para ver a saída bruta do modelo; experimente outro modelo, reduz a temperatura ou adicione instruções mais restritivas.
- Ao usar PDFs com pouco ou nenhum texto (p.ex. imagens escaneadas), a extração retornará pouco conteúdo — prefira um `.txt` ou `.md` quando possível.

## Requisitos

- Navegador moderno (Chrome, Firefox, Edge, Safari atualizados).
- Conexão com a internet para provedores remotos (a menos que use um servidor local `llama.cpp`).
- Para melhor compatibilidade com servidor local, sirva a página via HTTP (ex.: `python3 -m http.server`) em vez de abri-la via `file://`.

## Licença

Este projeto é distribuído sob a **GNU AFFERO GENERAL PUBLIC LICENSE Version 3 (AGPL-3.0)**.
