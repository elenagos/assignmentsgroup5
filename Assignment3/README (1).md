# Scenario

- **Healthy Cell:** all nodes start at 0, including `DNA_damage`.
- **Stressed Cell:** `DNA_damage=1`; all other nodes start at 0.
- **Oncogene Hijacked Cell:** `MYC=1`; all other nodes start at 0.

Scenarios retain their initial states; mutation rules apply from the first update. Oncogene initialization differs from B's permanently activated MYC.

Outputs are (Growth, Death, p53). Single tuples denote fixed points; D's stressed scenario repeats the three listed phases.

| Network | Healthy Cell | Stressed Cell | Oncogene Hijacked Cell |
| --- | --- | --- | --- |
| Normal | (1, 0, 0) | (0, 1, 1) | (1, 0, 0) |
| A: p53 knockout | (1, 0, 0) | (1, 0, 0) | (1, 0, 0) |
| B: MYC amplification | (1, 0, 0) | (1, 0, 0) | (1, 0, 0) |
| C: MDM2 overexpression | (1, 0, 0) | (1, 0, 0) | (1, 0, 0) |
| D: p21 knockout | (1, 0, 0) | (0,0,1) → (0,1,1) → (0,1,0) → repeat | (1, 0, 0) |

Normal and A–C reach fixed points. Under stress, Normal reaches death, A–C reach growth, and D cycles without a single final state.

Cancer-like growth requires `DNA_damage=1`, `Growth=1`, and `Death=0`; growth without DNA damage is normal in this model.

## 2. Attractor analysis

All 256 initial states receive equal weight. Synchronous transitions continue until recurrence: one repeating state indicates a fixed point; multiple states indicate cycles.

Normal and A–C have only fixed points. D has three fixed points and two period-3 cycles, detected by tracking all previously visited states.

### Final comparison table

| Network | Fixed points | Cycles | Total attractors | Cancer-like basin | Cancer-like percentage | Cyclic basin |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Normal | 3 | 0 | 3 | 8/256 | 3.125% | 0% |
| A: p53 knockout | 2 | 0 | 2 | 128/256 | 50% | 0% |
| B: MYC amplification | 2 | 0 | 2 | 128/256 | 50% | 0% |
| C: MDM2 overexpression | 2 | 0 | 2 | 128/256 | 50% | 0% |
| D: p21 knockout | 3 | 2 | 5 | 8/256 | 3.125% | 37.5% |

| Network | Growth without DNA damage | Death fixed point | Cancer-like fixed point | Cyclic attractors |
| --- | ---: | ---: | ---: | ---: |
| Normal | 128 | 120 | 8 | 0 |
| A: p53 knockout | 128 | 0 | 128 | 0 |
| B: MYC amplification | 128 | 0 | 128 | 0 |
| C: MDM2 overexpression | 128 | 0 | 128 | 0 |
| D: p21 knockout | 128 | 24 | 8 | 56 + 40 = 96 |

Columns group outcomes by phenotype. D's death fixed point differs from Normal because p21 is OFF.

Constant DNA damage limits cancer-like basins to 50% overall. Among damaged initial states only, percentages are 6.25% for Normal/D and 100% for A–C.

### Full attractor states for Normal and A–C

Normal has H, X and K. A, B and C have H and K only.

| Attractor | DNA_damage | p53 | MYC | CDK2 | MDM2 | p21 | Growth | Death |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| H: growth without DNA damage | 0 | 0 | 1 | 1 | 1 | 0 | 1 | 0 |
| X: death with DNA damage | 1 | 1 | 0 | 0 | 0 | 1 | 0 | 1 |
| K: cancer-like growth | 1 | 0 | 1 | 1 | 1 | 0 | 1 | 0 |

### Mutation D: all attractors and cycle phases

| D attractor | Type | Basin size | Percentage of all 256 states |
| --- | --- | ---: | ---: |
| D1: growth without DNA damage | Fixed point | 128 | 50% |
| D2: cycle 1 | Period 3 | 56 | 21.875% |
| D3: death with DNA damage | Fixed point | 24 | 9.375% |
| D4: cycle 2 | Period 3 | 40 | 15.625% |
| D5: cancer-like growth | Fixed point | 8 | 3.125% |

D's five attractors contain nine recurrent states. Cycle phases repeat in the listed order; identifiers match the notebook heatmap.

| Attractor / phase | DNA_damage | p53 | MYC | CDK2 | MDM2 | p21 | Growth | Death |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| D1: fixed point | 0 | 0 | 1 | 1 | 1 | 0 | 1 | 0 |
| D2: phase 1/3 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| D2: phase 2/3 | 1 | 1 | 0 | 0 | 1 | 0 | 0 | 1 |
| D2: phase 3/3 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |
| D3: fixed point | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 1 |
| D4: phase 1/3 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 0 |
| D4: phase 2/3 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 |
| D4: phase 3/3 | 1 | 0 | 0 | 0 | 1 | 0 | 0 | 1 |
| D5: fixed point | 1 | 0 | 1 | 1 | 1 | 0 | 1 | 0 |

D's stressed scenario reaches D2. Starting phases are arbitrary; the two cycles attract 96 states, representing 37.5% of all initial states.

## 3. Which mutation is most dangerous, and why?

**A–C tie as most dangerous:** each has a 50% cancer-like basin, versus Normal's 3.125%—a 16-fold increase. All convert stressed outcomes from death to growth.

The mechanisms differ:

- **A** directly removes p53 activity, disabling the model's p53-dependent growth inhibition and Death activation.
- **B** forces MYC ON. MYC activates MDM2, which suppresses p53; p21 then falls, permitting CDK2 and Growth activity.
- **C** forces MDM2 ON, suppressing p53 and subsequently p21, allowing MYC and CDK2 to support growth.

D retains Normal's cancer-like percentage but redirects 96 states into cycles. Direct inhibition by p53 remains; neither cycle activates Growth.

The metric cannot distinguish A–C's severity. Basin percentages describe equally weighted model starting states, not a person's probability of developing cancer.

## 4. What role do feedback loops play?

In the normal rules, MYC activates MDM2, MDM2 inhibits p53, and p53 inhibits MYC. Closing the loop gives:

```text
MYC activates MDM2; MDM2 inhibits p53; p53 inhibits MYC.
```

Two inhibitory interactions make this feedback loop positive: MYC promotes its own activity through MDM2-mediated p53 suppression; p53 activates p21, reinforcing MYC inhibition.

With DNA damage, initial conditions determine whether Normal reaches p53-active death or MYC/MDM2-active growth, explaining its small cancer-like basin.

A–C fix loop components, eliminating the death attractor. D removes p21-mediated inhibition of MYC and CDK2, producing two cycles under synchronous updates.

Feedback structure alone cannot prove oscillation. D's cycles follow these synchronous Boolean rules; they do not demonstrate biological oscillatory homeostasis.

## 5. Three specific limitations

1. **Binary states:** Nodes are only ON or OFF; simplified mutation rules cannot represent graded activity, expression levels, or dosage effects.

2. **Synchronous timing:** All nodes update simultaneously without delays or randomness; alternative update schedules could change trajectories, attractors, and resulting conclusions.

3. **Biological scope:** Repair and irreversible death are absent; equally weighted initial states are artificial, so basin percentages cannot predict cancer risk.

