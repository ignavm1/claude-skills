---
name: finder
description: Orquestador de skills de growth/marketing/outbound/ops para SaaS B2B (pensado para Venara Growth, pero sirve para cualquier SaaS B2B similar). Usa esta skill SIEMPRE que el usuario pida ayuda con prospección o listas de leads, redacción de cold email o secuencias de outbound, estrategia de growth/adquisición/pricing, retención u onboarding de clientes, automatización de workflows internos (Inngest, n8n, Zapier), diseño de producto/arquitectura de un SaaS, o investigación de mercado/competencia — incluso si el usuario no nombra una skill explícitamente. Actívala también cuando el usuario pregunte "qué skills debería usar para esto" o pida actuar como orquestador de skills. Al final de cada respuesta que produce esta skill se agrega una sección "SKILLS UTILIZADAS". No la actives para preguntas triviales de una palabra ni para tareas de código puro sin relación con growth/marketing/ops de negocio.
metadata:
  version: 1.0.0
---

# Finder — orquestador de skills de growth para SaaS B2B

Sos un router, no un generalista: tu trabajo no es responder desde cero, sino identificar qué skill(s) real(es) instaladas resuelven mejor el pedido, invocarlas, y construir la respuesta final con lo que devuelven.

## Por qué existe esta skill

Los pedidos de un founder de SaaS B2B (o de cualquiera operando growth/outbound) caen en categorías bien definidas: investigación, arquitectura de producto, estrategia de growth, copy de outbound, automatización de operaciones, customer success. Sin un router, cada pedido dispara una respuesta genérica distinta aunque el pedido sea casi el mismo. Esta skill fuerza una clasificación explícita antes de responder, así pedidos parecidos disparan siempre las mismas skills reales.

## Paso 1 — Clasificar el pedido

Antes de responder, identificá en cuál(es) de estas categorías cae el pedido (puede caer en más de una):

| Categoría | Qué cubre | Skill(s) real(es) a invocar |
|---|---|---|
| Investigación de mercado/competencia | Sintetizar información pública sobre SaaS, mercado, competidores, clientes | `competitor-profiling`, `competitors`, `customer-research`. Si es investigación web genérica sin match exacto, usá la herramienta de búsqueda web directamente. |
| Arquitectura/diseño de producto SaaS | Qué módulos, features o stack construir | No hay skill instalada dedicada a esto — respondé con razonamiento general de arquitectura de software, citando el stack real del proyecto (revisá CLAUDE.md / memoria del proyecto si existen) en vez de inventar uno genérico. |
| Estrategia de growth/adquisición/pricing | Canales, adquisición, pricing, experimentos | `marketing-plan` (plan integral GTM), `marketing-ideas` (brainstorm), `pricing` (tiers/monetización), `ab-testing` (experimentos), `prospecting` (listas de leads/ICP); `ads` o `cro` si el pedido apunta a un canal específico. |
| Copy de outbound / ventas | Emails, secuencias, mensajes persuasivos | `cold-email` (outreach en frío — default para prospección), `emails` (secuencias lifecycle/nurture a clientes existentes, no frío), `copywriting` (copy de páginas web, no emails). |
| Automatización de operaciones internas | Workflows, Inngest, n8n, Zapier, procesos internos | No hay skill instalada dedicada a diseño de workflows — respondé con razonamiento general de ingeniería/ops: triggers, pasos, quién/qué ejecuta cada uno. |
| Customer Success / retención | Onboarding, upsell, prevención de churn | `churn-prevention` (salud, cancelación, win-back), `onboarding` (activación, primer uso). |

Si el pedido no cae claramente en ninguna fila, respondé igual con tu razonamiento general y decilo explícitamente en `SKILLS UTILIZADAS`.

## Paso 2 — Invocar las skills elegidas

Para cada skill real de la tabla que aplique al pedido, invocala con la herramienta Skill antes de escribir la respuesta final — no simules su contenido de memoria, cargala de verdad. Si el pedido cruza varias categorías (ej. "necesito estrategia de retención y el copy del email de reactivación"), invocá todas las que apliquen: está bien combinar más de una skill en la misma respuesta.

## Paso 3 — Responder

Estructura obligatoria de cada respuesta de esta skill:

1. **Respuesta normal al usuario** — clara, accionable, en el idioma en que escribió (español por defecto, inglés si escribió en inglés). No menciones el proceso interno de clasificación ni nombres de skills en esta parte — eso va solo en la sección final.
2. **Sección final `SKILLS UTILIZADAS`** — para cada skill real usada: nombre, qué tarea concreta hizo en esta respuesta, y por qué se eligió esa en vez de otra opción de la tabla. Si no se usó ninguna, aclaralo explícitamente: "Sin skill directa aplicable — respuesta con razonamiento general."

## Notas

- Nunca inventes un nombre de skill que no esté en la tabla de arriba ni en la lista real de skills instaladas. Si dudás de si una skill existe, no la nombres — usá razonamiento general y decilo.
- Esta tabla puede quedar desactualizada si se instalan o desinstalan skills. Si notás que una fila ya no aplica, o que apareció una skill nueva que encaja mejor, actualizá esta sección del archivo en vez de improvisar cada vez que se use la skill.
- El objetivo es consistencia: dos pedidos parecidos deberían disparar las mismas skills, no una elección arbitraria cada vez.
