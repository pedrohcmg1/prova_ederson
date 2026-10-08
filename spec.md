# Spec — Zona Azul Digital

API REST (JSON) de bilhetes de estacionamento rotativo. Seguir as convenções de `constitution.md`.

## Regra central de cálculo

- `segundos = max(0, saida - entrada)`
- `minutos = ceil(segundos / 60)`
- Se `minutos <= 15`: `valor_centavos = 0`
- Caso contrário:
  - `fracoes = ceil(minutos / 30)`
  - `valor_centavos = min(7000, fracoes * 300)`

> A tolerância não é descontada da duração.  
> 15 min = 0; 16 min = 300.  
> `minutos` representa a duração real, não os minutos cobrados.

## Erros

| Situação | HTTP | Body |
|---|---:|---|
| Placa inválida | 422 | `{"erro":"placa_invalida"}` |
| Entrada inválida | 422 | `{"erro":"entrada_invalida"}` |
| Data inválida | 422 | `{"erro":"data_invalida"}` |
| Bilhete inexistente | 404 | `{"erro":"bilhete_nao_encontrado"}` |
| Bilhete já encerrado/cancelado ao encerrar | 409 | `{"erro":"bilhete_ja_encerrado"}` |
| Bilhete não aberto ao cancelar | 409 | `{"erro":"bilhete_nao_aberto"}` |
| Placa já ocupada | 409 | `{"erro":"bilhete_em_aberto"}` |

### Placa

Regex: `^[A-Z0-9]{7}$`.

Minúscula, tamanho diferente de 7, símbolos, ausência ou valor não-string → `placa_invalida`.

---

## UC1 — Abrir bilhete

`POST /bilhetes`

Body:
```json
{"placa":"ABC1D23"}
