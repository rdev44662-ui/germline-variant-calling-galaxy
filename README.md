# Germline Variant Calling and Annotation Using Galaxy

## Project Overview

A browser-based bioinformatics project demonstrating germline variant calling and variant-effect annotation using Galaxy Europe.

The analysis uses paired-end whole-genome sequencing (WGS) reads, the human GRCh38/hg38 reference genome, and an annotation database from SnpEff.

## Workflow

FASTQ reads → FastQC → fastp → BWA-MEM2 → SAMtools → FreeBayes → SnpEff → Annotated VCF

## Tools and Skills

- **FastQC:** sequencing quality assessment
- **fastp:** read filtering and trimming
- **BWA-MEM2:** alignment to the reference genome
- **SAMtools:** BAM processing and alignment statistics
- **FreeBayes:** variant calling
- **SnpEff:** predicted variant-effect annotation
- **Galaxy Europe:** browser-based workflow execution and reproducibility

## Key Results

- Alignment mapping rate: **99.61%**
- Properly paired reads: **98.41%**
- Variants processed by SnpEff: **268,087**
- Predicted variant effects: **2,080,764**
- Reference build: **GRCh38/hg38**

These figures are taken from the Galaxy outputs and SnpEff report. The reported SnpEff warnings require review before drawing biological conclusions.

## Repository Contents

- `README.md` — project overview and key results
- `results/project-results.md` — detailed results and limitations
- Galaxy `.ga` workflow file — exported workflow structure
- `results/high-impact-variant-interpretation.md` — detailed review of selected HIGH-impact variants

## Limitations

Predicted variant effects are not proof of pathogenicity or clinical significance. Further quality filtering, annotation review, and biological interpretation are needed.

## Data and Reproducibility

The analysis was performed using Galaxy Europe in a web browser. Large sequencing and annotated VCF files are not included in this repository.
The SnpEff CSV statistics report is included in the `results` folder, and the HTML statistics report is published through GitHub Pages. The annotated VCF is retained in Galaxy because of its file size.
## View Project Results

- [**Open SnpEff HTML Annotation Report**](https://rdev44662-ui.github.io/germline-variant-calling-galaxy/results/SnpEff_eff__on_dataset_44_-_HTML_stats_html.html)
- [Browse CSV Statistics and Other Results](https://github.com/rdev44662-ui/germline-variant-calling-galaxy/tree/main/results)
- [View Detailed Results and Limitations](https://github.com/rdev44662-ui/germline-variant-calling-galaxy/blob/main/results/project-results.md)
- [View Exported Galaxy Workflow](https://github.com/rdev44662-ui/germline-variant-calling-galaxy/blob/main/results/Galaxy-Workflow-Germline_Variant_Calling.ga)

The SnpEff HTML statistics report is available through GitHub Pages, while the CSV statistics are available in the repository for review and reproducibility. The annotated VCF is retained in Galaxy because of its file size.
## HIGH-Impact Variant Interpretation

The germline variant-calling workflow was followed by SnpEff annotation, VCF filtering, and HIGH-impact variant extraction using SnpSift.

**Detailed report:** [View HIGH-impact Variant Interpretation](results/high-impact-variant-interpretation.md)
## Interpretation Note

SnpEff predicts potential effects of genetic variants. These predictions alone do not establish pathogenicity or clinical significance.
## Author

MSc Biotechnology graduate | Bioinformatics and Genomics Portfolio Project
