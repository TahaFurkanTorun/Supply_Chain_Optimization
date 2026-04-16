# Supply Chain Optimization
### Aggregate Production Planning, Promotion Strategy & Competitive Analysis

Linear programming model for a detergent manufacturer's 12-month production plan — extended to cover promotional pricing, demand forward buying, and a full competitive game theory analysis against a market rival.

Built with Python (Pyomo + GLPK). Written with Nil Erva Demir and Miray Köse.

---

## Problem

A detergent manufacturer (Q&H) operates with a fixed workforce of 100 employees and a monthly internal production cap of 160 tons. Demand fluctuates significantly across the year — peaking in April (610 tons) and bottoming out in August (220 tons). The company can subcontract excess demand at a premium ($1,250/ton vs. $1,000/ton internal cost) and must maintain a 100-ton safety stock throughout.

The analysis builds in layers:

1. **Cost minimisation** — find the cheapest way to meet 12 months of demand
2. **Profit maximisation** — same model, different objective
3. **Promotion analysis** — what happens to profit if price drops 10% for one month, triggering 40% demand uplift + forward buying from the next two months?
4. **Competitive analysis** — the market is a duopoly; how does Q&H's promotion decision interact with rival Unilock's moves?
5. **Game theory** — maximin strategy under uncertainty, and the case for (and against) coordination

---

## Results

### Baseline

| Objective | Value |
|---|---|
| Minimum total cost | $9,112,500 |
| Maximum profit (fixed price) | $3,470,700 |

Internal production runs at full capacity (160 tons/month) in all scenarios. Subcontracting covers the shortfall every month, peaking in April and December.

### Promotion timing

| Scenario | Q&H Profit |
|---|---|
| No promotion | $3,470,700 |
| Promote April | $3,571,700 |
| Promote August | $3,479,700 |

April promotion outperforms August — it captures a demand uplift earlier in the planning horizon and the forward buying effect pulls from May/June, which are lower-demand months anyway.

### Competitive payoff matrix

| Q&H \ Unilock | No promotion | Promotes April | Promotes August |
|---|---|---|---|
| **No promotion** | 3,470,700 / 3,470,700 | 2,997,950 / 3,571,700 | 3,295,200 / 3,479,700 |
| **Promotes April** | 3,571,700 / 2,997,950 | 3,252,300 / 3,252,300 | 3,396,200 / 3,006,950 |
| **Promotes August** | 3,479,700 / 3,295,200 | 3,006,950 / 3,396,200 | 3,357,650 / 3,357,650 |

### Maximin decision

Q&H's worst-case profit by action:
- No promotion → $2,997,950
- Promote April → **$3,252,300** ← highest floor
- Promote August → $3,006,950

**Promote in April** is the rational choice under uncertainty — it guarantees the best outcome in the worst case, regardless of Unilock's move.

---

## Key Takeaways

- **Promotion timing matters more than the discount itself.** April's higher base demand amplifies the 40% uplift effect significantly more than August's.
- **Forward buying creates a supply planning challenge.** The demand spike in the promotion month requires heavy subcontracting; the demand dip in the following months must be anticipated to avoid excess inventory.
- **Not promoting when the competitor does is the worst outcome.** A 50% demand drop in that month costs more in lost revenue than the entire benefit of running a promotion yourself.
- **Coordination is profitable but illegal.** Staggering promotions (one firm in April, one in August) avoids same-month cannibalization and is the jointly optimal outcome — but constitutes price-fixing under antitrust law.

---

## Stack

- **Language:** Python
- **Optimisation:** Pyomo + GLPK solver
- **Key packages:** `pyomo`

---

## Files

| File | Description |
|---|---|
| `supply_chain_optimization.ipynb` | Full model implementation and scenario analysis |
| `report.pdf` | Written analysis with model formulation and interpretation |
