# Gut-Microbiome-BLAST-Analysis
An analysis comparing bacterial genera in distal lumen and proximal mucosa human colon samples using BLAST to match sequencing reads against a reference database.

## Description
This project contains the methodology and results from a bioinformatics analysis comparing the bacterial genera present in two distinct regions of the human colon: the distal lumen (DL) and the proximal mucosa (PM). Sequencing reads (SRR files) were aligned to a reference database using BLAST, and the results were processed with Unix command-line tools to find common and unique genera.

**Note: This is an archived project from a previous course or assignment. The original code/script files are not available in this repository; only the documentation of the commands used and the resulting output summaries are provided.**

## Overview of Methodology

### 1. Data Transfer & Setup
Sequencing files (`SRR6288926.fasta`, `SRR6288933.fasta`) and a reference file (`reference.ft`) were transferred to a remote server (Graham of Compute Canada) using `scp`.

### 2. BLAST Database Creation
A BLAST-compatible database was created from the reference sequence file.
```bash
makeblastdb -dbtype nucl -in reference.ft
```

### 3. Sequence Alignment
The sequences from each sample were aligned against the reference database using ```blastn```. The top match for each sequence was recorded.
```bash
# For Distal Lumen sample
blastn -db reference.ft -query SRR6288926.fasta -max_target_seqs 1 -outfmt "6 qseqid sseqid bitscore" -out DL_match.txt

# For Proximal Mucosa sample
blastn -db reference.ft -query SRR6288933.fasta -max_target_seqs 1 -outfmt "6 qseqid sseqid bitscore" -out PM_match.txt
```

### 4. Data Processing
The genera names were extracted from the BLAST results, cleaned, and sorted.

```bash
# Example for PM sample
cut -f2 PM_match.txt | sed -E 's/_/ /g' | sed -E 's/ [0-9]$//g' | sort | uniq > PM_genera_list
```
Lists of genera unique to each sample and common to both were generated using ```sort```, ```uniq```, and ```cat```.

### 5. Results Retrieval
The final result files were copied back to a local machine using ```scp```.

## Key Findings

**Total Sequences:** DL sample had 93,012 sequences; PM sample had 158,374.

**Most Common Genus:** ```Clostridium``` in the DL sample; ```Prevotella_4``` in the PM sample.

**Unique Genera:** 35 genera were unique to the DL sample; 19 were unique to the PM sample.

**Common Genera:** 117 genera were common to both samples.

## File Descriptions

The original file names (e.g., ```DL_match.txt```) have been retained in the documentation for clarity. In a real implementation, more descriptive names are recommended, such as:

```SRR6288926_distal_lumen_blast_results.txt```

```SRR6288933_proximal_mucosa_blast_results.txt```

```distal_lumen_unique_genera.txt```

```proximal_mucosa_unique_genera.txt```

```common_genera_between_samples.txt```

## Limitations & Considerations

The analysis discusses several limitations, including:

**Sampling Bias:** Variations in bacterial communities based on colon location.

**Sequencing Bias:** Potential over-representation of certain bacterial species.

**BLAST Score Threshold:** The inclusion/exclusion of sequences with low BLAST scores, which may represent weak matches, contaminants, or novel species.


