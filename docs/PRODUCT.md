# Visão do produto

## Visão

O One Photo a Day será um diário visual pessoal para iPhone. Sua função é ajudar o usuário a guardar um momento representativo de cada dia sem transformar o hábito em uma tarefa demorada.

Com o passar do tempo, os registros formarão uma memória visual privada, navegável por calendário e por timeline.

## Problema

A biblioteca de fotos do celular registra muita coisa, mas não ajuda necessariamente a contar a história cotidiana do usuário. As imagens mais significativas se misturam a conteúdos utilitários, e manter um diário tradicional exige constância e esforço de escrita.

Existe espaço para uma experiência mais intencional: escolher somente uma imagem que represente cada dia e encontrá-la depois com facilidade.

## Proposta de valor

Oferecer a forma mais simples possível de construir um diário visual:

- uma decisão pequena por dia;
- nenhum cadastro ou configuração obrigatória para começar;
- dados sob controle do usuário;
- acesso aos registros mesmo sem internet;
- interface familiar para quem usa iOS.

## Público-alvo

Pessoas que desejam guardar memórias cotidianas de maneira visual e privada, mas não querem manter um diário complexo nem publicar sua rotina em uma rede social.

O produto deve funcionar tanto para quem já cultiva hábitos diários quanto para quem precisa de uma experiência muito simples para começar.

## Princípios do produto

1. **Uma foto por dia.** Cada dia civil tem no máximo uma fotografia principal. Essa restrição é a identidade do produto.
2. **Simplicidade.** Registrar e rever uma memória deve exigir o mínimo de passos e decisões.
3. **Local-first.** As funções essenciais e os dados do MVP ficam disponíveis no dispositivo, sem depender de servidor ou conexão.
4. **Privacidade.** Não há conta, feed público ou envio intencional de fotos para um backend do produto.
5. **Experiência nativa.** Navegação, permissões, câmera, acessibilidade e notificações devem seguir os padrões do iOS.
6. **Compreensão imediata.** A tela inicial deve deixar claro o estado do dia e a próxima ação, sem tutorial obrigatório.

## Experiência principal

```text
Abrir o app
    ↓
Ver o dia atual e seu estado
    ↓
Tirar uma foto
    ↓
Revisar e confirmar
    ↓
Registro concluído
```

Se já existir um registro para o dia, a entrada principal passa a ser a própria foto, com ações explícitas para visualizá-la, substituí-la ou excluí-la.

Calendário e timeline são formas complementares de revisitar a memória: o calendário responde “em quais dias registrei?”, enquanto a timeline responde “como esses momentos se sucederam?”.

## Objetivos

- permitir que um novo usuário entenda e conclua o primeiro registro sem instruções extensas;
- tornar o registro diário rápido e previsível;
- preservar a regra de uma fotografia principal por dia;
- oferecer consulta agradável a um histórico crescente;
- operar integralmente offline no MVP;
- criar uma base técnica simples, testável e preparada para evolução incremental.

## Não objetivos

- substituir a biblioteca Fotos do iOS;
- oferecer edição fotográfica avançada;
- criar uma rede social ou mecanismo de descoberta de pessoas;
- maximizar tempo de tela, engajamento público ou produção de conteúdo;
- armazenar dados em backend próprio no MVP;
- oferecer sincronização ou backup gerenciado pelo app no MVP;
- suportar múltiplas fotos principais por dia;
- antecipar recursos futuros com abstrações ou infraestrutura que o MVP não exige.
