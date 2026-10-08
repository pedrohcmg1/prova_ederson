Constitution — Zona Azul Digital

Regras persistentes. Valem para todo o CÓDIGO GERADO.

Parâmetros da variante
| Parâmetro            | Valor |
| -------------------- | ----- |
| TARIFA_HORA_CENTAVOS | 600   |
| FRACAO_MINUTOS       | 30    |
| TETO_DIARIO_CENTAVOS | 7000  |
| TOLERANCIA_MINUTOS   | 15    |
| PORTA_SERVICO        | 8005  |
Devem existir como constantes nomeadas em um único módulo de configuração. Nenhum número mágico espalhado.

1 - REGRAS GERAIS
- O serviço escuta em 0.0.0.0:8005. Base URL: http://localhost:8005.
- Valores monetários são sempre inteiros em centavos. Proibido float em qualquer cálculo ou resposta JSON.
- Todo timestamp em resposta é ISO-8601 com fuso -03:00 (ex.: 2026-10-05T14:30:00-03:00).
- Todo erro tem o formato {"erro": "<codigo>"} e o status HTTP da tabela de erros em spec.md. Nunca devolver stack trace, HTML ou campo detail.
- O contrato do enunciado vale sobre qualquer exemplo. O exemplo {"id": 7, "valor": 12.50} é inválido: usa float e a chave errada.
- Nenhum endpoint, campo ou código de erro além dos listados no contrato.
- Fonte única de tempo: toda leitura de "agora" passa por uma função agora().
- O serviço sobe sem etapas manuais (sem migração, sem seed) e com estado vazio.
- IDs são inteiros sequenciais a partir de 1.
