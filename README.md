# MPMR_Squamata

### Dataset

Four groups:

+ **Acrodonta**
+ **Pleurodonta**
+ **Serpentes**
+ **Others** (Gekkota, Scincomorpha, Laterata, Anguiformes)

### Steps

 **1 -** define dataset (`dataset.xlxs`) and download both proteomes (*protein.faa) and annotation (*genomic.gff) files.
 
 **2 -** remove putative isoforms with [`primary_transcript.py`](https://github.com/davidemms/OrthoFinder/blob/master/tools/primary_transcript.py) 
 
 **3 -** edit headers (`>proteincode__Namespecies`)
 
 **4 -** run OrthoFinder 

 > orthofinder -f [path/to/dir/containing/proteomes/] -y -X -M msa -t [number of threads available]
 
 Proteins > 100 aa
2. Run OrthoFinder

**First Check BUSCO genes**
    
    for file in *.iqtree; do sed -n '/LIKELIHOOD MAPPING STATISTICS/,$ { /Quartet support of areas 1-7 (mainly for clustered analysis):/,$!p }' "$file" > "$(basename "$file" .iqtree).extracted.txt"; done
    
    grep "^    5000" *extracted.txt > supported_topologies.txt


    







