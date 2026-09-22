






- **Healthy Cell:** all nodes start at 0, including `DNA_damage`.
- **Stressed Cell:** `DNA_damage=1`.
- **Oncogene Hijacked Cell:** `MYC=1` and `DNA_damage=0`.

The oncogene scenario is an initial condition, whereas Mutation B forces MYC ON at every update. To match the notebook, the scenario initial states are retained for all networks. Mutant update rules take effect on the first update; B and C are not necessarily ON at time zero. This timing can affect intermediate trajectories. All three supplied scenarios start with p21=0. The exhaustive enumeration also includes p21=1 at time zero; the D rule forces it OFF from the first update. This preserves the same all-256-state enumeration convention across networks.

The table reports attractor outputs as **(Growth, Death, p53)**. Single tuples are fixed points; the D stressed scenario repeats a three-phase cycle:

| Network | Healthy Cell | Stressed Cell | Oncogene Hijacked Cell |
| --- | --- | --- | --- |
| Normal | (1, 0, 0) | (0, 1, 1) | (1, 0, 0) |
| A: p53 knockout | (1, 0, 0) | (1, 0, 0) | (1, 0, 0) |
| B: MYC amplification | (1, 0, 0) | (1, 0, 0) | (1, 0, 0) |
| C: MDM2 overexpression | (1, 0, 0) | (1, 0, 0) | (1, 0, 0) |
| D: p21 knockout | (1, 0, 0) | (0,0,1) → (0,1,1) → (0,1,0) → repeat | (1, 0, 0) |

Normal and A–C reach fixed points in all three scenarios. A, B, and C allow growth despite the stressed scenario's persistent DNA damage; the normal network reaches death. D reaches growth fixed points in the Healthy and Oncogene scenarios, but its Stressed scenario reaches a period-3 cycle. It has no single final state. The displayed cycle can be rotated without changing its identity.

Growth without DNA damage is the normal growth outcome in this practical; Growth=1 alone is not sufficient to classify a state as cancer-like.

## 2. Attractor analysis

Eight binary nodes give `2^8 = 256` possible starting states, each given equal weight. The complete analysis follows synchronous transitions until any state repeats. A period-1 recurrence is a fixed point; a longer repeating sequence is a cyclic attractor. The complete analysis accounts for all 256 states in each network.

Normal and A–C have only fixed points. **D has three fixed points and two period-3 cycles.** Checking only whether the last two states match would miss D's cycles, so the complete comparison tracks every previously visited state.

### Definition of cancer-like growth

A point is cancer-like when:

```python
DNA_damage == 1 and Growth == 1 and Death == 0
```

For an attractor containing multiple phases, the main **persistent cancer-like** metric requires the condition in every phase. The notebook also calculates an **any-phase** metric, which requires the condition in at least one phase. Neither D cycle has Growth ON, so both metrics give the same percentages for all five networks.

A basin contains all starting states that eventually reach an attractor. The cancer-like basin percentage is `100 × number of starting states reaching cancer-like attractors / 256`. It is not the percentage of time spent in a state.

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

The columns above group outcomes by phenotype; the full death fixed point in D differs from normal because p21 is OFF.

DNA damage remains constant. The 128 starting states with `DNA_damage=0` cannot reach a cancer-like attractor, so 50% is the maximum across all 256 states under this definition. If only the 128 DNA-damaged starting states are considered, the cancer-like percentages are 6.25% for Normal/D and 100% for A/B/C. These use a different denominator.

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

There are five attractors containing nine recurrent states altogether. Each cycle's phases follow the order below and then return to phase 1. The D identifiers match the notebook heatmap.

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

D's stressed scenario reaches D2. A cycle's first listed phase is an identification convention, not a final resting state. The two cycles together attract `56 + 40 = 96` starting states, giving `96/256 = 37.5%`.

## 3. Which mutation is most dangerous, and why?

**A, B, and C tie as the most dangerous mutations according to the cancer-like basin metric.** Each sends 128 of 256 starting states to cancer-like growth, compared with 8 of 256 in the normal network. This is a 16-fold increase, or an increase of 46.875 percentage points. All three also turn the stressed scenario's final outcome from death to growth.

The mechanisms differ:

- **A** directly removes p53 activity, disabling the model's p53-dependent growth inhibition and Death activation.
- **B** forces MYC ON. MYC activates MDM2, which suppresses p53; p21 then falls, permitting CDK2 and Growth activity.
- **C** forces MDM2 ON, suppressing p53 and subsequently p21, allowing MYC and CDK2 to support growth.

D has the same cancer-like basin percentage as normal, but different dynamics. Its death fixed-point basin contains 24 states instead of 120, and its two cycles attract 96 states. The stressed scenario cycles rather than reaching a death fixed point. Direct p53 inhibition remains, and neither cycle activates Growth, so these changes do not increase the measured cancer-like basin. The cancer-like metric alone therefore misses an important effect of the p21 knockout.

There is no quantitative basis here to rank A, B, and C against each other. Basin percentages describe model starting states, not a person's probability of developing cancer.

## 4. What role do feedback loops play?

In the normal rules, MYC activates MDM2, MDM2 inhibits p53, and p53 inhibits MYC. Closing the loop gives:

```text
MYC activates MDM2; MDM2 inhibits p53; p53 inhibits MYC.
```

This is an **overall positive feedback loop**: the loop contains two inhibitory interactions. High MYC can raise MDM2, reducing p53 and thereby removing inhibition of MYC. Conversely, active p53 inhibits MYC, reducing MDM2 and helping p53 remain active when DNA damage is present. The p53-to-p21 pathway reinforces MYC inhibition because p53 activates p21 and p21 inhibits MYC.

These relationships support alternative stable outcomes. With DNA damage present, the normal network can settle into either the p53-active death state or the MYC/MDM2-active growth state, depending on its starting state. This explains why the normal network already has a small cancer-like basin.

A, B, and C each fix one component of this loop and eliminate the death attractor under the implemented rules. D leaves the direct MYC–MDM2–p53 loop intact but removes the p53 → p21 inhibitory route to MYC and CDK2. Under the specified synchronous updates, this alteration produces two period-3 cycles for some starting states.

Normal and A–C show only fixed points; D shows both fixed points and cycles. Feedback structure can help explain the results, but a loop in the interaction graph alone does not prove oscillation. D's observed cycles are a mathematical result of these rules, not evidence of biological oscillatory homeostasis.

## 5. Three specific limitations

1. **Binary states and simplified mutation rules.** Every node is only 0 or 1, so the model cannot distinguish weak, moderate, or strong activity. Forcing a node ON or OFF is an idealized representation of amplification or knockout. The results describe these Boolean rules and cannot quantify expression levels or dose-dependent effects.

2. **Synchronous, deterministic timing.** Every node updates together from the previous state, with no node-specific delays or random variation. Update steps have no calibrated duration. Different update schedules could change trajectories and attractors, so this analysis does not establish real biological response times or robustness to asynchronous updates.

3. **Restricted biological scope and artificial starting-state sampling.** The model contains only eight nodes, keeps DNA damage constant, and has no repair process, external growth-signal dynamics, or irreversible death mechanism. In particular, the 'repairable' stressed label does not correspond to an implemented repair process. D's cycles switch the Death output ON and OFF; this exposes the missing irreversible-death mechanism and should not be interpreted as real cells dying and reviving. Giving all 256 binary combinations equal weight also does not represent a measured distribution of cell states. The basin percentages are useful for comparing the models under a shared convention, but they are not experimentally validated cancer probabilities.

## Reproducing the analysis

Open `analysis3_complete_with_d.ipynb` and run its cells in order in an environment with NumPy, Matplotlib, pandas, NetworkX, and seaborn installed. The complete Mutation D section calculates the Normal/A/B/C/D comparison from the network rules, reports fixed points and full cyclic scenario outcomes, and generates the comparison plots. The D-versus-normal basin charts include all five D attractors, and its heatmap displays all nine recurrent states.
