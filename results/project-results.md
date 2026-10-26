# Germline Variant Calling and Annotation

## Project Overview

This project demonstrates a browser-based germline variant-calling workflow using Galaxy Europe. The analysis uses paired-end whole-genome sequencing (WGS) reads and a human reference genome.

## Tools Used

- Galaxy Europe
- FastQC — sequencing read quality assessment
- fastp — read quality filtering and trimming
- BWA-MEM2 — reference alignment
- SAMtools — BAM processing and alignment statistics
- FreeBayes — variant calling
- SnpEff — variant-effect annotation

## Reference Genome

- Reference build: GRCh38 / hg38
- SnpEff annotation database: GRCh38.115

## Alignment Results

According to the Galaxy SAMtools flagstat output:

- Total reads: 31,359,409
- Primary reads: 31,091,692
- Mapped reads: 31,235,817
- Mapping rate: 99.61%
- Primary mapping rate: 99.60%
- Properly paired reads: 30,597,888
- Properly paired rate: 98.41%
- Singletons: 7,114 (0.02%)

## Variant Annotation Results

According to the SnpEff statistics report:

- Variants processed: 268,087
- Predicted effects: 2,080,764
- SnpEff version: 5.4c
- Annotation database: GRCh38.115
- Report date: 22 October 2026
- Reported warnings: 88,920

The report lists zero variants with non-empty IDs. This means the input variants had no IDs recorded in that field; it does not establish that all variants are novel.

## Interpretation and Limitations

The reported alignment statistics indicate a high mapping rate for this dataset. SnpEff generated predicted variant-effect annotations, but the reported warnings should be reviewed before drawing biological conclusions.

Variant-effect predictions alone do not establish pathogenicity or clinical significance. Further filtering, annotation review, and biological interpretation are required.

## Reproducibility

The analysis was performed in Galaxy Europe using browser-accessible tools. The Galaxy history and exported workflow should be retained to document the analysis steps and tool settings.

## Data Availability

The annotated VCF and intermediate sequencing files remain in Galaxy. Large genomic data files are not included in this repository.
