# MPMR_Squamata

### Goal

Develop a workflow from Orthofinder to TreePuzzle on the whole proteome. 

### Dataset

Same-size groups (~8 species per group). 
Groups:

+ Agamidae
+ Pleurodonta
+ Serpentes
+ Others

### Steps

**First Check BUSCO genes**
    
    for file in *.iqtree; do sed -n '/LIKELIHOOD MAPPING STATISTICS/,$ { /Quartet support of areas 1-7 (mainly for clustered analysis):/,$!p }' "$file" > "$(basename "$file" .iqtree).extracted.txt"; done
    
    grep "^    5000" *extracted.txt > supported_topologies.txt

0. Genome quick annotation
1. Orthofinder
2. IQ-TREE2
3. ERCnet

    







