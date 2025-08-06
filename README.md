# Squamata Mitonuclear Coevolution 

### Background
This project aims to explore the impact of mitonuclear coevolution across entire proteomes. In brief, it is possible to predict the interaction and/or a shared evolutionary history between two proteins calculating the correlation between their evolutionary rates. The higher the Pearson’s correlation coefficient, the stronger the presumed coevolution. Applying this concept to the entire proteome of a species dataset allows the identification of nuclear proteins that coevolve with mitochondrial ones.

For this purpose, we selected the clade of Serpentes (Order: Squamata), that shows adaptive amino acidic changes in core mtOXPHOS subunits.
Literature posits these changes to new metabolic necessities. 
Thus, we expected a highly impacted proteomic network and a particularly strong covariation between mithocondrial proteins and other metabolic pathways. 

Mitonuclear coevolution may also significantly affect phylogenetic inference. In fact, coevolving genes often support the same phylogenetic topology. In some cases, mitochondrial markers that support a topology that contradict that inferred from nuclear genes (a phenomenon called mitonuclear phylogenetic discordance). In such instances, subsets of nuclear genes that interact closely with mitochondrial proteins or, or have experienced similar selective pressures, may adopt the same biased topology as the mitochondrial one.
To investigate the extent of this phenomenon, we used the same species dataset of squamate species. In fact, the Squamata order is characterized by a deep mitonuclear phylogenetic discordance: mitochondrial markers sustain a close sister relationship between the Agamidae and snakes, whereas morphology and nuclear genes support Agamidae+Pleurodonta. This discordance has been attributed to convergent evolution and selective pressures on the OXPHOS pathway in both agamids and snakes. Indeed, some nuclear OXPHOS genes have already been shown to support the mitochondrial topology. While some researchers suggest an ancient shared selective pressure on the oxidative phosphorylation pathway in both lineages, this remains speculative.
This unique case provided an opportunity to test which other (non-OXPHOS) nuclear genes might support the mitochondrial-biased topology. To avoid bias from a priori gene selection, we analyzed the entire nuclear proteome and performed topological tests on thousands of proteins. 

The project is splitted into two blocks: 

1. **Evolutionary Rate Covariations** detecting proteins highly dependent from mitochondrial genes using whole-proteome evolutionary rates covariation (note: we assume that the starting point of selection is the mitogenome, but this assumption
 could be debated).
2. **MitoNuclear Discordances** detecting proteins that show signals of convergence between agamids and snakes towards the OXPHOS-biased topology.

### Dataset

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

### Evolutionary Rate Covariations - SNAKES

BSc Student: `Giorgia Galletti`

*Evolutionary Rates Covariation* as in [`ERCnet`](https://github.com/EvanForsythe/ERCnet/tree/main)).

- [x] Phylogenomic analyses

		./Phylogenomics.py -j p2r8_julia -r 8 -p 2 -l 99 -m 32 -P 0.9 -b 85 -T -o <path/to/orthofinder/results/>

- [x] Gene-tree/Species-tree reconciliation 
	
		./GTST_reconciliation.py -j p2r8_julia

- [x] ERC analysis

		./ERC_analyses.py -j p2r8_julia -m 32 -b R2T -s Nnaj

- [x] Network analysis

		./Network_analyses.py -j p2r8_julia -p 0.001 -r 0.8 -c pearson -m R2T -S -y fg -s Nnaj -F -L 

Networks were inspected with custom made python3 scripts. 

- [x] The closest proteins of mitochondrial genes were extracted from the network and submitted to InterProScan, specifying GO terms seach.
We did it for both a relaxed network (./Network_analyses.py -p 0.05 -r 0.5) and a more stringent search (-p 0.001 and -r 0.7).
- [x] GO terms were analsysed with g:profiler (** pipeline mirko **)

---

### MitoNuclear Discordances - disentangling OXPHOS-biased phylogenetic signal in SQUAMATA

BSc Student: `Nicolò Gabriele`

### Dataset construction

- [x] From the Orthofinder output, we selected orthogroups with no more than two paralogs per species.
- [x] We performed gene-tree inferences using IQTREE2 (-m MFP, -B 1000). 
- [x] In order to identify the correct paralog, we pruned gene trees with [PhyloPyPruner](https://github.com/fethalen/phylopypruner).

PhyloPyPruner filters:
 - [x] Remove sequences shorter than 100 bases
 - [x] Remove branches longer than 5 standard deviations of all branches
 - [x] Collapse branches with less support than 60% into polytomies
 - [x] Mask monophylies using the pdist method
 - [x] Discard output alignments with fewer than 20 sequences

Command:

		phylopypruner --threads 20 --no-supermatrix --dir /home/PERSONALE/oscar.wallnoefer2/05_Squamata_2.0/01_Input_Orthofinder_All/01_genetrees/03_phylopypruner/ --min-len 100 --trim-lb 5 --mask pdist --outgroup Spun --prune LS --min-taxa 20 --min-gene-occupancy 60

### Phylogenetic Inference: Mitochondrial Tree vs Nuclear Tree

- [x] Coalescent-based tree

                iqtree2 -s [gene.fasta] -m MFP -B 1000 -T AUTO
                /path/to/astral4 -i input_astral.treefile -o AstralIV           

- [x] Robinson-Foulds distances

        	for a in *.treefile; do phykit robinson_foulds_distance ${a} [species_tree.treefile]; done            

- [x] Concatenated Maximum Likelihood Tree restricted to those genes that have all the 35 species. 

		iqtree2 -s concatenated.out -p partitions.txt -m MFP+MERGE -B 1000 

- [x] Gene Concordance Factors

		iqtree2 -t [species_tree_topology_squamata.treefile] --gcf [input_gCF.treefile] -pre gCF_squamata --cf-verbose

- [x] Mitochondrial tree

        	iqtree2 -s concatenated.out -p partitions.txt -m MFP+MERGE -b 100 -T AUTO 


### Topological Test 

- [x] Likelihood Mapping Analysis

		iqtree2 -s [gene.fa] -m TEST -lmap 5000 -n 0 -lmclust [species_clusters.nex]

NB: **species_clusters.nex** comprised four clusters: Acrodonta, Pleurodonta, Serpentes and Others (the remaining species). We removed genes without
at least one rapresentative per cluster. 

- [x] Quick script to create the supports table:

		for file in *.iqtree; do sed -n '/LIKELIHOOD MAPPING STATISTICS/,$ { /Quartet support of areas 1-7 (mainly for clustered analysis):/,$!p }' "$file" > "$(bas>
    		grep "^    5000" *extracted.txt > supported_topologies.txt 

Then, we created a OG list for each topologies supported by at least 30% of quartets.

es. 
|Gene|A1|A2|A3|
|---|---|---|---|
|OG0001|20|40|40|
|OG0002|50|30|20|

- [x] AU test: for each genes, do:
		
		#create custom species tree and mitochondrial tree according to species in the alignment
		phykit prune_tree "$SPECIES_TREE" taxa_present.txt -k -o pruned_trees/species_${GENE}.nwk
		phykit prune_tree "$MITO_TREE" taxa_present.txt -k -o pruned_trees/mito_${GENE}.nwk
    		cat pruned_trees/species_${GENE}.nwk pruned_trees/mito_${GENE}.nwk > pruned_trees/topologies_${GENE}.nwk
		
		iqtree2 -s [gene.fa] -m MFP -z pruned_trees/topologies_[gene].nwk -n 0 -zb 10000 -au -T 10		

### Functional Annotation

Genes those showed DeltaLog-likelihood < 0 (towards mitochondrial tree) and p-value < 0.05 were considered significantly biased 
towards the mitochondrial topology. This set of genes constituted the foreground layer for the GO terms enrichment. 

- [x] We ran InterProScan to annotate each proteins.

                path/to/interproscan.sh -cpu 48 -goterms -pa -i [00_input_interproscan.sh] -appl PANTHER --output-dir [01_output_interproscan]

- [x] We used a custom made R script written by Mirko Martini (@mirkmart) to infer TopGO enrichment, followed by visualization 
on ReViGO. 
