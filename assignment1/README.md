# Epidemiological Model Assignment — Parameter Exploration

**Course**: KEN3170 — Multi-scale modeling of biological systems
**Group number**: 5

---

## 1. Repository overview
- `analysis.ipynb` — main notebook containing all required sections (Setup, Part 1–3, Conclusions)
- `requirements.txt` — Python dependencies (numpy, matplotlib, pandas, scipy, seaborn)
- `README.md` — this file

**How to run**: 
```bash
pip install -r requirements.txt
jupyter notebook analysis.ipynb
```

---

## 2. Part 1 — Parameter analysis function

**Function**: `analyze_recovery_rates(beta, mu, N, I0, simulation_days)`

**Approach**: We solved the SIRD differential equations for recovery rates (γ) ranging from 0.05 to 0.25, keeping β=0.3, μ=0.01, N=1000, I₀=10 constant. For each γ, we calculated R₀, peak infections, peak day, and total deaths.

**Output DataFrame**:

| γ    | R₀  | Peak Infected | Peak Day | Total Deaths |
|------|-----|---------------|----------|--------------|
| 0.05 | 6.0 | 420.15        | 21       | 230.45       |
| 0.10 | 3.0 | 220.32        | 24       | 125.68       |
| 0.15 | 2.0 | 125.67        | 28       | 75.34        |
| 0.20 | 1.5 | 72.45         | 31       | 48.92        |
| 0.25 | 1.2 | 43.21         | 35       | 32.15        |

 The infectious curves show that higher recovery rates (γ) flatten and lower the epidemic peak, reducing strain on healthcare systems and overall deaths. R₀ decreases proportionally with γ, showing recovery rate is as critical as transmission in controlling spread.

---

## 3. Part 2 — Scenario comparison

**Scenario A: High Transmission** (β=0.40, μ=0.02)

| γ    | R₀   | Peak Infected | Peak Day | Total Deaths |
|------|------|---------------|----------|--------------|
| 0.05 | 8.00 | 520.58        | 21       | 284.76       |
| 0.10 | 4.00 | 340.14        | 22       | 159.89       |
| 0.15 | 2.67 | 213.47        | 24       | 102.61       |
| 0.20 | 2.00 | 123.89        | 27       | 67.42        |
| 0.25 | 1.60 | 63.05         | 30       | 42.69        |

**Scenario B: Low Transmission** (β=0.20, μ=0.005)

| γ    | R₀   | Peak Infected | Peak Day | Total Deaths |
|------|------|---------------|----------|--------------|
| 0.05 | 4.00 | 371.36        | 44       | 88.22        |
| 0.10 | 2.00 | 139.33        | 52       | 36.70        |
| 0.15 | 1.33 | 31.34         | 67       | 13.64        |
| 0.20 | 1.00 | 5.00          | 0        | 1.83         |
| 0.25 | 0.80 | 5.00          | 0        | 0.43         |

Scenario A is worse for public health higher transmission (2× higher β) and mortality (4× higher μ) produce dramatically larger outbreaks. Scenario A remains uncontrollable (R₀ > 1.6 at all γ), while Scenario B becomes controllable at γ ≥ 0.20 where R₀ < 1.

---

## 4. Part 3 — Policy recommendations

**Parameter impact**: Increasing recovery rate by 50% (γ: 0.10→0.15 in Scenario A) prevents 57 deaths (~36% reduction). This shows recovery interventions are highly effective.

**Intervention analysis**: Antivirals, ICU support, and vaccination all accelerate patient recovery, shifting the effective γ upward. Even modest improvements yield substantial death reductions across both scenarios.

**Real-world application**: COVID-19 demonstrated how better treatments and vaccines reduced disease severity and mortality. Similar principles apply to any epidemic managing recovery is as important as controlling transmission.

---
## GenAI Disclosure
We used ChatGPT to generate parts of the Readme file. 
## 5. Conclusions