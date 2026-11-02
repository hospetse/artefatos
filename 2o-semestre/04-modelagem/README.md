# 4. Modelagem

Modelos do sistema, do mais geral para o mais detalhado:

| # | Modelo | Pasta | O que mostra |
|---|---|---|---|
| 1 | Diagramas de Casos de Uso | [casos-de-uso](casos-de-uso) | Quem usa o sistema e o que cada ator faz. Visão completa e 16 diagramas por módulo |
| 2 | Diagrama de Classes | [classes](classes) | Entidades do domínio e como se relacionam. Visão completa, Auth Server e 5 seções |
| 3 | DER | [der](der) | Tabelas dos dois bancos PostgreSQL (Auth Server e back-end) |
| 4 | Diagramas de Sequência | [sequencia](sequencia) | A troca de mensagens entre App, Auth Server, back-end e serviços externos, em 8 tipos de fluxo |

Os diagramas são feitos em PlantUML. O `.svg` é a imagem, e o `.puml` com o mesmo nome é a fonte.
