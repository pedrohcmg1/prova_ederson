# Tasks

Tarefas em ordem de dependência. Cada tarefa deve atender aos critérios de conclusão.

| # | Tarefa | Depende de | Pronto quando |
|---|---|---|---|
| 1 | **Setup:** `requirements.txt`, `app/config.py` com as 5 constantes e `app/clock.py` com `agora()` em `-03:00` | — | `uvicorn` sobe na porta `8005` |
| 2 | **Erros globais:** padronizar `{"erro": ...}`, usar `422` em validações e `404` para recursos inexistentes | 1 | Rotas inexistentes e entradas inválidas seguem o formato definido |
| 3 | **Pricing:** criar `app/pricing.py` com `calcular(segundos)` usando apenas inteiros, tolerância, frações e teto | 1 | Todos os casos de cálculo de `tests.md` passam |
| 4 | **Store:** criar `app/store.py` com armazenamento em memória, IDs sequenciais e índice de placas abertas | 1 | Permite criar, buscar e listar bilhetes por placa/status |
| 5 | **UC1 + UC8:** implementar abertura, validação de placa/entrada e bloqueio de placa ocupada | 2, 4 | Casos de abertura e conflito de `tests.md` passam |
| 6 | **UC2 + UC5:** implementar encerramento, cálculo do valor e cancelamento | 3, 5 | Valores, `404`, `409` e liberação da placa estão corretos |
| 7 | **UC3 + UC6:** implementar ativos e histórico por placa, ambos em `id` decrescente | 5 | `/bilhetes/ativos` não colide com `/bilhetes/{id}` |
| 8 | **UC4:** implementar relatório diário e média arredondada com `0,5` para cima | 6 | Todos os casos de relatório de `tests.md` passam |
| 9 | **Testes + README:** cobrir `tests.md` e documentar execução | 5–8 | Suíte automatizada verde e comando de execução documentado |
