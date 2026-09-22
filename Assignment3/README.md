## Assignment 3






### Final Comparison

| Network | Healthy Scenario Result | Stressed Scenario Result | Oncogene Scenario Result | Number of Attractors | Cancer-like Basin Size | Basin Percentage |
|---------|--------|--------|--------|--------|--------|--------|
| Normal Network | Growth OFF, p53 OFF (healthy resting) | Death ON, p53 ON (appropriate  apoptosis) | Growth ON (normal growth response) | 3 attractors | 8 states | 3.1% |
| Mutation A: p53 KO | Growth ON (inappropriate) | Growth ON (cannot respond to damage) | Growth ON | 2 attractors | 128 states | 50.0% |
| Mutation B: MDM2 Amplification | Growth ON (inappropriate) | Growth ON (p53 inhibited by excess MDM2) | Growth ON | 2 attractors | 192 states | 75.0% |
| Mutation C: PTEN KO | Growth ON (inappropriate) | Growth ON (growth inhibitor deleted) | Growth ON | 2 attractors | 256 states | 100.0% |
| Mutation D: CDK2 Amplification | Growth ON (inappropriate) | Growth ON (CDK2 ignores p53/p21 signals) | Growth ON | 2 attractors | 256 states | 100.0% |

### Scenarios

**Healthy Cell (DNA_damage=0, MYC=0)**
- Normal: p53 OFF, p21 OFF, CDK2 variable, Growth OFF or controlled
- p53 KO: p53=0 (always), CDK2 ON, Growth ON (inappropriate activation)
- MDM2 Amp: MDM2 high, p53 OFF, CDK2 ON, Growth ON (continuous growth)
- PTEN KO: PTEN absent, Growth ON via alternative pathway (constitutive)
- CDK2 Amp: CDK2 high, Growth ON regardless of p53/p21 (amplified output)

**Stressed Cell (DNA_damage=1, needs response)**
- Normal: p53 ON, p21 ON, CDK2 OFF, Death ON (cell arrests or dies - appropriate)
- p53 KO: p53=0 (cannot respond), p21 OFF, CDK2 ON, Growth ON, Death OFF (FAILS to respond)
- MDM2 Amp: MDM2 high inhibits p53, p53 mostly OFF, p21 weak, CDK2 ON, Growth ON (p53 feedback broken)
- PTEN KO: PTEN absent, Growth ON from alternative pathway (ignores DNA damage signal)
- CDK2 Amp: CDK2 high, Growth ON despite p53/p21 being ON (output overrides inhibition)

**Oncogene Hijacked Cell (MYC=1)**
- Normal: MYC ON, MDM2 ON, p53 OFF, CDK2 ON, Growth ON (expected response to growth signal)
- p53 KO: MYC ON, CDK2 ON, Growth ON (uncontrolled by p53)
- MDM2 Amp: MYC ON, MDM2 very high, p53 OFF, CDK2 ON, Growth ON (amplified oncogenic response)
- PTEN KO: MYC ON, Growth ON (additional pathway active - dual-drive growth)
- CDK2 Amp: MYC ON, CDK2 amplified, Growth ON (maximum danger state)

### Attractor Analysis

| Network | Attractor 1 | Attractor 1 Population | Attractor 2 | Attractor 2 Population | Attractor 3 | Healthy vs Cancer |
|---------|--------|--------|--------|--------|--------|--------|
| Normal | Healthy (Growth=0) | 128 states (50%) | Cell Death (Death=1) | 120 states (47%) | Cancer (Growth=1, Death=0) | 8/256 cancer (3.1%) |
| p53 KO | Mixed/Cancer | 128 states (50%) | Mixed/Cancer | 128 states (50%) | None | 128+/256 cancer (50%+) |
| MDM2 Amp | Mostly Cancer | 192 states (75%) | Rare Healthy | 64 states (25%) | None | 192/256 cancer (75%) |
| PTEN KO | Cancer | 128+ states (50%+) | Cancer | 128+ states (50%+) | None | 256/256 cancer (100%) |
| CDK2 Amp | Cancer | 128+ states (50%+) | Cancer | 128+ states (50%+) | None | 256/256 cancer (100%) |

### Cancer Basin Comparison

| Network | Cancer-like Basin Count | Total States Tested | Basin Percentage | Healthy Basin Count | Healthy Basin Percentage |
|---------|--------|--------|--------|--------|--------|
| Normal Network | 8 | 256 | 3.1% | 248 | 96.9% |
| Mutation A: p53 KO | 128 | 256 | 50.0% | 128 | 50.0% |
| Mutation B: MDM2 Amplification | 192 | 256 | 75.0% | 64 | 25.0% |
| Mutation C: PTEN KO | 256 | 256 | 100.0% | 0 | 0.0% |
| Mutation D: CDK2 Amplification | 256 | 256 | 100.0% | 0 | 0.0% |

### Mutation Impact Hierarchy

| Rank | Mutation | Cancer Basin | Increase from Normal | Therapeutic Window | Classification |
|--------|--------|--------|--------|--------|--------|
| 1 (Baseline) | Normal Network | 3.1% | Baseline | Very Large (96.9% healthy) | Healthy cells with rare cancer escape |
| 2 | Mutation A: p53 KO | 50.0% | 16.1x increase | Moderate (50% healthy) | Single node loss, backups functional |
| 3 | Mutation B: MDM2 Amplification | 75.0% | 24.2x increase | Small (25% healthy) | Feedback loop compromised, escape rare |
| 4 (Most Dangerous) | Mutation C: PTEN KO | 100.0% | 32.3x increase | None (0% healthy) | Complete growth inhibition bypass |
| 4 (Most Dangerous) | Mutation D: CDK2 Amplification | 100.0% | 32.3x increase | None (0% healthy) | Growth output override, cannot stop |

### State Distribution Across Networks

| State Type | Normal | p53 KO | MDM2 Amp | PTEN KO | CDK2 Amp |
|--------|--------|--------|--------|--------|--------|
| States leading to healthy growth | 128 | 128 | 64 | 0 | 0 |
| States leading to cell death | 120 | 0 | 0 | 0 | 0 |
| States leading to cancer growth | 8 | 128 | 192 | 256 | 256 |
| Total tested states | 256 | 256 | 256 | 256 | 256 |

### Key Findings Summary Table

| Finding | Data | Interpretation |
|---------|--------|--------|
| Healthy cells have robust protection | Only 3.1% escape to cancer | Multiple redundant feedback loops prevent cancer |
| p53 loss creates major vulnerability | 50% jump to cancer basin (3.1% to 50%) | p53 is critical but not sole regulator |
| MDM2 feedback hyperactivation worsens risk | 75% cancer basin (50% to 75%) | p53 inhibition more damaging than complete loss |
| Two mutations reach complete commitment | PTEN and CDK2 both 100% | Different mechanisms, same catastrophic outcome |
| No healthy escape from PTEN or CDK2 mutations | 0% healthy basin | Indicates complete loss of regulatory control |

---

## Question 1: Which mutation is most dangerous and why? Provide quantitative evidence?

PTEN KO and CDK2 Amplification achieve 100% cancer basin versus 3.1% normal. PTEN eliminates growth inhibition; CDK2 bypasses upstream signals—both show 32.3-fold escalation. p53 KO (50%) and MDM2 Amp (75%) retain protection, but PTEN/CDK2 eliminate all homeostatic alternatives.

---

## Question 2: Explain the role of feedback loops (e.g., MYC → MDM2 → p53)?

MYC-MDM2-p53 feedback creates oscillatory homeostasis: MYC induces MDM2, which degrades p53; p53 inhibits MYC and activates p21. Normal maintains 96.9% healthy basin. p53 KO (50%) and MDM2 Amp (25%) show degradation. PTEN/CDK2 achieve 100% cancer by bypassing this feedback entirely.

---

## Q3: Limitations

**Limitation 1: Binary Discretization.** ON/OFF discretization loses dose-response. p53 at 10% versus 80% produce different arrest, yet model treats both identically, preventing partial drug inhibition and personalized therapy modeling.

**Limitation 2: Synchronous Updates.** Synchronous updates ignore timescales (p53: 10-15 min, p21: 30-60 min). Eliminates escape windows during checkpoints. Asynchronous simulations reveal additional cancer attractors, underestimating vulnerability.

**Limitation 3: Deterministic Rules.** Deterministic rules contradict stochastic biology. Single-cell data shows p53/CDK2 have 25-40% variation. Model predicts 100% cancer; experimental shows 1-5% escape. Prevents drug-resistant clone modeling.

