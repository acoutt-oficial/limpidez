# LIMPIDEZ — SDD COM-001 / COST-001 / MAP-001
Status: SPEC_READY; INTEGRATION_PENDING; NOT_PRODUCTION_READY.
Governança: AES v3.2, AEDS, AES-AID, SDD. Preservar React, quatro personas, logo e tokens originais.

## COST-001: precificação e custo operacional
Separar: (a) preço bruto cliente, (b) extras e deslocamento, (c) descontos, (d) taxa da plataforma, (e) repasse ao profissional/empresa, (f) gateway, (g) impostos e tributos estimados, (h) comunicação WhatsApp, (i) mapas/geocoding, (j) infraestrutura e suporte, (k) margem de contribuição. **Nunca** calcular preço vinculante apenas no frontend. Preço final só após cotação autoritativa, disponibilidade e validade temporal.
Registros: PricingPolicy(id, version, effectiveAt, tenantId?, serviceCategory, rules), Quote(id, bookingDraftId, currency, lineItems, subtotal, discount, total, expiresAt, status), CostLedger(id, bookingId?, category, provider, units, unitCost, currency, actualOrEstimated, occurredAt), Payout(id, bookingId, beneficiaryId, gross, fees, net, status).
Admin: dashboard custos fixos/variáveis, gasto por canal, receita, take rate, margem, alertas de orçamento e limites por tenant. Nenhum valor padrão comercial deve ser tratado como preço aprovado.

## COM-001: chat transacional interno
Conversas vinculadas a bookingId/quoteId, participantes autorizados CLIENTE e PROFISSIONAL/EMPRESA; equipe de suporte com acesso auditado, não acesso irrestrito. Mensagens texto e anexos controlados, paginação, estados PENDING/SENT/DELIVERED/READ/FAILED, idempotencyKey, rate limiting, anti-spam, bloqueio e denúncia. Notificações não contêm endereço exato ou conteúdo sensível em push. Não permitir mensagens entre tenants ou acesso por IDs previsíveis.
Modelo: Conversation(id, bookingId, tenantId, status), Participant(conversationId, actorId, role), Message(id, conversationId, senderId, body, attachmentIds, status, createdAt), MessageReceipt(messageId, actorId, deliveredAt?, readAt?), ModerationEvent(messageId?, actorId, type, outcome).
API: POST /api/conversations; GET /api/conversations/:id/messages?cursor=; POST /api/conversations/:id/messages; POST /api/conversations/:id/read. Auth, ownership e tenant verificados no backend. Realtime por canal autorizado, fallback polling.

## WA-001: WhatsApp Business Platform (opcional)
Usar API oficial da Meta Cloud API ou BSP aprovado; não automatizar WhatsApp Web. Primeiro estágio: lembretes e atualização de agendamento mediante opt-in específico, com templates aprovados e categoria correta. Respostas entram em caixa de atendimento quando política e janela permitirem; não afirmar sincronização bidirecional sem webhook funcional. Backend guarda credenciais em secrets, verifica assinatura dos webhooks, deduplica event IDs, trata retries, opt-out, retenção e exclusão LGPD. Telefone não deve ser exposto entre participantes por padrão.
Modelo: CommunicationPreference(actorId, channel, purpose, consentAt, revokedAt), OutboundNotification(id, bookingId, channel, templateId, status, providerMessageId, idempotencyKey), ProviderWebhookEvent(id, signatureVerified, processedAt, eventType).
API: POST /api/notifications/booking-events (interno), POST /api/webhooks/whatsapp (assinatura), GET /api/admin/communications/costs. Respeitar políticas Meta e aprovação de template; fallback chat interno/e-mail onde disponível. Custos por categoria, país e provedor configuráveis e versionados, nunca constantes hardcoded.

## MAP-001 e LOC-001
Mapa de cobertura e prestadores por região, sem divulgar residência exata. Cliente pode alterar endereço, invalidando cotação, matching e slot; endereço preciso apenas após necessidade operacional. Geocoding, tiles, rotas e IP geo com adaptadores substituíveis e quotas. Dados GPS apenas com permissão explícita. No MVP, mapa e chat não dependem de rastreamento em tempo real.

## Interface responsiva
Desktop: header com localização, notificações, avatar/logout; sidebar por papel; Smart Booking com configurador e resumo lateral, chat em painel contextual sem bloquear formulário. Mobile: bottom nav original, chat por tela dedicada e ações acessíveis. Tokens oficiais em src/index.css; não recriar logo.

## Critérios de aceite
C01 preço final calculado e assinado no backend; C02 troca de local invalida preço e slot; C03 cálculo contábil reconcilia totais e repasse; C04 custo API registrado sem duplicação; C05 quota evita gasto excessivo; C06 conversa exige participante autorizado; C07 cross-tenant negado; C08 webhook assinatura inválida rejeitado; C09 opt-out impede envio; C10 mensagens duplicadas idempotentes; C11 template fora da política não enviado; C12 sem vazamento de endereço/telefone; C13 chat responsivo e acessível; C14 falha WhatsApp não impede reserva; C15 simulação identificada; C16 testes mobile/desktop; C17 evidências CI e revisão humana independente.

## Entrega incremental
PR05: ledger de custos + motor de cálculo puro/testes; PR06: chat interno + backend seguro; PR07: notificações WhatsApp oficial e webhooks; PR08: mapas/locais com limites; PR09: testes E2E, custo, segurança e acessibilidade. Nenhum desses PRs equivale a integração já realizada.
