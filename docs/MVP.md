# Escopo do MVP

## Objetivo

O MVP deve comprovar que o usuário consegue criar e manter um diário visual privado com uma única foto por dia, além de revisitar os registros por calendário e timeline.

Toda capacidade descrita neste documento é planejada. Ainda não há implementação no repositório.

## Funcionalidades obrigatórias

### 1. Registro diário

- Mostrar o dia civil atual e indicar claramente se ele possui registro.
- Abrir a câmera a partir do app.
- Permitir revisar a imagem capturada antes de confirmá-la.
- Salvar a fotografia confirmada como registro do dia atual.
- Impedir a criação de mais de um `DailyPhoto` para o mesmo dia civil.

O MVP não exige preenchimento retroativo de dias sem registro. A captura cria um registro para o dia atual segundo a regra de calendário definida em [DATA_MODEL.md](DATA_MODEL.md).

### 2. Visualização e manutenção

- Exibir a foto registrada e a data à qual ela pertence.
- Permitir substituir a imagem de um registro existente, sem criar um segundo registro para a data.
- Pedir confirmação antes da substituição definitiva e antes da exclusão.
- Permitir excluir um registro e o respectivo arquivo de imagem.
- Apresentar estados compreensíveis quando câmera, arquivo ou permissão não estiverem disponíveis.

Registros anteriores que já existem podem ser substituídos ou excluídos. Isso não permite criar um registro retroativo para uma data vazia.

### 3. Calendário

- Exibir um calendário mensal.
- Diferenciar visualmente dias com e sem registro.
- Permitir navegar entre meses que contenham ou possam conter histórico local.
- Permitir selecionar um dia com registro e abrir seus detalhes.

### 4. Timeline

- Listar registros em ordem cronológica, inicialmente do mais recente para o mais antigo.
- Exibir, no mínimo, miniatura e data de cada registro.
- Abrir o detalhe de um registro selecionado.
- Carregar imagens de forma gradual para não manter todas as fotos em resolução completa na memória.

### 5. Lembrete diário

- Permitir ativar e desativar um lembrete local.
- Permitir escolher o horário diário do lembrete.
- Solicitar autorização de notificações no contexto da configuração, sem bloquear o primeiro uso do app.
- Refletir na interface quando o sistema negar ou restringir a permissão.

No MVP, o lembrete é recorrente e independe de já existir uma foto no dia. Torná-lo condicional ao estado diário pode ser avaliado depois de validar a experiência básica.

### 6. Funcionamento offline

- Executar captura, armazenamento, consulta, substituição, exclusão, calendário e timeline sem internet.
- Persistir metadados e imagens no armazenamento privado do aplicativo.
- Não depender de backend, conta ou serviço remoto.

## Critérios de conclusão

O MVP estará concluído quando:

1. todos os requisitos do MVP em [REQUIREMENTS.md](REQUIREMENTS.md) estiverem implementados e validados;
2. o fluxo principal puder ser concluído em um iPhone físico: abrir, capturar, revisar, confirmar e rever a foto;
3. tentativas repetidas no mesmo dia resultarem em substituição explícita, nunca em dois registros;
4. registros continuarem disponíveis após encerrar e reabrir o app;
5. substituição e exclusão mantiverem metadados e arquivos consistentes, inclusive após falhas recuperáveis;
6. calendário e timeline representarem corretamente o mesmo conjunto de registros;
7. os fluxos essenciais funcionarem com o dispositivo sem conexão;
8. o lembrete puder ser configurado, alterado e desativado;
9. permissões negadas, falta de espaço e arquivo ausente tiverem tratamento compreensível, sem perda silenciosa de dados;
10. os fluxos principais forem utilizáveis com VoiceOver e tamanhos de texto maiores;
11. testes automatizados cobrirem, no mínimo, a identidade do dia civil, a unicidade, as operações de substituição/exclusão e a lógica de consulta;
12. não houver dependência, backend ou funcionalidade fora do escopo adicionada sem uma nova decisão explícita.

## Fora do MVP

- Android;
- iPad como experiência específica;
- backend ou API remota;
- autenticação, login ou conta de usuário;
- rede social, seguidores, comentários, likes ou feed público;
- inteligência artificial;
- compartilhamento social;
- mapas e organização por localização;
- edição avançada de imagem;
- filtros fotográficos;
- importação da biblioteca como fluxo principal;
- múltiplas fotos principais por dia;
- preenchimento retroativo de dias vazios;
- CloudKit ou sincronização por iCloud;
- sincronização entre dispositivos;
- retrospectiva automática;
- geração de timelapse ou vídeos;
- widgets, streaks e estatísticas.

Itens fora do MVP são possibilidades, não compromissos. Alguns aparecem em [ROADMAP.md](ROADMAP.md) apenas para registrar caminhos futuros.
