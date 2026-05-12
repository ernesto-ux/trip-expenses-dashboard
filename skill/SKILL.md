# Trip Expenses

Track, split, and evaluate shared trip expenses. Generates an interactive HTML breakdown (filters, daily totals, per-person modals, category proportions) and applies a structured framework to evaluate whether the trip was well-managed financially.

## Invocation

```
/trip-expenses
```

Or trigger when user mentions: "viaje gastos", "splitear gastos viaje", "cuadro de gastos", "evaluar viaje", "trip split", "trip expenses".

## Context

Built from Amsterdam abril 2026 trip with Adriana. Encodes the workflow + lessons learned. Sandbox project at `~/Desktop/Sandbox/trip-expenses/`. Template at `~/Desktop/Sandbox/trip-expenses/src/template.html`.

## What it does

### Mode 1: Generate trip expense breakdown

Inputs from user:
- List of expenses (who paid, concept, amount, category, day)
- Items already paid before trip (flights, anchor event tickets) with split details
- Personal items (no split): shopping, individual transport
- Notes: items marked "no split" (gifts), special split ratios

Process:
1. Categorize each expense (see categories below)
2. Compute split: per-person share, balance, who-owes-whom
3. Compute daily totals (excluding hotel and other one-shot lump sums if user prefers)
4. Compute proportions of complete trip (including pre-paid flights/event)
5. Generate per-person breakdown including pre-paid items + personal expenses

Output:
- Save HTML to `~/Desktop/Sandbox/trip-expenses/src/[destination]-[YYYY]-[MM].html`
- Open in browser
- Update `CHANGELOG.md` in the sandbox project

### Mode 2: Evaluate trip spending

When user asks "cómo evalúas el gasto" or similar, apply the framework in `~/Desktop/Sandbox/trip-expenses/docs/evaluation-principles.md`.

**CRITICAL: Apply people-scaled lens.** When evaluating any per-decision cost for couples/groups, multiply by people count *before* judging. Don't use single-person heuristics.

Example anti-pattern (do NOT repeat):
- "Bolt CDG €40 was wasteful, RER B is €11.80"
- WRONG: for 2 people, Bolt = €20.05/p, RER = €23.60 total → Bolt is cheaper

Verdict template (see docs):
- Layer 1: anchor costs (hotel, flights, event) — lock-in, evaluate vs market
- Layer 2: operational (food, transport) — efficiency metrics
- Layer 3: personal/discretionary (shopping, upgrades) — separate from trip evaluation

## HTML structure

The template includes:

### Header
- Title + dates
- Two pill buttons (E and A) on the right opening per-person modals

### Summary stats (5 cards)
1. Total Viaje (everything included, highlighted)
2. Para split (50/50) — sub: "Sin vuelos ni concierto"
3. Pagó [Person 1] + transaction count
4. Pagó [Person 2] + transaction count
5. Por persona (split / 2)

### Saldo pendiente box (yellow)
- Big amount + "X debe a Y"

### Proporciones del viaje completo (grid of cards with bars)
- Concierto, Hotel, Vuelos, Alimentación, Transporte total, Souvenirs, etc.
- Each card: name, €amount, %, bar fill

### Detalle por categoría (table)
- All categories with €monto + % of total

### Gasto por día (4 day-cards)
- Click filters expense table to that day
- Excludes hotel from daily totals (shows separately as note)
- Description per day (e.g., "tulipanes y ciudad")

### Detalle de gastos comunes con filtros
- 3 dropdown filters: Categoría / Día / Pagó (Ernesto/Adriana)
- Live filter summary: "Total filtrado: €X · N gastos"
- Table with: Fecha, Pagó (badge), Categoría (chip), Concepto, Detalle, Monto
- Each row has `data-day`, `data-cat`, `data-who`, `data-amount` attrs

### Cálculo del saldo (table with breakdown)

### Transporte personal (Revolut, no split) — table

### Compras personales (no split, control propio) — table
- Categories: 👕 Ropa, 🧥 Ropa, 🛍️ Souvenirs propios, 🎁 Regalos, 🏠 Casa, etc.

### Modal E and Modal A
- Click pill button → modal with per-person breakdown
- Includes: split share + personal transport + personal shopping + concert share + flight
- Categories with %

## Categories standard

| Emoji | Category data attr | Notes |
|-------|--------------------|-------|
| 🏨 | `Hotel` | One-shot, exclude from daily totals |
| ✈️ | `Vuelos` | Pre-paid, separate |
| 🎵 | `Concierto` | May have non-50/50 split |
| 🍽️ | `Alimentación` | Default category |
| 🛍️ | `Souvenirs` | Common souvenirs (split) |
| 🛒 | `Compras personales` | NEVER split, personal tracking |
| ✈️ | `Transp. aeropuerto` | Bolt/Uber to airport |
| 🚆 | `Transp. ciudad común` | Train tickets that are split |
| 🌷 | `Transp. tulipanes` (or special spot) | Day trips |
| 🚇 | `Transp. personal` | OVpay/metro individual, no split |

## User preferences encoded

- Spanish output, casual tu-form
- No em dashes (use periods or parentheses)
- Save outputs to `~/Downloads/` first, then to sandbox `src/`
- Always open in browser after generating
- Filters must be functional (not decorative)
- Each row needs data attributes for filters to work
- Daily totals exclude hotel (note hotel separately)
- Per-person modals must include pre-paid items (flights, concert)
- "Para split" stat label, not "Común" (clarity)

## File outputs

1. `~/Desktop/Sandbox/trip-expenses/src/[destination]-[YYYY]-[MM].html` (canonical)
2. `~/Downloads/viaje-[destination]-gastos.html` (working copy, open in browser)
3. Update `~/Desktop/Sandbox/trip-expenses/CHANGELOG.md` with new trip entry

## When evaluating

Always:
1. Get destination context (Amsterdam vs Paris vs SF have different baselines)
2. Compute people-scaled metrics (€/p/day for food, etc.)
3. Decompose disparity into Layer 1/2/3
4. State verdict explicitly: racional / mixto / cash-mode
5. Be honest, not flattering. Internal tone, direct.

If user pushes back on a criticism, re-evaluate with their context. The Bolt CDG lesson: external "expensive" can be optimal when scaled and contextualized (time of day, safety, luggage). Don't double-down on a wrong call.

## Reference

- Sandbox project: `~/Desktop/Sandbox/trip-expenses/`
- Template: `~/Desktop/Sandbox/trip-expenses/src/template.html`
- Evaluation framework: `~/Desktop/Sandbox/trip-expenses/docs/evaluation-principles.md`
- First instance: `~/Desktop/Sandbox/trip-expenses/src/amsterdam-2026-04.html`