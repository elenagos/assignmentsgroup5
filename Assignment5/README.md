# Assignment 5. Plant Tissue Simulations

## 1. Pathogen infection simulation

### Initial state

![0 min](images/Task1.initialstate.png)!
### 30 min

![30 min](images/Task130min.png)

### 60 min

![60 min](images/Task160min.png)

### 90 min

![90 min](images/Task190min.png)

### 120 min

![120 min](images/Task1120min.png)

### Description

Spread: In the very beginning, the tissue is highly ordered, with long and vertical cells with straight cell walls. During the simulation the cells progressively start to deform and spread from the initially affected region(in the center of the tissue) into surrounding tissues(closer to borders, left part of the tissue was changed the most). 
How the tissue deforms: cells close to this region change shape from rectangle shape to circle one first, then surrounding cells start to deform. Therefore, the tissue becomes less organized. Tissues in the left upper corner seem to be deformed the most, while cells in the right part seem to be more organized and cells have form of rectangles/squares.

## 2. Infection analysis

### How is a cell wall stiffness reduced as a function of chemical level?

The cell wall stiffness is reduced when level of the chemical 0 in a normal cell increases. First, we divide the chemical level by 0.5 and limit the result to 1.2. If this level is above 0.1, the wall stiffness is calculated as 3 - patho_chem_level. That means that a higher chemical level makes the wall softer(but stiffness never falls to 0).

### What does the pathogen do differently?

The pathogen cells are excluded from the chemical-dependent wall weakening. Their wall stiffness stays at the default value. But pathogen cells grow, they can divide and produce the chemical signal. This chemical then spreads to neighbouring cells and weakens their walls, that leads to tissue deformation.
## 3.Diffusion coefficient

the diffusion coefficient in CelltoCellTransport inversely correlates with the stiffnes of the wall.The is stiffness calculated by getLengthAndStiffness, which combines wall element stiffnesses from both adjacent cells, and lengths are used as the weights. the transport term is 
\[
\phi=L\,D\,(C_2-C_1)
\]
So,transport increases with wall length, the diffusion coefficient, and the chemical concentration difference. The area-dependent factors corr1 and corr2 then scale the changes in the two cells. As diffusion coefficient increases chemicals ove into neighboring cells faster and when chemicals reach plant cells wall stiffnes decreases and diffusion coefficient increases again.
## 4. rel_cell_div_threshold
IF cell is a pathogen:increase target area

IF actual area > rel_cell_div_threshold × base area:
        divide cell

So when we change rel cel div threshold two sitation can happen:

if we decrease the threshold cells can divide at a smaller size,pathogen cell count increases.
if we increase threshold cells must grow larger before dividing, generally delaying population expansion.
## 5.Difference to other models we worked with
tissue spatial geometry is a huge factor in determining interactions between cells.Cells interact through shared walls, and transport depends on the properties of those walls.This is different to fixed neighbor models where interactions follow rules. here interactions can change based on changes in cells

## 6.Plant defence

we would put this at the end of CellHouseKeeping after the whole if/else that weakens the walls. This way the defence sets the stiffness last so the weakening rule does not overwrite it. It only applies to plant cells not pathogen cells which are type 2 ^^

```text
\(^o^)/
keep the existing pathogen growth and division rules
keep the wall setup and wall weakening rules

IF cell type is not 2 AND chemical 0 > defence threshold:
    FOR each wall element of the cell:
        set stiffness to a chosen value above 3
```

3 is the normal stiffness so a value above it makes the wall stiffer. The threshold and the higher stiffness would be values we choose

this adds negative feedback. More chemical triggers stiffer walls which lowers diffusion and slows the chemical spreading to nearby cells. So it works against the original loop where chemical makes walls softer and spreads faster  :)    
