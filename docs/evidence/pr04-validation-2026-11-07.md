# PR-04 — Relatório de validação reproduzível (2026-11-07)

**Status:** ENGINE_ONLY / ENVIRONMENT_BLOCKED (build) / NOT_CERTIFIED

## Baseline examinada
Pacote local `LIMPIDEZ-Smart-Booking-PR04-LOC001-Agenda-Demo.zip`, projeto React/Vite; `package.json` com scripts `build: vite build` e `lint: tsc --noEmit`; `bun.lock` presente, `package-lock.json` ausente.

## Evidências executadas
- `node --experimental-strip-types --test tests/*.test.mjs` — **22/22 PASS**, 0 falhas, testes de regras residenciais e rascunho.
- `npm ci --offline` — **BLOCKED**: não existe `package-lock.json` ou shrinkwrap.
- `npm install --offline` — **BLOCKED**: pacote `@google/genai` não disponível no cache.
- Build `vite build`, TypeScript `tsc --noEmit`, E2E e inspeção visual — **NOT_RUN**.

## LOC-001
A implementação local declara localização manual editável, GPS sob ação explícita e invalidação de prestador/agenda na troca de endereço. **Não** há geolocalização aproximada por IP, geocodificação reversa ou validação de cobertura real. Sem provas de fluxo completo, não elevar a REAL_E2E.

## Gate mínimo seguinte
1. Reproduzir instalação com gerenciador compatível com `bun.lock` em ambiente com acesso ao registro e lockfile íntegro.
2. Executar `tsc --noEmit`, `vite build` e testes de regressão.
3. Validar em viewport 360/390/430, incluindo mudança de local com slot selecionado e GPS negado.
4. Revisar alterações por diff contra ZIP original, manter tokens e marca.
5. Publicar arquivos do PR-04 em branch própria somente após revisão e evidências; este commit **não** contém código React do PR-04.

## Segurança e veracidade
Nenhum preço, slot ou prestador demonstrativo deve ser apresentado como disponibilidade confirmada; nenhuma reserva ou cobrança real pode ser executada neste estágio.
