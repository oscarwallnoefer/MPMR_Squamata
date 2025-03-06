# MPMR_Squamata

### Dataset

Same-size groups (~8 species per group). 
Groups:

+ **Acrodonta**
+ + *Pogona_vitticeps*
  + *Phrynocephalus_forsythii*
  + *Leudakia_wui*
  + ...
+ **Pleurodonta**
+ + *Phrynosoma_platyrhinos*
  + *Sceloporus_undulatus*
  + *Anolis_sagrei*
  + *Anolis_carolinensis*
+ **Serpentes**
+ + *Crotalus_adamanteus*
  + *Candoia_aspera*
  + *Ahaetulla_prasina*
  + *Naja_naja*
  + *Thamnophis_elegans*

+ **Others**
+ + *Eublepharis_macularius (Gekkota)*
  + *Gekko_japonicus (Gekkota)*
  + *Euleptes_europaea (Gekkota)*
  + *Heteronotia_binoei (Gekkota)*
  + *Lerista_edwardsae (Scincomorpha)*
  + *Tiliqua_scincoides (Scincomorpha)*
  + *Sphaerodactylus_townsendi (Gekkota)*
  + *Varanus_komodoensis (Anguiformes)*
  + *Podarcis_muralis (Lacertidae)*
  + *Podarcis_raffonei (Lacertidae)*
  + *Zootoca_vivipara (Lacertidae)*
  + *Lacerta_agilis (Lacertidae)*
  

### Steps

1. Maintain only proteins > 100 aa
2. Run OrthoFinder

**First Check BUSCO genes**
    
    for file in *.iqtree; do sed -n '/LIKELIHOOD MAPPING STATISTICS/,$ { /Quartet support of areas 1-7 (mainly for clustered analysis):/,$!p }' "$file" > "$(basename "$file" .iqtree).extracted.txt"; done
    
    grep "^    5000" *extracted.txt > supported_topologies.txt


    







