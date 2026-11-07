# LIMPIDEZ — Smart Booking Residencial | SDD-LIMPIDEZ-SBR-001

**Status:** SPEC_READY · IMPLEMENTATION_PENDING · NOT_CERTIFIED
**Governança:** AES v3.2 + AEDS + AES-AID + SDD
**Baseline:** ZIP original Google AI Studio; alterações Smart Booking V01 mantidas separadas.

## Escopo imutável
Preservar Home, header, bottom navigation, quatro personas, rotas, assets e tokens de `src/index.css`. Sem alteração de identidade, pagamentos reais ou novas categorias nesta vertical. Não considerar frontend autoridade de preço, agenda, cobertura, autenticação ou tenant.

## Jornada
1. PROPERTY: tipo de imóvel, quartos, banheiros, área e acesso.
2. SCOPE: limpeza padrão/profunda, materiais, adicionais, pets, observações e fotos quando pertinentes.
3. AVAILABILITY: classificar DIRECT / QUOTE / RECURRING; validar cobertura, prestador e agenda.
4. REVIEW: escopo e condições transparentes; demo usa **Simular solicitação**.

Entradas HOME, SEARCH, CATEGORY, PROVIDER e REPEAT convergem no mesmo draft. Prestador pré-selecionado incompatível nunca é substituído silenciosamente.

## Regras condicionais rastreáveis
- R01 casa: área externa opcional; se selecionada, descrição obrigatória.
- R02 apartamento: não mostrar quintal; varanda é adicional independente.
- R03 limpeza profunda: explicar escopo, verificar complexidade; não garantir preço.
- R04 recorrência: exigir dias/períodos e data inicial; não criar série sem confirmação.
- R05 materiais do prestador: validar capacidade; custo apenas quando cotado.
- R06 pets: solicitar orientação pertinente, sem exclusão automática.
- R07 adicionais: validar IDs, compatibilidade, dependências e escopo.
- R08 área >120m² ou complexidade: avaliação, possível QUOTE.
- R09 fora da cobertura: explorar permitido, confirmar bloqueado.
- R10 prestador incompatível: informar e oferecer alternativa consentida.
- R11 slot expirado: invalidar agenda, manter escopo.
- R12 localização negada: entrada manual.
- R13 escopo alterado: invalidar preço/slot dependentes.
- R14 DEMO: nenhuma reserva, pagamento ou disponibilidade real.
- R15 draft recuperado: validar versão e migrar ou pedir revisão.
- R16 repetir pedido: pré-preencher somente dados consentidos e revalidar oferta.

## Validações
Imóvel enum obrigatório; quartos inteiros 0–20; banheiros inteiros 1–20; faixa de área e tipo de limpeza válidos; frequência e origem de materiais obrigatórias; adicionais únicos e presentes no catálogo; observações até 500 caracteres; cidade/UF para descoberta e endereço validado para reserva; fotos com tipo/tamanho/acesso verificados no backend; slot da mesma oferta, não expirado. Mostrar erros inline e acessíveis; backend repete todas as validações.

## Estados
`PROPERTY → SCOPE → AVAILABILITY → REVIEW → DEMO_COMPLETE`; em produção `REVIEW → SUBMITTED` somente após confirmação autoritativa. Voltar/editar preserva escolhas compatíveis. Expiração e falha de integração não podem produzir pedido confirmado. SUBMITTED não equivale a pagamento capturado ou serviço concluído.

## Critérios de aceite
AC01 entradas convergentes; AC02 casa/apartamento condicional; AC03 payload inválido bloqueado; AC04 draft restaurado; AC05 complexidade pode exigir orçamento; AC06 cobertura bloqueia confirmação; AC07 prestador incompatível sem troca silenciosa; AC08 slot concorrente rejeitado; AC09 preço sem confirmação não é definitivo; AC10 isolamento tenant; AC11 idempotência; AC12 teclado/leitor de tela; AC13 360/390/430/1024 sem overflow e tokens preservados; AC14 regressão Home/Buscar/Pedidos/Mais/login; AC15 demo não produz reserva/pagamento real.

## PRs
PR-01 tipos, regras puras e testes; PR-02 UX progressiva e draft; PR-03 matching demo determinístico; PR-04 agenda e revisão; PR-05 integração backend posterior. Cada PR exige diff delimitado, testes, screenshots e revisão independente. Build/lint verde não implica REAL_E2E.

## Pendências
Preços e política comercial, catálogo de adicionais, cobertura, regras de recorrência e integrações reais exigem aprovação antes da produção. Documento técnico detalhado em Markdown disponível como artefato da conversa; este arquivo é o resumo operacional versionado.
