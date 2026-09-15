# Modelo de dados

## Estado

O modelo abaixo é uma proposta inicial. Nenhuma entidade SwiftData foi implementada ainda.

## Entidade `DailyPhoto`

`DailyPhoto` representa o único registro visual associado a um dia civil.

| Atributo | Tipo | Obrigatório | Regra |
| --- | --- | --- | --- |
| `id` | `UUID` | sim | identificador técnico, único e imutável |
| `dayKey` | `String` | sim | identidade canônica e única do dia no formato `yyyy-MM-dd` |
| `timeZoneIdentifier` | `String` | sim | fuso IANA usado ao atribuir a captura ao dia, por exemplo `America/Fortaleza` |
| `imagePath` | `String` | sim | caminho relativo para o arquivo no contêiner do app |
| `createdAt` | `Date` | sim | instante absoluto de criação do registro |
| `updatedAt` | `Date` | sim | instante absoluto da última alteração |

### Campo considerado e adiado

`caption: String?` foi considerado, mas legenda não faz parte do MVP. Para evitar persistir um campo sem comportamento de produto, ele deve ser adicionado somente quando essa funcionalidade entrar no escopo e sua regra estiver definida.

## Por que não usar apenas `date: Date`

Em Swift, `Date` representa um instante absoluto, não um dia civil. Duas capturas feitas no mesmo dia podem ter valores `Date` diferentes; normalizar para meia-noite também produz instantes diferentes quando o fuso horário muda.

Por isso, o modelo separa três conceitos:

- `dayKey`: identidade lógica usada para unicidade e ordenação diária;
- `timeZoneIdentifier`: contexto que atribuiu o instante ao dia;
- `createdAt` e `updatedAt`: instantes absolutos para auditoria local.

Uma propriedade calculada chamada `date`, se for conveniente para a interface, pode ser derivada de `dayKey` e `timeZoneIdentifier`; ela não deve ser uma segunda fonte de verdade persistida.

## Regra de unicidade

> Existe no máximo um `DailyPhoto` para cada dia civil.

Para o MVP, um dia civil é identificado assim:

1. usar o calendário gregoriano;
2. usar o fuso horário atual do dispositivo no momento em que o registro é criado;
3. extrair ano, mês e dia nesse contexto;
4. produzir `dayKey` com formato fixo `yyyy-MM-dd`, sem depender da localidade de exibição;
5. preservar essa chave e o `timeZoneIdentifier` mesmo se o usuário viajar ou alterar o fuso depois.

Exemplo: uma foto capturada em 15 de setembro de 2026 no fuso `America/Fortaleza` recebe `dayKey = "2026-09-15"`. Alterar o fuso posteriormente não move o registro para outro dia.

A interface pode formatar datas conforme a localidade do usuário. Formatação visual não deve ser usada como chave de persistência.

### Garantia em duas camadas

- A lógica de aplicação consulta `dayKey` antes de inserir e direciona o usuário para substituição quando já há registro.
- O modelo SwiftData aplica uma restrição de unicidade a `dayKey` como última linha de defesa.

Somente checar antes de inserir é insuficiente diante de operações concorrentes ou erros de fluxo.

## Comportamento em mudanças de fuso

O “hoje” é recalculado com o fuso atual ao entrar em primeiro plano, em mudanças relevantes do sistema e imediatamente antes de uma gravação. Registros existentes mantêm sua chave original.

Consequências intencionais:

- viajar não reclassifica fotos antigas;
- se o usuário reencontrar a mesma data civil ao cruzar fusos, a chave existente continua limitando essa data a uma foto;
- mudanças manuais de relógio ou fuso não devem criar duplicatas para a mesma `dayKey`;
- a data de captura é a atribuída pelo app na confirmação, não metadados EXIF potencialmente inconsistentes.

Casos de virada do dia e troca de fuso devem ter testes unitários dedicados.

## Restrições

- `id` é único e não muda em uma substituição de imagem;
- `dayKey` é único, obrigatório e não muda em uma substituição;
- `imagePath` é obrigatório, relativo, normalizado e não pode escapar do diretório gerenciado pelo app;
- `createdAt` não muda após a criação;
- `updatedAt` nunca é anterior a `createdAt` e é atualizado na substituição;
- um registro só deve ser considerado válido quando seu arquivo de imagem existir e puder ser lido;
- nomes físicos de arquivo devem ser opacos, preferencialmente derivados de um UUID, e não da legenda ou de dados pessoais.

## Relacionamentos

Não há relacionamentos no MVP. `DailyPhoto` é a única entidade necessária.

Configurações simples, como lembrete ativado e horário escolhido, podem usar armazenamento de preferências do sistema. Não justificam uma entidade SwiftData neste momento.

## Regras de integridade

### Criação

- gerar a `dayKey` uma única vez na confirmação;
- rejeitar ou converter em substituição qualquer conflito de unicidade;
- só persistir metadados apontando para um arquivo gravado com sucesso;
- remover o arquivo recém-criado se a persistência falhar.

### Substituição

- manter `id`, `dayKey`, `timeZoneIdentifier` e `createdAt`;
- gravar a nova imagem antes de alterar a referência;
- atualizar `imagePath` e `updatedAt`;
- remover a imagem anterior somente depois que a nova referência estiver salva.

### Exclusão

- remover entidade e arquivo como uma única operação coordenada;
- nunca apagar um arquivo fora do diretório gerenciado;
- se apenas uma das etapas falhar, preservar informação suficiente para tentar novamente ou reconciliar o estado com segurança.

### Arquivo ausente ou órfão

- referência sem arquivo deve produzir estado de erro recuperável, não crash;
- arquivo sem referência pode ser removido por uma rotina conservadora de reconciliação;
- a rotina deve excluir somente arquivos reconhecidos dentro do diretório `DailyPhotos` e nunca usar caminhos fornecidos externamente.

## Estratégia para imagens

- armazenar arquivos sob `Application Support/DailyPhotos` ou diretório equivalente do contêiner privado;
- persistir `imagePath` relativo à raiz controlada pelo `PhotoService`;
- não salvar imagens completas como `Data` no SwiftData;
- fazer escrita atômica sempre que a API escolhida permitir;
- carregar a imagem completa somente no detalhe;
- gerar miniaturas sob demanda e começar com cache simples; persistir miniaturas apenas se medições justificarem;
- não salvar automaticamente a captura na biblioteca Fotos do usuário;
- não enviar imagens pela rede no MVP.

Formato, compressão, dimensões máximas e política de backup do diretório precisam ser confirmados com protótipos antes da implementação definitiva.

## Consultas previstas

- obter registro por `dayKey`;
- obter registros de um intervalo mensal;
- listar registros por `dayKey` em ordem decrescente;
- verificar rapidamente se o dia atual já possui registro.

Não há necessidade inicial de busca textual, tags, álbuns ou geolocalização.

## Evolução do schema

O primeiro schema deve conter apenas os campos necessários acima. Campos futuros, como legenda, localização ou referência de sincronização, exigem decisão de produto e plano de migração antes de entrar no modelo.

## Referências técnicas

- [Restrições de unicidade no SwiftData](https://developer.apple.com/documentation/swiftdata/unique(_:))
- [Armazenamento externo de atributos no SwiftData](https://developer.apple.com/documentation/swiftdata/schema/attribute/option/externalstorage)
