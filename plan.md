# Plan — Zona Azul Digital

## 1. Estrutura da aplicação

- Identificar a estrutura existente do projeto.
- Manter backend, frontend e banco conforme o esqueleto fornecido.
- Implementar a API REST seguindo `constitution.md` e `spec.md`.

## 2. Modelo de dados

Criar o modelo de `Bilhete` contendo, no mínimo:

- `id`
- `placa`
- `entrada`
- `saida`
- `status`
- `minutos`
- `valor_centavos`

Status possíveis:

- `aberto`
- `encerrado`
- `cancelado`

Garantir que o `id` seja sequencial.

## 3. Regras de negócio

Implementar:

1. Validação de placa.
2. Validação de entrada ISO-8601 com fuso.
3. Controle de uma vaga por placa.
4. Abertura de bilhete.
5. Encerramento e cálculo do valor.
6. Cancelamento.
7. Listagem de bilhetes ativos.
8. Histórico por placa.
9. Relatório diário.
10. Conversão de datas para o fuso `-03:00`.

Centralizar o cálculo de duração e valor para evitar regras duplicadas.

## 4. Endpoints

Implementar:

- `POST /bilhetes`
- `POST /bilhetes/{id}/encerramento`
- `GET /bilhetes/ativos`
- `GET /relatorios/diario`
- `POST /bilhetes/{id}/cancelamento`
- `GET /bilhetes?placa=...`

Garantir que `/bilhetes/ativos` seja resolvido antes de `/bilhetes/{id}`.

## 5. Validações e erros

Implementar os códigos HTTP e corpos de erro definidos na `spec.md`.

No UC1, respeitar a ordem:

```text
placa → entrada → conflito de placa
