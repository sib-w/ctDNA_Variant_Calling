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



## KRAS Variant Analysis
- VCF files were loaded into the Google Colab environment and filtered. Filtering criteria applied were: **alternate allele reads (AO) ≥ 10** and **variant allele frequency (VAF/AD) ≥ 1%**.  
- The concentration of cfDNA in blood plasma is low, which is why a relatively low VAF threshold was used.
- Following variant filtering, only variants located within the **KRAS gene region** (hg38: chr12:25,209,795–25,245,384) were retained for downstream analysis.
  
[KRAS Gene in UCSC Genome Browser (hg38)](https://genome.ucsc.edu/cgi-bin/hgGene?hgg_gene=ENST00000311936.8&hgg_chrom=chr12&hgg_start=25205245&hgg_end=25250929&hgg_type=knownGene&db=hg38)

- Although the run table does not explicitly indicate treatment timing, for this analysis I assumed that **CTC364** corresponds to pre-treatment and **CTC364-2** corresponds to during/post-treatment.  
- To identify differentiating variants, the filtered **KRAS variants** from the pre-treatment sample were compared to the post-treatment sample, and variants present in pre-treatment but absent in post-treatment were selected.
- Differentiating variants are listed below.

| Chromosome | Position | REF                  | ALT                  | REF_READS | ALT_READS | VAF     |
|------------|----------|----------------------|----------------------|-----------|-----------|---------|
| chr12      | 25215659 | ATTTTTTTTTTTTTAAAC   | ATTTTTTTTTTTTAAAC    | 110       | 57        | 34.13%  |
| chr12      | 25250357 | A                    | C                    | 277       | 17        | 5.78%   |

## Interpretation of KRAS Variants
- The variant at **chr12:25215659** shows a high VAF (34.13%), indicating a strong presence in pre-treatment ctDNA.  
- The variant at **chr12:25250357** has a lower VAF (5.78%), consistent with subclonal or low-frequency mutation.  
- Variants present in pre-treatment but absent in post-treatment could indicate response to therapy or clonal evolution.  

## Vision / Marketing Ideas

- **Highlight non-invasive monitoring:** ctDNA sequencing allows tumor mutation profiling through a simple blood draw, reducing the need for invasive biopsies.  

- **Show clinical relevance:** Variants in KRAS detected via ctDNA can indicate treatment resistance (anti-EGFR therapy), helping clinicians adapt therapy in real-time.  

- **Tracking treatment response:** Changes in variant allele frequency (VAF) and number of variants over time reflect tumor burden and response to therapy. The results of this analysis shows decreased number of variants after treatment.

| Sample VCF File                     | Number of Variants |
|-------------------------------------|--------------------|
| SRR13974119_freebayes.vcf           | 324,425            |
| SRR13974119_freebayes_filtered.vcf  | 7,509              |
| SRR13974120_freebayes.vcf           | 202,194            |
| SRR13974120_freebayes_filtered.vcf  | 6,746              |


- **Call-out for future applications:** ctDNA sequencing could enable early detection of resistance mutations, longitudinal patient monitoring, and personalized therapy decisions.

## REFERENCES
- Lim, Y., Kim, S., Kang, JK. et al. Circulating tumor DNA sequencing in colorectal cancer patients treated with first-line chemotherapy with anti-EGFR. Sci Rep 11, 16333 (2021). https://doi.org/10.1038/s41598-021-95345-4
- Saleh K, Ibrahim R, Khoury R, Tikriti Z, Khalife N. KRAS and EGFR inhibitors: a new step in the management of colorectal cancer. Transl Gastroenterol Hepatol. 2024 Sep 4;9:56. doi: 10.21037/tgh-24-73. PMID: 39503022; PMCID: PMC11535806.


