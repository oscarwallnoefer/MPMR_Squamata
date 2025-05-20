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
 
**PS3 -** edit headers (`>species__protein`)
 
**PS4 -** run OrthoFinder 

      orthofinder -f [path/to/dir/containing/proteomes/] -y -X -M msa -t [number of threads available]

---

### --- Path A

Student: `Nicolò Gabriele`

**Path A** Topological tests. 
+ **PA1**
  + gene trees (IQTREE2, -m MFP, -B 1000)
  + + Filters:
    +  a. < 10% gaps in gene alignments
    +  b. > 100 aa gene length
    +  c. pruning species with branch length equal to or 20 times the median length of all branches.
    +  d. removing genes if there is not monophyletic origin in each group (Acrodonta, Pleurodonta or Serpentes).
    +  e. removing genes if there is not > 80% bootstrap support for the deep node of Acrodonta, Pleurodonta and Serpentes.  
  + Robinson-Foulds + Multidimensional Scaling

+ **PA2**
  + Likelihood Mapping Analysis (LMA; IQTREE2)
    + LMA performed for all the XX single-gene alignments.
  + Coalescence 


### --- Path B

Student: `Giorgia Galletti`

**Path B** Co-variations among proteins. 
+ **PB1**
  + Phylogenomic analyses
  + Gene-tree/Species-tree reconciliation

+ **PB2**
  + ERC analyses
  + Network analyses

+ **PB3**
  + InterProScan -goterms
  + g:profiler

--- 
Serpentes mtDNA extractions

    miniprot --gff SpeciesX_Genome.fasta SpeciesY_mtDNA.faa > SpeciesX_aln.gff
  
    sed -E 's/Target=([^ ;]+) [^;]*/Name=\1;Target=\1/' SpeciesX_aln.gff > SpeciesX_named.gff
  
    agat_sp_extract_sequences.pl -g SpeciesX_named.gff -f SpeciesX_Genome.fasta -p --table 2 -o SpeciesX_mt.fasta




---

Old analysis:

**First Check BUSCO genes**
    
    for file in *.iqtree; do sed -n '/LIKELIHOOD MAPPING STATISTICS/,$ { /Quartet support of areas 1-7 (mainly for clustered analysis):/,$!p }' "$file" > "$(basename "$file" .iqtree).extracted.txt"; done
    
    grep "^    5000" *extracted.txt > supported_topologies.txt


    







