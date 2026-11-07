# LIMPIDEZ — Smart Booking V01 (escopo e gate)

## Origem
Baseline local recebida: `limpidez-4-de-set-de-2026-16-14-12.zip`.
Pacote de trabalho local: `limpidez-smart-booking-v01.zip` (ainda não importado neste branch).

## Objetivo
Preservar Home, identidade oficial, navegação mobile e quatro personas. Implementar jornada residencial com configuração contextual e dados mock tipados antes de Supabase/pagamento real.

## Regras
- Não autorizar por query string nem por flags do frontend.
- Modo demo claramente separado de produção.
- Não copiar identidade visual de concorrentes.
- Cada categoria tem configurador próprio.
- Dados selecionados devem persistir durante voltar/avançar.
- Matching, agenda, preço e checkout devem ser consistentes e não simular disponibilidade real como confirmada.
- Segurança, testes e auditoria de licenças OSS são gates de release.

## Critérios de aceite
- Fluxo residencial completo com validação e retorno sem perda de estado.
- Visual e interações em 360, 390, 430 px e desktop.
- Sem regressão na Home ou navegação.
- Evidência de build, testes e revisão humana.

## Estado
**FOUNDATION_ONLY / IMPORT PENDENTE**: este documento não representa importação do código nem aprovação de build. O ZIP local deve ser sincronizado e comparado com a versão mais recente do Google AI Studio antes de abrir PR funcional.
