# Squamata Metabolomics

This project aims to investigate about how positively selected mitochondrial OXPHOS impact the proteomic network.
 
For this purpose, we selected the clade of Serpentes (Order: Squamata), that shows adaptive amino acidic changes in core mtOXPHOS subunits.
Literature posits these changes to new metabolic necessities. 
Thus, we expected a highly impacted proteomic network and a particularly strong covariation between mithocondrial proteins and other metabolic pathways. 

Furthermore, snakes shows a suggestive case of convergent evolution among OXPHOS proteins (especially mtOXPHOS) with agamids (Family: Agamidae).
Likely due to an ancient episode of metabolic convergence, the signal is so radicated that agamids are consistently placed as the sister taxon of snakes
 when using mitochondrial markers (instead of being closely related to Pleurodonta). 
Using a mirror tree approach, we demonstrated that also nucOXPHOS genes show agamids-snakes sister relationship.
Thus, we aim to demonstrate that a larger set of nuclear proteins carry within themself the same phyogenetic signal. 

The project is splitted into two blocks: 
1. **path A** detecting proteins highly dependent from mitochondrial genes using whole-proteome evolutionary rates covariation (note: we assume that the starting point of selection is the mitogenome, but this assumption
 could be debated).
2. **path B** detecting proteins that show signals of convergence between agamids and snakes towards the OXPHOS-biased topology.

### Dataset

Four groups:

+ **Acrodonta** - 5 species
+ **Pleurodonta** - 5 species
+ **Serpentes** - 13 species
+ **Others** (Gekkota, Scincomorpha, Laterata, Anguiformes) - 11 species 

Outgroup: _Sphenodon punctatus_ (Order: Rhyncocephalia)

---

### Preliminary Steps - common to both path A and B

- [x] Define dataset (`dataset.xlxs`) and download proteomes (*protein.faa*) files
- [x] Remove putative isoforms with [`primary_transcript.py`](https://github.com/davidemms/OrthoFinder/blob/master/tools/primary_transcript.py). 
- [x] Edit headers (`>species__protein`). 
- [x] Identify mitochondrial genes and add sequences to proteomes where lacking. See `mtOXPHOS/`.
- [x] Run OrthoFinder 

      orthofinder -f [path/to/dir/containing/proteomes/] -y -X -M msa -t [number of threads available]

---

### Path A - snakes erc

BSc Student: `Giorgia Galletti`

*Evolutionary Rates Covariation* as in [`ERCnet`](https://github.com/EvanForsythe/ERCnet/tree/main)).

- [x] Phylogenomic analyses

		./Phylogenomics.py -j p1r7_julia -r 7 -p 1 -l 100 -m 32 -P 0.9 -b 85 -T -o <path/to/orthofinder/results/>

- [x] Gene-tree/Species-tree reconciliation 
	
		./GTST_reconciliation.py -j p1r7_julia

- [x] ERC analysis

		./ERC_analyses.py -j p1r7_julia -m 32 -b R2T -s Nnaj

- [x] Network analysis

		./Network_analyses.py -j p1r7_julia -p 0.05 -r 0.5 -c pearson -m R2T -S -y fg -s Nnaj -F -L 

Network analyses were inspected with Cytoscape. 

- [] The closest proteins of mitochondrial genes were extracted from the network and submitted to InterProScan, specifying GO terms seach.
We did it for both a relaxed network (./Network_analyses.py -p 0.05 -r 0.5) and a more stringent search (-p 0.001 and -r 0.7).
- [] GO terms were analsysed with g:profiler (** pipeline mirko **)

---

### Path B - disentangling OXPHOS-biased phylogenetic signal

BSc Student: `Nicolò Gabriele`

- [x] From the Orthofinder output, we selected orthogroups with no more than three paralogs per species and at least 33 species out of 35.
- [x] We performed gene-tree inferences using IQTREE2 (-m MFP, -B 1000)
- [x] In order to identify the correct paralog, we pruned gene trees with [PhyloPyPruner](https://github.com/fethalen/phylopypruner).

PhyloPyPruner filters:
+  a. < 10% gaps in gene alignments
+  b. > 100 aa gene length
+  c. pruning species with branch length equal to or 20 times the median length of all branches.
+  d. removing genes if there is not monophyletic origin in each group (Acrodonta, Pleurodonta or Serpentes).
+  e. removing genes if there is not > 80% bootstrap support for the deep node of Acrodonta, Pleurodonta and Serpentes.  

- [x] Likelihood Mapping Analysis

		iqtree2 -s gene.fa -m TEST -lmap 5000 -n 0 -lmclust species_clusters.nex

NB: species_clusters.nex comprised four clusters: Acrodonta, Pleurodonta, Serpentes and Others (the remaining species).

Script to create the supports table:

		for file in *.iqtree; do sed -n '/LIKELIHOOD MAPPING STATISTICS/,$ { /Quartet support of areas 1-7 (mainly for clustered analysis):/,$!p }' "$file" > "$(bas>
    		grep "^    5000" *extracted.txt > supported_topologies.txt 

- [] Robinson-Foulds distances
- [] Coalescent-based tree
- [] Mitochondrial tree

---
 
### Serpentes mtDNA extractions

    miniprot --gff SpeciesX_Genome.fasta SpeciesY_mtDNA.faa > SpeciesX_aln.gff
  
    sed -E 's/Target=([^ ;]+) [^;]*/Name=\1;Target=\1/' SpeciesX_aln.gff > SpeciesX_named.gff
  
    agat_sp_extract_sequences.pl -g SpeciesX_named.gff -f SpeciesX_Genome.fasta -p --table 2 -o SpeciesX_mt.fasta

