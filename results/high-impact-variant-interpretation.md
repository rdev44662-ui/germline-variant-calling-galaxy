# High-Impact Germline Variant Extraction and Preliminary Interpretation

## 1. Project Overview

This project documents the extraction and preliminary computational interpretation of high-impact genetic variants identified through a germline variant-calling workflow implemented using Galaxy Europe.

The analysis used human whole-genome sequencing (WGS) data from the Sequence Read Archive (SRA), accession **SRR41315477**. Following read quality control, alignment, variant calling, and functional annotation, variants were filtered to identify records containing predicted high-impact consequences.

The analysis used the human reference genome **GRCh38/hg38**.

The objective was to identify potentially disruptive genetic variants, describe their annotations, and review available sequencing evidence. These findings are computational and preliminary; they are not clinical classifications.

## 2. Study Objectives

- Identify high-impact variants from the filtered germline VCF.
- Extract records annotated with HIGH functional impact by SnpEff.
- Summarize the distribution of variant types.
- Examine gene-level and transcript-level annotations.
- Review available sequencing indicators, including read depth, genotype, allele balance, mapping quality, and strand distribution.
- Identify records that warrant further technical investigation.
- Document the workflow and findings in a reproducible portfolio format.

## 3. Dataset and Reference Genome

| Parameter | Description |
|---|---|
| Data type | Whole-genome sequencing |
| Organism | *Homo sapiens* |
| SRA accession | SRR41315477 |
| Sequencing strategy | WGS |
| Library layout | Paired-end |
| Reference genome | GRCh38/hg38 |
| Analysis platform | Galaxy Europe |
| Variant caller | FreeBayes |
| Functional annotation | SnpEff |
| Variant filtering | VCFfilter and SnpSift |
| Variant statistics | BCFtools |

## 4. Bioinformatics Workflow

The analysis followed a sequential germline variant-calling and annotation workflow:

1. Raw paired-end FASTQ data acquisition.
2. Read quality assessment using FastQC.
3. Read preprocessing using fastp.
4. Alignment to GRCh38 using BWA-MEM2.
5. BAM processing and coordinate sorting using SAMtools.
6. Alignment statistics using SAMtools flagstat.
7. Germline variant calling using FreeBayes.
8. Functional annotation using SnpEff.
9. Variant filtering using VCFfilter.
10. Variant statistics using BCFtools.
11. HIGH-impact annotation extraction using SnpSift.
12. Preliminary review of selected variants.

### 4.1 Read Alignment and Mapping Statistics

Sequencing reads were aligned against the GRCh38 reference genome using BWA-MEM2. Alignment statistics were evaluated using SAMtools flagstat.

| Alignment metric | Result |
|---|---:|
| Total reads | 31,359,409 |
| Primary reads | 31,091,692 |
| Mapped reads | 31,235,817 |
| Mapping percentage | 99.61% |
| Primary mapped percentage | 99.60% |
| Properly paired reads | 30,597,888 |
| Properly paired percentage | 98.41% |
| Singletons | 7,114 |

The mapping percentage of 99.61% indicates that most reads aligned to the selected reference genome. The properly paired percentage was 98.41%. These alignment metrics support successful mapping but do not independently establish the accuracy of individual variant calls.

## 5. Functional Annotation Using SnpEff

The FreeBayes-generated VCF was annotated using **SnpEff version 5.4c**, with the GRCh38.115 annotation database.

SnpEff assigns predicted functional consequences to variants based on their genomic locations and transcript annotations.

### 5.1 Annotation Summary

| Annotation metric | Result |
|---|---:|
| Input lines | 265,728 |
| Variants before annotation | 268,097 |
| Variants processed | 268,087 |
| Known variant IDs | 0 |
| Warnings | 88,920 |
| Errors | 9,528 |
| Total predicted effects | 2,080,764 |

The annotation process reported more than two million predicted effects. A single genomic variant can affect multiple transcripts and therefore receive multiple functional annotations.

The reported warnings and errors are important quality considerations. The annotation log should be reviewed before treating the annotation output as fully validated.

### 5.2 Distribution of Variant Types

| Variant type | Count |
|---|---:|
| SNP | 218,822 |
| MNP | 9,940 |
| Insertions | 15,939 |
| Deletions | 20,219 |
| Mixed variants | 3,167 |

Single-nucleotide polymorphisms represented the largest category in the SnpEff summary.

### 5.3 Distribution of Functional Impact

| Impact category | Count |
|---|---:|
| HIGH | 2,239 |
| MODERATE | 18,206 |
| LOW | 19,643 |
| MODIFIER | 2,031,148 |

The HIGH category includes predicted consequences that may substantially disrupt gene function, such as frameshift, stop-gained, and essential splice-site alterations.

**A HIGH annotation is a predicted molecular consequence, not proof that a variant is clinically pathogenic.**

### 5.4 Functional Classification

| Functional class | Count |
|---|---:|
| Missense | 16,737 |
| Nonsense | 949 |
| Silent | 12,168 |

These classifications provide additional information about predicted effects on protein-coding sequences.

## 6. Variant Filtering

### 6.1 VCFfilter

VCFfilter was applied using the INFO-field expression:

```text
DP > 10
```

This criterion retained variants with a reported read depth greater than 10.

| Parameter | Result |
|---|---:|
| Original VCF records | 6,166 |
| Filtered VCF records | 5,486 |
| Records removed | 680 |
| Percentage removed | 11.03% |
| Percentage retained | 88.97% |

The filtering step reduced the record count from 6,166 to 5,486. The retained VCF was used for downstream statistical analysis and HIGH-impact extraction.

**Filtering limitation:** A depth threshold alone does not establish variant reliability. Allele balance, mapping quality, strand balance, local sequence context, and other evidence should also be considered.

## 7. BCFtools Statistical Comparison

BCFtools stats was used to compare the original VCF with the filtered VCF.

### 7.1 Original and Filtered VCF Comparison

| Variant category | Original VCF | Filtered VCF |
|---|---:|---:|
| Total records | 6,166 | 5,486 |
| Sites without ALT alleles | 0 | 0 |
| SNPs | 4,822 | 4,282 |
| MNPs | 456 | 396 |
| Indels | 967 | 881 |
| Other variants | 111 | 102 |
| Multiallelic sites | 348 | 324 |
| Multiallelic SNP sites | 26 | 25 |

The filtering procedure retained approximately 88.97% of the original VCF records. The reductions across the reported variant categories reflect the effect of the read-depth criterion.

## 8. Extraction of HIGH-Impact Variants

The filtered VCF was processed using **SnpSift Filter version 5.4.0c+galaxy0**.

The objective was to extract VCF records containing at least one annotation with HIGH predicted impact in the SnpEff ANN field.

### 8.1 Filtering Configuration

| Parameter | Value |
|---|---|
| Input | Dataset 77: VCFfilter output |
| Tool | SnpSift Filter |
| Version | 5.4.0c+galaxy0 |
| Input format | VCF |
| Expression type | Simple expression |
| Filtering expression | `ANN[*].IMPACT has 'HIGH'` |
| Invert filter | Disabled |
| Output | Dataset 83 |

The expression selected records containing a HIGH-impact annotation.

### 8.2 Extraction Results

The extracted HIGH-impact VCF was evaluated using BCFtools stats.

| Metric | Result |
|---|---:|
| Total HIGH-impact VCF records | **27** |
| Samples | 1 |
| Sites without ALT alleles | 0 |
| SNPs | 14 |
| MNPs | 1 |
| Indels | 12 |
| Other variants | 1 |
| Multiallelic sites | 1 |
| Multiallelic SNP sites | 0 |

The extraction produced **27 VCF records** containing at least one HIGH-impact annotation.

The BCFtools category counts are reproduced as reported by the analysis. Because the listed category counts total 28 while the record count is 27, these categories should not be summed or treated as a mutually exclusive partition without checking the original BCFtools stats output and its definitions.

## 9. Preliminary Interpretation of Selected HIGH-Impact Variants

Eleven distinct genomic records were examined from the extracted HIGH-impact results. The review considered the affected gene or annotated transcript, predicted consequence, genotype, sequencing depth, allele balance, mapping quality, and other available read-level statistics.

### 9.1 Summary of Distinct Records Reviewed

| Chromosome and position | Gene / annotation | Predicted HIGH consequence | Preliminary observation |
|---|---|---|---|
| chr1:119538653 | HSD3BP3 pseudogene | Splice donor variant | Low-depth heterozygous record; pseudogene annotation requires caution |
| chr10:87864103 | PTEN | Splice acceptor / donor variants | High depth and homozygous alternate genotype; read-level validation required |
| chr10:87960876 | PTEN | Splice acceptor variant | Heterozygous insertion in a repetitive T-rich region |
| chr11:94492641 | MRE11 | Splice donor variant | Heterozygous record with notable strand imbalance |
| chr11:108250683 | ATM | Splice acceptor variant | Insertion in a T-rich region with allele balance requiring investigation |
| chr12:865189 | WNK1 | Frameshift variant | c.2220dupC; predicted p.Phe741fs; read depth 26 |
| chr12:38206042 | ENSG00000297967 | Splice donor variant | Pseudogene-associated annotation; low-depth heterozygous record |
| chr12:47879112 | VDR | Start-lost variant | c.2T>C; predicted alteration of the translation initiation codon |
| chr12:68820911 | MDM2 | Splice donor variant | c.232+1G>A; heterozygous record with allele balance below 0.5 |
| chr13:18211836 | ENSG00000303053 pseudogene | Splice donor variant | Low mapping-quality values and pseudogene annotation raise mapping concerns |
| chr13:40920109 | SUGT1P3 pseudogene | Splice donor variant | n.1543+2T>G; pseudogene-associated transcript annotation |

### 9.2 Detailed Observations

#### A. PTEN — chr10:87864103

- Predicted consequence: HIGH splice-site alteration.
- Genotype: 1/1.
- Read depth: 2,140.
- Alternate observations: 2,128.
- Reference observations: 0.
- QUAL: 59,983.7.
- SnpEff LOF annotation was present.

This record showed strong alternate-allele representation at high read depth. However, the reported reference mapping-quality statistic was zero, which warrants investigation. The alignment should be inspected to assess read placement, local alignment quality, and possible technical artefacts.

#### B. PTEN — chr10:87960876

- Predicted consequence: HIGH splice-acceptor alteration.
- Genotype: 0/1.
- Read depth: 1,268.
- Reference observations: 230.
- Alternate observations: 529.
- QUAL: 9,580.71.
- Annotation: c.802-3dupT.

The variant occurs in a repetitive T-rich sequence context. Such regions can be challenging for short-read alignment and indel calling. Despite substantial read support, the local sequence context should be reviewed.

#### C. MRE11 — chr11:94492641

- Predicted consequence: HIGH splice-donor alteration.
- Genotype: 0/1.
- Read depth: 763.
- Reference observations: 392.
- Alternate observations: 370.
- QUAL: 8,429.76.
- Transcript annotation: c.29+1G>A.

The allele representation was approximately balanced. However, the available strand statistics showed a notable imbalance between alternate reads on the forward and reverse strands. Strand-specific read inspection is recommended.

#### D. ATM — chr11:108250683

- Predicted consequence: HIGH splice-acceptor alteration.
- Genotype: 0/1.
- Read depth: 2,165.
- Reference observations: 759.
- Alternate observations: 541.
- QUAL: 10,323.4.
- Transcript annotation: c.1236-3dupT.
- Reported allele balance: approximately 0.25.

The insertion occurs in a repetitive T-rich region. The observed allele balance is lower than the approximate 0.5 expected for a conventional heterozygous call in a diploid region. Possible explanations include mapping difficulty, local sequence complexity, biological variation in allele representation, or technical bias. Further inspection is required.

#### E. WNK1 — chr12:865189

- Predicted consequence: HIGH frameshift.
- Genotype: 1/1.
- Read depth: 26.
- QUAL: 775.688.
- Transcript annotation: c.2220dupC.
- Predicted protein consequence: p.Phe741fs.

The annotation predicts a frameshift alteration that may disrupt the downstream protein sequence. The relatively low read depth means that the supporting reads should be reviewed before accepting the call.

#### F. VDR — chr12:47879112

- Predicted consequence: HIGH start-lost.
- Genotype: 1/1.
- Read depth: 23.
- QUAL: 679.858.
- Transcript annotation: c.2T>C.
- Predicted protein consequence: p.Met1 alteration.

This variant was annotated at the translation initiation codon. A start-lost consequence may affect normal protein translation. The relatively low read depth means that the call requires additional technical validation.

#### G. MDM2 — chr12:68820911

- Predicted consequence: HIGH splice-donor alteration.
- Genotype: 0/1.
- Read depth: 1,932.
- QUAL: 15,545.5.
- Transcript annotation: c.232+1G>A.
- Reported allele balance: approximately 0.35.

The variant was supported by substantial sequencing depth. However, the alternate allele fraction was below the approximate 0.5 expected for a typical heterozygous diploid call. Further investigation of the alignment and local genomic context is recommended.

### 9.3 Pseudogene-Associated Annotations

Four reviewed records were associated with pseudogene annotations:

- HSD3BP3 — chr1:119538653.
- ENSG00000297967 — chr12:38206042.
- ENSG00000303053 — chr13:18211836.
- SUGT1P3 — chr13:40920109.

These records require caution because pseudogene regions may share sequence similarity with functional genes. This can lead to ambiguous read alignment and potentially unreliable variant calls.

The chr13:18211836 record was particularly notable because of its relatively low reported mapping-quality values. Pseudogene-associated variants require examination of read placement, mapping uniqueness, and transcript annotation.

## 10. Quality Assessment and Interpretation Limitations

The extracted records represent computational predictions rather than confirmed biological or clinical findings.

### 10.1 Read Depth

Read depth indicates the number of sequencing reads covering a genomic position. High depth can provide stronger technical support, but it does not guarantee that a variant is genuine.

### 10.2 Allele Balance

For a conventional heterozygous variant in a diploid region, the alternate allele fraction is often expected to be approximately 0.5. Deviations may arise from mapping bias, local sequence complexity, copy-number variation, allelic imbalance, or technical artefacts.

### 10.3 Mapping Quality

Mapping quality describes confidence in the genomic placement of reads. Low mapping quality can indicate ambiguous alignment, particularly in repetitive regions and pseudogene-associated sequences.

### 10.4 Strand Bias

A substantial difference in the number of alternate reads aligned to the forward and reverse strands may indicate technical bias. Such records should be examined in an alignment viewer.

### 10.5 Repetitive Regions

Variants in homopolymer or repetitive regions can be challenging to call accurately. The PTEN and ATM insertion annotations reviewed here are examples where local sequence context deserves additional attention.

### 10.6 Functional Impact Is Not Clinical Pathogenicity

SnpEff classifies variants according to predicted molecular consequences. It does not independently establish whether a variant causes disease.

Clinical classification requires additional evidence, including:

- Population allele frequencies.
- ClinVar or other curated clinical evidence.
- Relevant disease-gene associations.
- Inheritance patterns and phenotype correlation.
- Established ACMG/AMP classification criteria.
- Orthogonal experimental validation where appropriate.

No disease-causing conclusion is made from this analysis.

## 11. Overall Results

The workflow processed paired-end whole-genome sequencing data and generated annotated and filtered variant datasets.

The principal findings were:

1. **Mapping performance:** 99.61% of reads mapped to the GRCh38 reference genome.
2. **Paired-end alignment:** 98.41% of reads were reported as properly paired.
3. **Functional annotation:** SnpEff processed 268,087 variants and reported 2,080,764 predicted effects.
4. **Annotation impact summary:** 2,239 effects were classified as HIGH impact in the SnpEff summary.
5. **Variant filtering:** VCFfilter reduced the record count from 6,166 to 5,486 using the `DP > 10` criterion.
6. **HIGH-impact extraction:** SnpSift identified 27 VCF records containing at least one HIGH-impact annotation.
7. **Statistical characterization:** BCFtools reported 14 SNPs, 1 MNP, 12 indels, and 1 other variant category in the extracted output. The category totals should be checked against the original stats output because they sum to 28 while the reported record count is 27.
8. **Preliminary review:** Eleven distinct genomic records were examined for annotation, genotype, read depth, and available quality indicators.

The results demonstrate the practical application of a Galaxy-based germline variant analysis pipeline, from raw sequencing data to functional annotation, filtering, and preliminary variant interpretation.

## 12. Conclusion

This project demonstrates an end-to-end computational workflow for germline variant analysis using publicly available human whole-genome sequencing data.

The combination of BWA-MEM2, SAMtools, FreeBayes, SnpEff, VCFfilter, SnpSift, and BCFtools enabled read alignment, variant detection, functional annotation, filtering, and statistical characterization.

The extraction of 27 HIGH-impact VCF records provided a focused dataset for further examination. Preliminary review highlighted records annotated in genes such as PTEN, MRE11, ATM, WNK1, VDR, and MDM2, alongside several pseudogene-associated annotations.

The findings illustrate why variant interpretation must extend beyond functional impact classification. Read depth, allele balance, mapping quality, strand distribution, and genomic sequence context are important considerations when evaluating candidate variants.

Overall, this project strengthened practical understanding of germline variant-calling workflows, VCF manipulation, functional annotation, and preliminary variant-quality assessment using a reproducible browser-based bioinformatics environment.

## 13. Tools and Technologies

| Tool | Application |
|---|---|
| Galaxy Europe | Browser-based bioinformatics workflow execution |
| FastQC | Raw sequencing quality assessment |
| fastp | Read preprocessing and quality filtering |
| BWA-MEM2 | Reference genome alignment |
| SAMtools | BAM processing and alignment statistics |
| FreeBayes | Germline variant calling |
| SnpEff | Functional annotation |
| VCFfilter | Read-depth-based variant filtering |
| SnpSift | HIGH-impact annotation extraction |
| BCFtools | VCF statistics and comparison |
| GitHub | Project documentation and portfolio publication |

## 14. Reproducibility and Future Work

The workflow was implemented using Galaxy Europe, allowing the analysis to be performed through a browser-based environment without requiring local installation of the major bioinformatics tools.

The following steps would strengthen the project further:

1. Inspect candidate variants using an interactive genome browser such as IGV.
2. Normalize and validate indel representations before comparing records.
3. Annotate population frequencies using appropriate population databases.
4. Review curated clinical evidence where available.
5. Investigate pseudogene-associated mapping ambiguity.
6. Evaluate candidate variants using established clinical interpretation frameworks when relevant evidence is available.
7. Export the final workflow and document tool versions and parameters for reproducibility.

## 15. References and Resources

- Galaxy Project: https://usegalaxy.eu/
- Galaxy Training Network: https://training.galaxyproject.org/
- NCBI Sequence Read Archive: https://www.ncbi.nlm.nih.gov/sra
- Ensembl: https://www.ensembl.org/
- SnpEff documentation: https://pcingola.github.io/SnpEff/
- SnpSift documentation: https://pcingola.github.io/SnpEff/ss_introduction/
- SAMtools documentation: https://www.htslib.org/
- BCFtools documentation: https://samtools.github.io/bcftools/
- ClinVar: https://www.ncbi.nlm.nih.gov/clinvar/
- gnomAD: https://gnomad.broadinstitute.org/
- IGV: https://igv.org/

---

**Project status:** Computational workflow completed; preliminary HIGH-impact variant review completed for 11 distinct records.

**Interpretation status:** Research-level computational findings; candidate variants have not been clinically validated.

**Data source:** SRA accession SRR41315477.

**Reference assembly:** GRCh38/hg38.
