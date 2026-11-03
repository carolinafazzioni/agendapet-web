# Sprint Planning — AgendaPet Web

**Sprint:** 16/10/2026 a 02/11/2026 (Semanas 13 e 14) · **Cliente:** Patinhas & Cia · **Equipe:** Carolina Minuzzi Parmagnani Fazzioni

## Meta da sprint

Ao final da Sprit, a atendete do caixa conseguirá navegar e cadastrar pelo celular agendamentos de horários sem conflitos, enquanto a dona do Pet Shop poderá consultar a agenda e realizar o fechamento do dia pelo terminal.

## Backlog estimado

Pontos de história: **1, 2, 3, 5 ou 8** (compare a complexidade entre os cartões; não é tempo em horas).

| Or|Ticket|         História                |Pts|                  Por que nessa ordem                            | 
|---|------|---------------------------------|---|-----------------------------------------------------------------|
| 1 | US01 | Estrutura semântica             | 3 | US01 cria a estrutura base (HTML) para todo o projeto           |
| 2 | US10 | Layout responsivo               | 8 | Adapta estrututa inicial a diferentes telas (CSS)               |
| 3 | US02 | Catálogo de serviços            | 5 | Antes do formulário precisamos dos serviços para seleção        |
| 4 | US03 | Formulário de novo agendamento  | 8 | Após US02 podemos implementar os cadastros de agendamento       |
| 5 | US04 | Endereço automático por CEP     | 5 | Com o formulário consigo avançar para integrar o CEP            |
| 6 | US05 | Alerta de feriados              | 5 | Para melhor organizar as datas na agenda                        |
| 7 | US06 | Agenda do dia                   | 8 | Com o cadastro e data conseguimos visualizar os agendamentos    |
| 8 | US07 | Regras e fechamento do dia      | 8 | Com os agendamentos aplicamos as regras e o fechamento          |
| 9 | US08 | Exportação dos dados            | 8 | Com o fechamento, prioridade recuperar e salvar dados           |
| 10| US11 | Node.js e RF                    | 8 | Com fechamento, integramos os scripts erelatórios               |
| 11| US09 | Lembretes de atendimento        | 5 | Lembretes ficou depois de funcionalidades principais            |
| 12| US12 | Depuração e relatório de bug    | 5 | Etapa final de revisão e relatório de bugs                      |
|   |      |          **Total**             |**76**|                                                               |
|------------------------------------------------------------------------------------------------------------------|


## Riscos e plano B

|                         Risco                    |                            Plano                               |
|--------------------------------------------------|----------------------------------------------------------------|
| API ou ViaCEP fora do ar durante a demonstração  | Preenchimento manual do endereço, aviso de feriado indisponível|
|                                                  | e cache de feriados por 30 dias                                |
|--------------------------------------------------|----------------------------------------------------------------|
| Loyalt quebrar em 360 px                         | Conferir a largura pelo (DevTools, modo celular)               |
|--------------------------------------------------|----------------------------------------------------------------|
| Atraso no Git/Kanban (tudo no último dia)        | Criar a branch antes de começar o ticket                       |
|                                                  | e mover o cartão a cada etapa                                  |
|-------------------------------------------------------------------------------------------------------------------|

## O que seria cortado se faltasse tempo

1. GitHub pages
2. Estilos de impressão com @media.print
3. Exportação de arquivos baixaveis
4. Configuração do launch.json para depurar erros também no navegador, deixaria só o node.js    

## Definição de Pronto para Iniciar (cada cartão)

- [x] História lida e critérios de aceite entendidos
- [x] Ticket estimado em pontos
- [x] Branch criada no padrão `tipo/US##-descricao`

## Cerimônias

|     Data      |            O que           |             Resultado                   |
|---------------|----------------------------|-----------------------------------------|
|     16/10     |         Sprint Plannig     |             Leitura do projeto          |
| 20,22,23/10   |       Mentoria semana 13   |    Este documento + 12 cartões Kanban   |
|    27 e 29    |       Mentoria semana 14   |  Dúvidas e desenvolvimento das historias|
|    30/10      |   Apresentação live coding |    Dúvidas, correções e o que falta     |
|   31,01,02/10 |           Revisão          |        Revisão e últimos ajustes        |
|   02 e 03/10  |    Entrega do projeto AVA  |     Todos os ítens e links enviados     |
|--------------------------------------------------------------------------------------|