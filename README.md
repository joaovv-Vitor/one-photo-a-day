# One Photo a Day

Aplicativo iOS nativo para registrar uma única foto por dia e construir, com o tempo, uma linha do tempo visual pessoal.

> **Status:** em planejamento. O repositório contém, neste momento, apenas a documentação inicial; o projeto Xcode e o aplicativo ainda não foram criados.

## Problema

Fotos importantes se misturam a capturas de tela, downloads e centenas de imagens ocasionais. Ao mesmo tempo, aplicativos de diário frequentemente exigem esforço demais para manter o hábito.

## Proposta

O One Photo a Day reduz o diário visual ao menor fluxo útil:

**abrir o app → ver o dia atual → tirar uma foto → confirmar → concluir o registro**

Cada dia civil pode ter no máximo uma fotografia principal. A experiência será privada, local-first, offline e integrada ao ecossistema Apple.

## Funcionalidades planejadas para o MVP

- captura da foto do dia pela câmera;
- visualização, substituição e exclusão do registro;
- calendário mensal com indicação dos dias registrados;
- consulta aos registros anteriores;
- timeline cronológica;
- lembrete diário local e configurável;
- funcionamento sem conexão com a internet.

O MVP não terá backend, autenticação, conta, sincronização entre dispositivos, inteligência artificial ou funcionalidades sociais.

## Stack planejada

- Swift;
- SwiftUI;
- SwiftData;
- APIs nativas da Apple para câmera e fotografias;
- UserNotifications;
- Xcode.

Não há dependências de terceiros previstas para o MVP.

## Arquitetura resumida

As telas em SwiftUI delegarão estado e regras de aplicação a ViewModels ou tipos equivalentes. O SwiftData armazenará apenas os metadados dos registros. As imagens ficarão como arquivos no armazenamento privado do app, referenciadas por caminho relativo. Serviços pequenos isolarão câmera/arquivos e notificações locais.

Essa é uma direção arquitetural planejada, não uma implementação existente. Detalhes e limites estão em [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Documentação

| Documento | Conteúdo |
| --- | --- |
| [PRODUCT.md](docs/PRODUCT.md) | visão, público, princípios e limites do produto |
| [MVP.md](docs/MVP.md) | escopo obrigatório e critérios de conclusão |
| [REQUIREMENTS.md](docs/REQUIREMENTS.md) | requisitos funcionais e não funcionais verificáveis |
| [ARCHITECTURE.md](docs/ARCHITECTURE.md) | arquitetura inicial, responsabilidades e decisões técnicas |
| [DATA_MODEL.md](docs/DATA_MODEL.md) | modelo `DailyPhoto`, integridade por dia civil e arquivos de imagem |
| [ROADMAP.md](docs/ROADMAP.md) | evolução incremental até o MVP e possibilidades futuras |

## Desenvolvimento

Ainda não existem `.xcodeproj`, `.xcworkspace`, `Package.swift`, código-fonte ou suíte de testes no repositório. Por isso, não há comandos de build ou teste confirmados nesta fase.

Quando o projeto for iniciado, será necessário:

- macOS com uma versão do Xcode compatível com SwiftUI e SwiftData;
- um simulador para os fluxos gerais;
- um iPhone físico para validar a captura real pela câmera.

O deployment target ainda precisa ser definido. Como SwiftData faz parte da stack planejada, ele deverá ser compatível com iOS 17 ou posterior, salvo revisão dessa decisão durante a criação do projeto.

## Estado do escopo

- **Existente:** documentação de produto e planejamento técnico.
- **Planejado para o MVP:** captura diária, calendário, timeline, lembretes e persistência local.
- **Possível no futuro:** recursos descritos no roadmap, sem compromisso de implementação.
