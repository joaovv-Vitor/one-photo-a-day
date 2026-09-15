# Roadmap

## Como ler este roadmap

As versões descrevem incrementos de escopo, não datas ou compromissos de entrega. Cada etapa deve resultar em software executável e testável antes de ampliar a próxima.

Tudo ainda está planejado; o repositório não contém uma versão implementada.

## V0.1 — Foundation

- criar o projeto iOS e definir deployment target compatível com a stack;
- estabelecer navegação e composição básica das dependências;
- configurar o contêiner SwiftData;
- implementar o modelo `DailyPhoto` e a identidade de dia civil;
- criar o `DailyPhotoStore` mínimo;
- cobrir unicidade e mudanças de dia/fuso com testes unitários;
- preparar estados vazios e tratamento básico de erros.

**Resultado esperado:** o app abre e consegue criar, consultar e remover metadados de teste sem imagens reais.

## V0.2 — Daily Capture

- integrar a câmera nativa e suas permissões;
- permitir captura, revisão e confirmação;
- implementar armazenamento local de imagens no `PhotoService`;
- exibir o registro do dia;
- substituir e excluir com confirmação;
- tratar falhas de câmera, arquivo e espaço;
- validar o fluxo em iPhone físico.

**Resultado esperado:** o fluxo diário completo funciona offline e preserva a regra de uma foto por dia.

## V0.3 — Calendar

- implementar calendário mensal;
- indicar dias com registro sem depender apenas de cor;
- navegar entre meses;
- abrir detalhes a partir de uma data;
- testar virada de mês, ano, localidade e fuso horário.

**Resultado esperado:** o histórico local pode ser encontrado por data com indicadores consistentes.

## V0.4 — Timeline & Reminders

- implementar timeline do mais recente para o mais antigo;
- adicionar miniaturas e carregamento gradual;
- integrar `UserNotifications`;
- permitir ativar, alterar e desativar o lembrete diário;
- adicionar configurações básicas e estados de permissão.

**Resultado esperado:** o usuário consegue percorrer seu histórico e configurar um lembrete local sem criar agendamentos duplicados.

## V1.0 — MVP

- integrar e revisar todos os fluxos;
- concluir tratamento de erros e reconciliação conservadora de arquivos;
- validar funcionamento offline;
- revisar acessibilidade, Dynamic Type e VoiceOver;
- medir memória e responsividade com uma base longa de registros;
- completar testes unitários, de integração e smoke tests de interface;
- refinar textos, estados vazios, confirmações e UX;
- preparar distribuição de teste e documentação de desenvolvimento.

**Resultado esperado:** todos os critérios de conclusão de [MVP.md](MVP.md) e requisitos de [REQUIREMENTS.md](REQUIREMENTS.md) estão atendidos.

## Possíveis versões futuras

As ideias abaixo devem passar por validação antes de entrar no escopo. A ordem não está definida.

### Backup e continuidade

- CloudKit/iCloud para sincronização e backup;
- sincronização entre dispositivos Apple;
- exportação e importação de arquivo local;
- restauração e migração assistidas.

CloudKit exige desenho explícito de conflitos, privacidade, exclusão e migração. Não deve ser habilitado apenas como detalhe de infraestrutura.

### Revisão de memórias

- retrospectiva anual;
- timelapse ou geração de vídeo;
- estatísticas pessoais;
- streaks opcionais, sem punição ou pressão excessiva;
- comparação do mesmo dia em anos diferentes.

### Ecossistema Apple

- widgets;
- atalhos e App Intents;
- experiência dedicada para iPad;
- compartilhamento privado ou exportação para Fotos, sob ação explícita do usuário.

### Outras plataformas

- eventualmente Android, após validação do produto e definição de uma estratégia de dados compatível.

## Limites permanentes de produto

Mesmo em versões futuras, o One Photo a Day não deve se transformar em rede social. Feed público, seguidores, likes e comentários não fazem parte da visão do produto.

Qualquer evolução deve preservar a compreensão imediata, a privacidade e a regra central de uma fotografia principal por dia.
