# MPMR_Squamata

### Dataset

Four groups:

+ **Acrodonta**
+ **Pleurodonta**
+ **Serpentes**
+ **Others** (Gekkota, Scincomorpha, Laterata, Anguiformes)


### Preliminary Steps

**PS1 -** define dataset (`dataset.xlxs`) and download both proteomes (**protein.faa*) and annotation (**genomic.gff*) files.
 
**PS2 -** remove putative isoforms with [`primary_transcript.py`](https://github.com/davidemms/OrthoFinder/blob/master/tools/primary_transcript.py) 
 
**PS3 -** edit headers (`>proteincode__Namespecies`)
 
**PS4 -** run OrthoFinder 

   orthofinder -f [path/to/dir/containing/proteomes/] -y -X -M msa -t [number of threads available]

---

### Path A

Student: `Nicolò Gabriele`

**Path A** concerns analyses about topological tests. 
+ **PA1**
  + densitree
  +  Robinson-Foulds + Multidimensional Scaling

+ **PA2**
  + Gene Concordance Factors (gCFs; IQTREE2)

+ **PA3**
  + Likelihood Mapping Analysis (LMA; IQTREE2)


### Path B

Student: `Giorgia Galletti`

**Path B** concerns analyses about co-variations among proteins. 
+ **PB1**
  + Phylogenomic analyses
  + Gene-tree/Species-tree reconciliation

+ **PB2**
  + ERC analyses
  + Network analyses	

---

Old analysis:

**First Check BUSCO genes**
    
    for file in *.iqtree; do sed -n '/LIKELIHOOD MAPPING STATISTICS/,$ { /Quartet support of areas 1-7 (mainly for clustered analysis):/,$!p }' "$file" > "$(basename "$file" .iqtree).extracted.txt"; done
    
    grep "^    5000" *extracted.txt > supported_topologies.txt


    







