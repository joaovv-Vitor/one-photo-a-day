# Histórias de Usuário

## Convenções

As histórias seguem o modelo **3Cs**:

- **Card:** descreve, de forma curta, quem deseja algo, o que deseja e por quê.
- **Conversation:** registra regras de negócio, decisões e comportamentos esperados que precisam ser compreendidos pela equipe.
- **Confirmation:** define critérios objetivos usados para considerar a história concluída.

Os critérios de aceitação utilizam, quando apropriado, o formato **Dado / Quando / Então**.

Os requisitos não funcionais definidos em `REQUIREMENTS.md` são considerados restrições transversais do MVP e devem ser respeitados por todas as histórias aplicáveis.

---

## Épico 1 — Registro diário

### HU01 — Registrar a foto do dia

#### Card — HU01

> Como usuário, quero registrar uma fotografia do meu dia para construir um histórico visual ao longo do tempo.

#### Conversation — HU01

- O registro corresponde ao dia civil atual do dispositivo.
- Cada dia pode possuir no máximo uma fotografia principal.
- O usuário inicia a captura a partir da tela principal.
- Depois da captura, a fotografia deve ser apresentada para revisão.
- A fotografia só passa a ser considerada registrada depois da confirmação do usuário.
- Cancelar a captura ou a revisão não deve criar um registro.
- Caso já exista uma fotografia para o dia atual, o aplicativo não deve criar um segundo registro; deve direcionar o usuário para o registro existente ou para o fluxo de substituição.
- O registro deve permanecer disponível após fechar ou reiniciar o aplicativo.

#### Confirmation — HU01

##### Cenário 1 — Registrar uma fotografia

**Dado** que o dia atual ainda não possui uma fotografia  
**Quando** o usuário captura uma imagem e confirma seu uso  
**Então** a fotografia deve ser registrada para o dia atual.

##### Cenário 2 — Cancelar a captura

**Dado** que o dia atual ainda não possui uma fotografia  
**Quando** o usuário inicia a captura e cancela antes da confirmação  
**Então** nenhum registro deve ser criado.

##### Cenário 3 — Impedir dois registros no mesmo dia

**Dado** que o dia atual já possui uma fotografia  
**Quando** o usuário tenta registrar outra fotografia  
**Então** o aplicativo não deve criar um segundo registro para o mesmo dia.

##### Cenário 4 — Persistência

**Dado** que uma fotografia foi registrada com sucesso  
**Quando** o aplicativo é encerrado e aberto novamente  
**Então** o registro deve continuar disponível.

**Rastreabilidade:** RF01, RF02, RF14, RF15.

---

### HU02 — Identificar o estado do dia atual

#### Card — HU02

> Como usuário, quero saber se já registrei a fotografia de hoje para entender imediatamente qual ação está disponível.

#### Conversation — HU02

A tela principal possui dois estados principais:

##### Dia não registrado

- Deve indicar que ainda não existe fotografia para o dia atual.
- Deve oferecer a ação de registrar a fotografia.

##### Dia registrado

- Deve deixar evidente que o registro do dia já foi realizado.
- Deve permitir abrir a fotografia registrada.
- As ações relacionadas ao registro existente devem permanecer acessíveis.

Mudanças realizadas no registro devem refletir imediatamente na tela principal.

#### Confirmation — HU02

**Dado** que não existe registro para o dia atual  
**Quando** a tela principal é exibida  
**Então** o aplicativo deve apresentar o estado de dia não registrado e disponibilizar a ação de captura.

**Dado** que existe registro para o dia atual  
**Quando** a tela principal é exibida  
**Então** o aplicativo deve indicar que o dia já possui uma fotografia.

**Dado** que o registro atual é criado, substituído ou excluído  
**Quando** a operação termina  
**Então** o estado da tela principal deve ser atualizado sem reiniciar o aplicativo.

**Rastreabilidade:** RF03.

---

## Épico 2 — Gerenciamento de registros

### HU03 — Visualizar uma fotografia registrada

#### Card — HU03

> Como usuário, quero visualizar uma fotografia registrada para recordar o registro associado àquele dia.

#### Conversation — HU03

- O detalhe deve apresentar a fotografia registrada.
- A data associada ao registro deve estar claramente identificável.
- Um mesmo registro pode ser acessado pela tela principal, calendário ou timeline.
- Independentemente do ponto de entrada, o conteúdo exibido deve representar o mesmo registro.

#### Confirmation — HU03

**Dado** que existe um registro válido  
**Quando** o usuário o seleciona  
**Então** deve ser exibida a fotografia correspondente e sua data.

**Dado** que um mesmo registro aparece em diferentes áreas do aplicativo  
**Quando** o usuário o abre pelo calendário, timeline ou tela principal  
**Então** todas as entradas devem levar ao mesmo registro.

**Rastreabilidade:** RF04.

---

### HU04 — Substituir uma fotografia

#### Card — HU04

> Como usuário, quero substituir uma fotografia registrada para corrigir um registro sem criar outra entrada para o mesmo dia.

#### Conversation — HU04

- A substituição deve exigir intenção explícita do usuário.
- O aplicativo deve informar que a fotografia atual será substituída.
- O usuário captura e revisa a nova fotografia antes de confirmar.
- Cancelar o fluxo deve preservar a fotografia anterior.
- A substituição não altera a data do registro.
- Depois da confirmação, somente a nova fotografia deve representar aquele dia.
- Caso ocorra uma falha durante a substituição, o registro anterior deve permanecer utilizável.

#### Confirmation — HU04

**Dado** que existe uma fotografia registrada  
**Quando** o usuário solicita sua substituição  
**Então** o aplicativo deve solicitar confirmação antes de substituir o conteúdo existente.

**Dado** que o usuário inicia a substituição  
**Quando** ele cancela a operação  
**Então** a fotografia anterior deve permanecer inalterada.

**Dado** que o usuário confirma uma nova fotografia  
**Quando** a substituição é concluída  
**Então** o dia deve continuar contendo exatamente um registro e a nova fotografia deve ser exibida.

**Dado** que ocorre uma falha durante a substituição  
**Quando** a operação não consegue ser concluída  
**Então** a fotografia anterior não deve ser perdida.

**Rastreabilidade:** RF02, RF05; RNF03, RNF08.

---

### HU05 — Excluir uma fotografia

#### Card — HU05

> Como usuário, quero excluir uma fotografia registrada para remover um registro que não desejo manter.

#### Conversation — HU05

- A exclusão é uma operação destrutiva.
- Deve existir uma confirmação explícita antes da exclusão.
- Cancelar a confirmação preserva o registro.
- Depois da exclusão, o registro não deve aparecer em nenhuma área do aplicativo.
- Se o registro excluído pertencer ao dia atual, o dia deve voltar ao estado de não registrado.
- O usuário poderá registrar novamente uma fotografia caso o dia excluído seja o dia atual.

#### Confirmation — HU05

**Dado** que existe um registro  
**Quando** o usuário solicita sua exclusão  
**Então** o aplicativo deve solicitar confirmação.

**Dado** que a confirmação de exclusão está sendo exibida  
**Quando** o usuário cancela  
**Então** o registro deve permanecer inalterado.

**Dado** que o usuário confirma a exclusão  
**Quando** a operação termina  
**Então** o registro deve desaparecer da tela principal, calendário, timeline e demais pontos de acesso.

**Dado** que o registro excluído corresponde ao dia atual  
**Quando** a exclusão é concluída  
**Então** o usuário deve poder registrar novamente uma fotografia naquele dia.

**Rastreabilidade:** RF06.

---

## Épico 3 — Histórico

### HU06 — Consultar registros pelo calendário

#### Card — HU06

> Como usuário, quero visualizar meus registros em um calendário para identificar rapidamente em quais dias fotografei e acessar essas lembranças.

#### Conversation — HU06

- O calendário apresenta meses do calendário gregoriano.
- A apresentação deve respeitar a localidade configurada no dispositivo.
- Dias que possuem fotografia devem ser visualmente distinguíveis.
- O indicador de existência de fotografia não deve depender exclusivamente de cor.
- O usuário deve poder navegar entre meses.
- Selecionar um dia registrado deve abrir seu respectivo registro.
- Dias sem registro não devem abrir um registro inexistente.
- Alterações nos registros devem ser refletidas no calendário.

#### Confirmation — HU06

**Dado** que o usuário abre o calendário  
**Quando** um mês é exibido  
**Então** os dias devem corresponder corretamente ao mês e às convenções da localidade do dispositivo.

**Dado** que existem registros em determinados dias  
**Quando** o calendário é exibido  
**Então** esses dias devem possuir indicação visual de que existe uma fotografia.

**Dado** que existe uma fotografia em determinado dia  
**Quando** o usuário seleciona esse dia  
**Então** o registro correspondente deve ser aberto.

**Dado** que um registro é criado ou excluído  
**Quando** o usuário retorna ao calendário  
**Então** o indicador do respectivo dia deve refletir o novo estado.

**Rastreabilidade:** RF07, RF08, RF09.

---

### HU07 — Consultar registros pela timeline

#### Card — HU07

> Como usuário, quero visualizar minhas fotografias em uma timeline para percorrer meu histórico do registro mais recente para o mais antigo.

#### Conversation — HU07

- Todos os registros existentes devem poder aparecer na timeline.
- A ordenação é cronológica decrescente.
- Cada item deve permitir identificar visualmente o registro e sua data.
- A timeline deve utilizar miniaturas ou versões adequadas à visualização, evitando carregar desnecessariamente imagens em resolução completa.
- Selecionar um item abre seu registro.
- Criação, substituição e exclusão devem ser refletidas corretamente na timeline.

#### Confirmation — HU07

**Dado** que existem registros em diferentes datas  
**Quando** a timeline é aberta  
**Então** os registros devem aparecer do mais recente para o mais antigo.

**Dado** que um registro aparece na timeline  
**Quando** o usuário seleciona esse item  
**Então** o respectivo registro deve ser aberto.

**Dado** que um registro é criado, substituído ou excluído  
**Quando** a timeline é atualizada  
**Então** sua lista deve representar o estado atual dos registros.

**Rastreabilidade:** RF10; RNF04.

---

## Épico 4 — Lembretes

### HU08 — Ativar um lembrete diário

#### Card — HU08

> Como usuário, quero configurar um lembrete diário para reduzir a chance de esquecer minha fotografia do dia.

#### Conversation — HU08

- O lembrete é opcional.
- O usuário deve escolher o horário em que deseja ser lembrado.
- A solicitação de permissão para notificações ocorre somente quando o usuário tenta ativar o recurso.
- No MVP, o lembrete pode ser disparado mesmo quando a fotografia daquele dia já tiver sido registrada.
- O lembrete é local e não depende de conexão com a internet.
- Caso a permissão de notificações seja negada, o restante do aplicativo continua disponível.

#### Confirmation — HU08

**Dado** que o lembrete está desativado  
**Quando** o usuário escolhe um horário, concede a permissão necessária e confirma a ativação  
**Então** um lembrete diário deve ficar configurado para o horário escolhido.

**Dado** que o dispositivo está sem acesso à internet  
**Quando** chega o horário configurado  
**Então** o lembrete local continua apto a ser apresentado pelo sistema.

**Dado** que o usuário nega a permissão para notificações  
**Quando** tenta ativar o lembrete  
**Então** o aplicativo deve informar que o recurso não pôde ser ativado sem impedir o uso das demais funcionalidades.

**Rastreabilidade:** RF11, RF13, RF14.

---

### HU09 — Alterar ou desativar o lembrete diário

#### Card — HU09

> Como usuário, quero alterar ou desativar meu lembrete para adaptá-lo à minha rotina.

#### Conversation — HU09

- Deve existir no máximo um lembrete diário gerenciado pelo aplicativo.
- Alterar o horário substitui a configuração anterior.
- Alterações não devem criar lembretes duplicados.
- Desativar o recurso deve cancelar o lembrete anteriormente configurado.
- A configuração exibida deve representar o estado efetivamente gerenciado pelo aplicativo.

#### Confirmation — HU09

**Dado** que existe um lembrete configurado  
**Quando** o usuário altera seu horário  
**Então** o horário anterior deve ser substituído pelo novo.

**Dado** que existe um lembrete configurado  
**Quando** o usuário desativa o recurso  
**Então** o lembrete gerenciado pelo aplicativo deve ser removido.

**Dado** que o usuário altera o horário várias vezes  
**Quando** a configuração final é salva  
**Então** não devem existir lembretes duplicados gerenciados pelo aplicativo.

**Rastreabilidade:** RF12.

---

## Épico 5 — Permissões e recuperação

### HU10 — Recuperar-se de permissões indisponíveis

#### Card — HU10

> Como usuário, quero receber orientação quando uma permissão necessária estiver indisponível para entender como recuperar o recurso sem perder acesso ao restante do aplicativo.

#### Conversation — HU10

- A câmera só deve ser solicitada quando o usuário iniciar uma ação que necessite dela.
- A permissão de notificações só deve ser solicitada quando o usuário tentar ativar lembretes.
- Permissões negadas não devem impedir o acesso a funcionalidades independentes delas.
- Quando possível, o aplicativo deve informar como o usuário pode reabilitar a permissão.
- O aplicativo não deve solicitar repetidamente uma permissão de maneira inesperada ou sem contexto.

#### Confirmation — HU10

**Dado** que a permissão da câmera ainda não foi solicitada  
**Quando** o usuário apenas navega pelo calendário, timeline ou configurações  
**Então** o aplicativo não deve solicitar acesso à câmera.

**Dado** que o acesso à câmera está negado  
**Quando** o usuário tenta registrar ou substituir uma fotografia  
**Então** o aplicativo deve explicar que a câmera é necessária e oferecer orientação adequada.

**Dado** que a permissão de notificações está negada  
**Quando** o usuário tenta ativar um lembrete  
**Então** o aplicativo deve informar a limitação sem bloquear os demais recursos.

**Rastreabilidade:** RF13.

---

## Restrições transversais do MVP

Os requisitos abaixo não devem ser transformados em histórias artificiais apenas para preencher o backlog. Eles funcionam melhor como **critérios de qualidade aplicáveis às histórias relevantes** e como parte da Definition of Done.

| Aspecto | Aplicação |
| --- | --- |
| Privacidade local-first | Nenhuma história deve introduzir envio de fotografias, metadados ou métricas para serviços remotos. |
| Funcionamento offline | Captura, consulta, substituição, exclusão e configuração de lembrete devem funcionar sem conexão. |
| Persistência | Operações concluídas devem sobreviver a encerramento e reinicialização do aplicativo. |
| Consistência | Nenhuma operação deve deixar registros duplicados, referências quebradas ou perda silenciosa do estado válido anterior. |
| Desempenho | Processamento de fotografias não deve bloquear perceptivelmente a interface. |
| Memória | Calendário e timeline devem trabalhar com carregamento adequado e não manter todas as fotografias completas simultaneamente em memória. |
| Acessibilidade | Fluxos devem ser utilizáveis com VoiceOver, Dynamic Type e sem depender exclusivamente de cor. |
| Experiência iOS | Navegação, permissões, confirmações e operações destrutivas devem seguir os padrões da plataforma. |
| Tolerância a erros | Falhas de câmera, armazenamento e arquivos devem possuir tratamento compreensível e seguro. |
| Manutenibilidade | Regras de negócio e integrações de sistema devem permanecer separadas da camada de apresentação. |

## Rastreabilidade resumida

| História | Requisitos principais |
| --- | --- |
| HU01 — Registrar foto | RF01, RF02, RF14, RF15 |
| HU02 — Estado do dia | RF03 |
| HU03 — Visualizar registro | RF04 |
| HU04 — Substituir foto | RF02, RF05 |
| HU05 — Excluir registro | RF06 |
| HU06 — Calendário | RF07, RF08, RF09 |
| HU07 — Timeline | RF10 |
| HU08 — Ativar lembrete | RF11, RF13, RF14 |
| HU09 — Alterar/desativar lembrete | RF12 |
| HU10 — Permissões | RF13 |

## Definition of Done mínima para uma história

Uma história só deve ser considerada concluída quando:

1. todos os seus critérios de aceitação forem atendidos;
2. os requisitos não funcionais aplicáveis forem respeitados;
3. existirem testes automatizados para regras de negócio quando tecnicamente apropriado;
4. os principais estados de erro tiverem sido considerados;
5. acessibilidade tiver sido verificada no fluxo alterado;
6. não houver regressões conhecidas nos fluxos existentes.
