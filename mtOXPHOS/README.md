# Pipeline to annotate mitochondrial genes

1. When available, we downloaded mitogenomes directly from NCBI. 

2. Otherwise, we used Miniprot to identify genomic coordinates and AGAT to extract them and automatically traduction in amino acids.

		miniprot --gff [genome] [mitochondrial proteins of the closest species] > aln.gff
		sed -E 's/Target=([^ ;]+) [^;]*/Name=\1;Target=\1/' aln.gff > new_aln.gff
		agat_sp_extract_sequences.pl -g new_aln.gff -f [genome] -p --table 2 -o mtDNA.fasta

3. In the case of _Notechis scutatus_, we failed in recovering mitogenomes. Thus, we reassemble the whole transcriptome (SRR519122) and search for mitochondrial genes
 using diamond blastx against both _nr_ database and against mitogenome of _Naja naja_.

