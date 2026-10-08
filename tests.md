# Tests — Casos de borda

Parâmetros: tarifa `600`, fração `30`, teto `7000`, tolerância `15`.
Valor por fração: `300` centavos.

Nos testes de valor, o bilhete é aberto com `entrada` no passado e encerrado em seguida.

## Valor por duração — UC2 + UC7

| Duração | Minutos | Frações | Valor |
|---|---:|---:|---:|
| 0 min | 0 | 0 | 0 |
| 10 min | 10 | 0 | 0 |
| 15 min | 15 | 0 | 0 |
| 15 min 01 s | 16 | 1 | 300 |
| 16 min | 16 | 1 | 300 |
| 30 min | 30 | 1 | 300 |
| 31 min | 31 | 2 | 600 |
| 60 min | 60 | 2 | 600 |
| 61 min | 61 | 3 | 900 |
| 95 min | 95 | 4 | 1200 |
| 690 min | 690 | 23 | 6900 |
| 691 min | 691 | 24 | 7000 |
| 1440 min | 1440 | 48 | 7000 |

> `691` minutos já ultrapassa o teto: o resultado deve ser `7000`, nunca `7200`.

## Relatório — UC4

| Encerrados no dia | Total | Faturamento | Média |
|---|---:|---:|---:|
| 10, 16 | 2 | 300 | 13 |
| 30, 31 | 2 | 900 | 31 |
| 30, 30, 31 | 3 | 1200 | 30 |
| Nenhum | 0 | 0 | 0 |
| 1 encerrado + 1 aberto + 1 cancelado | 1 | do encerrado | do encerrado |

A média deve usar os `minutos` reais e arredondar `0,5` para cima.

## Validação e conflitos

| Caso | Esperado |
|---|---|
| Placa ausente | `422 placa_invalida` |
| Placa minúscula | `422 placa_invalida` |
| Placa com tamanho diferente de 7 | `422 placa_invalida` |
| Placa com símbolos | `422 placa_invalida` |
| Entrada `"ontem"` | `422 entrada_invalida` |
| Entrada sem fuso | `422 entrada_invalida` |
| Entrada ISO-8601 com fuso | `201` |
| Placa já aberta | `409 bilhete_em_aberto` |
| Reabrir após encerrar | `201` |
| Reabrir após cancelar | `201` |
| ID inexistente ao encerrar | `404 bilhete_nao_encontrado` |
| ID inexistente ao cancelar | `404 bilhete_nao_encontrado` |
| Encerrar duas vezes | `409 bilhete_ja_encerrado` |
| Encerrar cancelado | `409 bilhete_ja_encerrado` |
| Cancelar encerrado | `409 bilhete_nao_aberto` |
| Cancelar duas vezes | `409 bilhete_nao_aberto` |
| Data inválida | `422 data_invalida` |
| Data ausente | `422 data_invalida` |

## Listagens

| Caso | Esperado |
|---|---|
| Nenhum ativo | `200 []` |
| 3 abertos + 1 encerrado | 3 ativos, `id` decrescente |
| Placa sem histórico | `200 []` |
| Encerrado + cancelado + aberto | 3 itens, `id` decrescente e campos conforme status |
| Cancelamento | Não contém `saida` nem `valor_centavos` |

## Invariantes

- Nenhuma resposta contém números decimais.
- `entrada` e `saida` terminam em `-03:00`.
- Serviço disponível em `http://localhost:8005`.
- `/bilhetes/ativos` não pode ser interpretado como `/bilhetes/{id}`.
