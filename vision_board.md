# Variant Calling Analysis for KRAS

KRAS mutations are a major cause of resistance to anti-EGFR therapy. KRAS activating mutations appear later in the pathogenesis and contribute to permanent activation of KRAS dependent-pathways such as MAPK and PI3K-AKT signaling pathways, involved in the proliferation and survival of tumor cells
- Sequencing the circulating tumor DNA (ctDNA) is a non-invasive method to assess mutation profile
- Changes in VAF in ctDNA significantly correlates with tumor size change.
- Lim et al., 2021 investigates 93 metastatic colorectal cancer (mCRC) patients. One patient is selected for this analysis.


## Patient Selection
- For patient selection SRA Run Table is downloaded from NBCI
- According to the Lim et al.,
    - FFPE: Baseline tumor sample
    - PBMC: Normal control
    - CTC: ctDNA sample
- Selected patient:
  
| Run         | Sample Name  | Library Name | BioSample    |
|-------------|-------------|---------------|--------------|
| SRR13973869 | PBMC_CTC364 | PBMC_76       | SAMN18317964 |
| SRR13973877 | FFPE_CTC364 | colorectal_70 | SAMN18318305 |
| SRR13974119 | CTC364      | plasma_271    | SAMN18318235 |
| SRR13974120 | CTC364-2    | plasma_270    | SAMN18318234 |


## Variant Calling Pipeline
| Step | Tool / Method | Description | Output |
|------|---------------|-------------|--------|
| 1    | FASTQ         | Raw sequencing reads (input files) | `*.fastq.gz` |
| 2    | FastQC        | Quality control of raw reads | `*_fastqc.html` |
| 3    | fastp         | Trimming adapters and low-quality bases | `*_trimmed.fastq.gz`, `*_fastp_report.html` |
| 4    | BWA           | Align trimmed reads to reference genome | `*_sorted.bam` |
| 5    | Samtools      | Sort, index, and mark duplicates | `*_dedup.bam`, `*.bai` |
| 6    | FreeBayes     | Variant calling | `*.vcf` |
