# 7. Gestão

O planejamento e o acompanhamento do trabalho ficam no [Azure DevOps](https://dev.azure.com/escola-ti/Hospetse) (acesso por convite).

| Artefato | Arquivo | Wiki |
|---|---|---|
| Fluxo de trabalho e cerimônias | [fluxo-de-trabalho-e-cerimonias.md](fluxo-de-trabalho-e-cerimonias.md) | [Wiki #43](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/43) |
| Definition of Done e critérios de aceite | [definition-of-done.md](definition-of-done.md) | [Wiki #34](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/34) |
| Epic HSP-EP-001 — Identidade, conta e acesso | [epic-hsp-ep-001.md](epic-hsp-ep-001.md) | [Wiki #69](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/69) |
| Compromissos da Sprint 7 | [compromissos-sprint-7.md](compromissos-sprint-7.md) | [Wiki #86](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/86) |
| Estimativa inicial em horas por PBI | [estimativa-horas-pbis.md](estimativa-horas-pbis.md) e [XLSX](estimativa-pbi-hospetse.xlsx) | [Wiki #80](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/80) |
| Visão macro do backlog: Epics, Features e PBIs, com uma aba por Feature | [XLSX](visao-macro.xlsx) | [Wiki #39](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/39) |
| Horas de trabalho do time (Jibble, 31/07 a 02/11/2026) | [XLSX](horas-hospetse-2026.xlsx) | [Wiki #39](https://dev.azure.com/escola-ti/Hospetse/_wiki/wikis/Hospetse.wiki/39) |
| Backlog completo e sprints | [Azure Boards](https://dev.azure.com/escola-ti/Hospetse/_boards) | — |

## Como o trabalho é organizado

O time usa Scrum com sprints de 2 semanas e o processo Scrum do Azure Boards, em quatro níveis:

```text
Epic  →  Feature  →  PBI (Product Backlog Item) ou Bug  →  Task
```

| Nível | Para que serve |
|---|---|
| Epic | Um grande objetivo do produto, que atravessa várias sprints |
| Feature | Uma capacidade do produto dentro de um Epic |
| PBI | Uma entrega que o usuário percebe, escrita como história com critérios de aceite. Defeitos entram como Bug |
| Task | O trabalho técnico para entregar o PBI, atribuído a uma pessoa |

Branches seguem o padrão `feature/<número>-descrição`, e commits e Pull Requests citam o work item com `AB#<número>` (ADR-014). Assim, toda mudança no código é rastreável até o PBI e o Epic de origem.
