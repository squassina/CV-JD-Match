# CV–JD Match

Aplicativo web de página única (HTML/CSS/JS, sem build ou backend) que compara
um currículo com uma descrição de vaga (JD), calcula uma pontuação de
compatibilidade e gera uma versão do currículo e uma carta de apresentação
adaptadas à vaga — usando a API de um provedor de LLM à sua escolha.

## Objetivo

A ideia é ajudar candidatos a entender rapidamente o quão bem seu currículo
se encaixa em uma vaga específica, e a partir disso:

- ver quais pontos do currículo já batem com o que a vaga pede (prós);
- ver quais requisitos importantes da vaga ainda não aparecem no currículo (contras);
- gerar automaticamente uma versão do currículo reorganizada para destacar
  os pontos mais relevantes para aquela vaga, mantendo o estilo original;
- gerar uma carta de apresentação no mesmo tom de voz do currículo.

Tudo roda no navegador: os textos do currículo e da vaga, assim como a chave
de API, nunca passam por um servidor intermediário — são enviados direto do
seu navegador para o provedor de IA escolhido.

## Como usar

1. Abra o arquivo `JobMatch.html` em qualquer navegador (basta dar duplo
   clique ou arrastar para uma aba).
2. Cole o texto do seu currículo e o texto da descrição da vaga nos campos
   correspondentes (ou importe arquivos `.txt`/`.md` pelos botões de import).
3. Escolha o provedor de IA (OpenAI, Anthropic, Google ou um servidor
   `llama.cpp` local) e informe a chave de API (não necessária para
   `llama.cpp` local) e, se quiser, o modelo específico a usar.
4. Clique em **Analyze match** para ver a pontuação de compatibilidade e a
   lista de prós/contras.
5. Clique em **Continue → Generate tailored CV & cover letter** para gerar o
   currículo adaptado e a carta de apresentação, prontos para copiar.

### Sobre a chave de API

- A chave fica salva apenas no armazenamento local do seu navegador
  (`localStorage`), e só se a opção "Remember API key" estiver marcada.
- Desmarcando essa opção, a chave é usada apenas durante a sessão da aba
  atual e é descartada ao fechar ou recarregar a página.
- Nenhuma informação — currículo, vaga ou chave — é enviada a nenhum servidor
  além do provedor de IA selecionado.

## Requisitos

Nenhuma instalação é necessária. É só um arquivo HTML autocontido; qualquer
navegador moderno é suficiente. Para usar com um modelo local, é preciso ter
o `llama-server` (do projeto [llama.cpp](https://github.com/ggml-org/llama.cpp))
rodando e acessível pelo navegador.

## Licença

Este projeto é distribuído sob a **GNU AFFERO GENERAL PUBLIC LICENSE Version 3 (AGPL-3.0)**.
