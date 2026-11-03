# AgendaPet Web 🐾

Painel de Agendamentos para Pet Shop.

> 📋 **Quadro Kanban:** https://trello.com/b/CmB1XYlu

> 🌐 **GitHub Pages (opcional/bônus):** [link, se publicou]

## 1. O que o sistema faz e para quem serve

A atendente do pet shop usa o painel (no celular ou tablet, na bancada) para:

* Ver o catálogo de serviços com preço e duração (carregado por fetch de um JSON);
* Agendar atendimentos com validação (data, horário de funcionamento, telefone…);
* Preencher o endereço de atendimentos a domicílio só com o CEP (API ViaCEP);
* Ser avisada de feriados na data escolhida (API BrasilAPI), com cache;
* Acompanhar a agenda do dia (busca, filtro por status, concluir/cancelar, atalhos de teclado);
* Receber lembretes de atendimentos próximos e ver os atrasados em destaque;
* Ver o fechamento do dia (faturamento, a receber, atendimentos por serviço);
* Exportar a agenda em JSON e fechar o dia pelo terminal com Node.js.

Tudo fica salvo no navegador (localStorage) — ao recarregar a página nada se perde.


`![Tela](docs/tela-1280.png)`.

## 2. Como executar (do zero)

Pré-requisitos: Node.js V24.20.0 e Git.

```bash
git clone https://github.com/carolinafazzioni/agendapet-web
cd agendapet-web
npm install
npm start
```

|     **Comando**      |                  **O que faz**                                |
|----------------------|---------------------------------------------------------------|
| `npm start`          | Executa o script start, se estiver configurado no package.json|
| `npm run fechamento` | Executa a script fechamento definida no package.json          |
| `npm run validar`    | Executa a script validar definida no package.json             |
|--------------------------------------------------------------------------------------|

## 3. Decisões de negócio

|          **Decisão**          |                             **Valor**                            |      **Onde está no código**             |
|-------------------------------|------------------------------------------------------------------|------------------------------------------|
| Horário de funcionamento      | [ex.: 08:00 às 18:00]                                            | CONFIG em js/dominio/agendamento.js      |
|---------------------------------------------------------------------------------------------------------------------------------------------|
| Conflito de horário           | Dois agendamentos ativos (Agendado ou Concluído) no mesmo        | conflitaCom() e temConflito()            |
|                               | dia e hora não podem coexistir. Cancelado libera o horário       |                                          |
|---------------------------------------------------------------------------------------------------------------------------------------------|
| Taxa domiciliar               | R$ 15,00 se a casa é em Florianópolis/SC (cidade da loja);       | AgendamentoDomiciliar.taxaDeslocamento() |
|                               | R$ 30,00 em outra cidade                                         |                                          |
|---------------------------------------------------------------------------------------------------------------------------------------------|
| Faturamento                   | Soma só dos Concluídos (com taxa). "A receber" soma os Agendados | calcularFechamento()                     |
|---------------------------------------------------------------------------------------------------------------------------------------------|
| Protocolo                     | AP-0001, AP-0002… sequencial; continua do maior número já salvo  | criarGeradorProtocolo()                  |
|---------------------------------------------------------------------------------------------------------------------------------------------|
| Validade do cache de feriados | Consulta a BrasilAPI por ano; cache de 30 dias no localStorage.  | api.js / storage.js                      |
|                               | Em feriado, é preciso marcar "Agendar mesmo assim"               |                                          |
|---------------------------------------------------------------------------------------------------------------------------------------------|
| Quando as APIs falham         | Nunca bloqueiam o trabalho: CEP → preenchimento manual;          | app.js                                   |
|                               | feriados → aviso e segue; serviços → botão "Tentar novamente"    |                                          |
|---------------------------------------------------------------------------------------------------------------------------------------------|
| Lembretes                     | Atualiza a cada 30 s; avisa quando faltam 15 min ou menos;       | atualizarRelogio()                       |
|                               | "Atrasado" = ainda Agendado e o horário passou                   |                                          |
|---------------------------------------------------------------------------------------------------------------------------------------------|

## 4. Conceitos do Módulo 01: onde foram aplicados

|                    **Conceito**                 |                                **Arquivo e função**                                 |
|-------------------------------------------------|-------------------------------------------------------------------------------------|
| HTML semântico, acessibilidade e SEO            | index.html: header, nav, main, section, aside, footer; um só h1; lang;              |
|                                                 | meta description skip link; label/aria-describedby/aria-live/role                   |
|---------------------------------------------------------------------------------------------------------------------------------------|
| Formulários + JavaScript                        | submit com preventDefault, validação com mensagens na tela e foco no                |
|                                                 | 1º erro — app.js (aoEnviar), regras.js (validarAgendamento)                         |
|---------------------------------------------------------------------------------------------------------------------------------------|
| CSS, Flexbox, variáveis e responsividade        | css/style.css (mobile-first, breakpoints 640px e 1024px, tema escuro por variáveis) |
|---------------------------------------------------------------------------------------------------------------------------------------|
| DOM e seletores                                 | ui.js: createElement, textContent, classList, querySelector(All), getElementById    |
|---------------------------------------------------------------------------------------------------------------------------------------|
| Eventos de mouse e teclado                      | app.js: click, mouseover/mouseout (delegação), input, keydown (atalhos)             |
|---------------------------------------------------------------------------------------------------------------------------------------|
| `fetch`, Promises e `async/await`               | api.js (serviços, ViaCEP, BrasilAPI) com 3 estados e erros por tipo (ErroApi)       |
|---------------------------------------------------------------------------------------------------------------------------------------|
| `localStorage`                                  | storage.js (agenda, filtro, tema, cache de feriados) com try/catch                  |
|---------------------------------------------------------------------------------------------------------------------------------------|
| `setTimeout` / `setInterval`                    | ui.js (toast some em 4 s) · app.js (relógio/lembretes a cada 30 s                   |
|                                                 | com clearInterval; confirmação de "limpar" por 4 s)                                 |
|---------------------------------------------------------------------------------------------------------------------------------------|
| Classes, construtor e `this`                    | Servico, Agendamento em agendamento.js                                              |
|---------------------------------------------------------------------------------------------------------------------------------------|
| Herança (`extends`/`super`)                     | AgendamentoDomiciliar extends Agendamento (acrescenta endereco, sobrescreve         |
|                                                 | calcularValor() e resumo())                                                         |
|---------------------------------------------------------------------------------------------------------------------------------------|
| Closure                                         | criarGeradorProtocolo() — o contador fica preso na função                           |
|---------------------------------------------------------------------------------------------------------------------------------------|
| Callback                                        | filtrarAgendamentos(lista, ...criterios) recebe funções (porStatus, porBusca)       |
|---------------------------------------------------------------------------------------------------------------------------------------|
| Métodos de array (mínimo 4) e laços             | filter, map, find, some, every, reduce, sort em fechamento.js                       |
|                                                 | for...of (montarLinhasRelatorio) e for aninhado (validar-agenda.cjs)                |
|---------------------------------------------------------------------------------------------------------------------------------------|
| `const`, `let` e `var` (e por que evitar `var`) | Usei const por padrão e let só quando o valor muda. var não é usado: ele tem escopo |
|                                                 | de função (não de bloco) e sofre hoisting, o que gera bugs difíceis; let/const      |
|                                                 | resolvem isso                                                                       |
|---------------------------------------------------------------------------------------------------------------------------------------|
| Módulos ESM                                     | package.json, scripts/validar-agenda.cjs, scripts/fechamento-dia.js                 |
|---------------------------------------------------------------------------------------------------------------------------------------|
| Node.js, npm e CommonJS                         | package.json, scripts/validar-agenda.cjs, scripts/fechamento-dia.js                 |
|---------------------------------------------------------------------------------------------------------------------------------------|
| Estrutura de decisão                            | if/else, switch (aoClicarNaAgenda), operador ternário (ui.js)                       |
|---------------------------------------------------------------------------------------------------------------------------------------|
| Depuração no VSCode                             | .vscode/launch.json + docs/bug-report.md                                            |
|---------------------------------------------------------------------------------------------------------------------------------------|

## 5. Arquitetura cliente-servidor

No modelo cliente-servidor, o cliente (o navegador) pede e o servidor responde. Aqui há três servidores diferentes:

Navegador (cliente)
   │ GET data/servicos.json ───────────► servidor local (http-server, npm start)
   │ GET viacep.com.br/ws/{cep}/json ──► servidor do ViaCEP (internet)
   │ GET brasilapi.com.br/api/feriados ─► servidor da BrasilAPI (internet)
   └ localStorage ─────────────────────► guarda dados NO navegador (não é servidor)

## 6. Gerenciadores de pacotes: instalação global vs local

|-------------------------|   **Local (npm install --save-dev http-server)**    |    **Global (npm install -g http-server)**                 |
|    Onde fica            | 	node_modules/ do projeto                        | Pasta global do Node, vale para o computador               |
| Registrado em           | package.json (devDependencies) e package-lock.json  | Nenhum lugar do projeto                                    |
| Quem tem a mesma versão | Todo mundo que clonar e rodar npm install           | Só quem instalou manualmente                               |
| Quando usar             | Ferramentas que precise (servidor, testes, lint)    | Ferramentas de uso geral na máquina (ex.: npm, npx, serve) |
|--------------------------------------------------------------------------------------------------------------------------------------------|

npm ls                 # dependências locais do projeto
npm ls -g --depth=0    # pacotes globais instalados na máquina
npx http-server --help # roda um pacote local (ou baixa e roda, sem instalar) com o npx

## 7. CommonJS vs ESM

|                            |                         CommonJS                      |                      ESM                       |
|----------------------------|-------------------------------------------------------|------------------------------------------------|
| Sintaxe                    | const fs = require('node:fs') / module.exports = ...  | import fs from 'node:fs' / export function ... |
| Extensão                   | .cjs (ou .js sem "type": "module")                    | .mjs (ou .js com "type": "module")             |
| Carregamento               | Síncrono                                              | Assíncrono (permite await no topo do arquivo)  |
| __dirname / __filename     | Existen                                               | Não existem — usa-se import.meta.url           |
| Funciona no navegador?     | Não                                                   | Sim (<script type="module">)                   |
|-------------------------------------------------------------------------------------------------------------------------------------|

Por que ESM como padrão: é o mesmo formato que o navegador entende. Assim, js/dominio/ (classes e regras) é escrito uma vez e usado pela página e pelos scripts do terminal. O package.json declara "type": "module".

Onde uso CommonJS: scripts/validar-agenda.cjs (usa require, __dirname e process.exit(1)). Para reaproveitar as regras do domínio (ESM), ele usa await import(...) dinâmico — a ponte que o CommonJS tem para carregar módulos ES.

## 8. Especificidade no CSS: um conflito real

**Conflito:** `.item--expandido { background: var(--superficie-2); }` não alterava o fundo de um item que também tinha a classe `.item--atrasado`.

**Causa:** os dois seletores têm a mesma especificidade — uma classe cada. Como `.item--atrasado` aparece depois no CSS, sua regra prevalece e mantém o fundo de alerta.

**Solução:** declare `.item--expandido` depois de `.item--atrasado` para que o estado expandido prevaleça quando ambos estiverem presentes. Isso resolve o conflito pela ordem das regras, sem `!important` nem IDs.


## 9. Depuração no VS Code

Foi utilizado o depurador do VS Code (F5), com a configuração disponível em `.vscode/launch.json`, para investigar um erro
no fechamento diário.  O problema ocorria porque o faturamento incluía atendimentos agendados que ainda não haviam sido concluídos.

A investigação permitiu identificar a diferença entre os atendimentos concluídos e os valores ainda a receber.

O faturamento correto é de R$ 515,00, enquanto o valor a receber é de R$ 385,00.

O relatório completo da investigação está em [docs/bug-report.md](docs/bug-report.md).

![Evidência da depuração](docs/depuracao.png)

## 10. Sprint: planejado × entregue

| | |
|---|---|
| Tickets planejados | [ ] |
| Pontos planejados | [ ] |
| Pontos entregues | [ ] |
| O que ficou de fora e por quê | [ ] |
| Branches / Pull Requests | [quantidade] |

Detalhes em [planejamento/sprint-planning.md](planejamento/sprint-planning.md).

### Retrospectiva

- **O que manter:** [ ]
- **O que parar de fazer:** [ ]
- **O que começar a fazer:** [ ]

## 11. Extensões do VS Code

Live Server, ESLint, Prettier, GitLens, Node.js, Claude.

## 12. Estrutura do projeto

```
[cole aqui a árvore de pastas do seu projeto]
```

## 13. Uso de IA

[Se usou, diga onde e o que você validou. Se não usou, escreva "Não utilizei".]