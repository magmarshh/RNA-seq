# RNA-seq analysis pipeline, featureCounts input, edgeR, functional enrichment, visualization

- `Analysis_STAR.Rmd` - RNA-seq analysis pipeline for `STAR` counts. Prerequisites:
    - A path to data folder. This folder should have 3 subfolders:
        - `02_STAR-align` - gzipped count files with `.tab` extension outputted by `STAR` aligner
        - `results` - folder where the results will be stored
        - `data` - Must have `sample_annotation.csv` file, example below

- `Analysis_featurecounts.Rmd` - RNA-seq analysis pipeline for `featureCount` counts. Prerequisites:
    - A path to data folder. This folder should have 3 subfolders:
        - `03_featureCount` - gzipped count files outputted by `featureCount`
        - `results` - folder where the results will be stored
        - `data` - Must have `sample_annotation.csv` file. Annotation file should have "Sample" column with sample names, and any other annotation columns. Include "Group" column containing covariate of interest. Example:
```
# Sample,Group
VLI10_AA_S61_L006_R1_001.txt.gz,AA
VLI10_AA_S61_L007_R1_001.txt.gz,AA
VLI10_AA_S61_L008_R1_001.txt.gz,AA
VLI11_C_S62_L006_R1_001.txt.gz,C
VLI11_C_S62_L007_R1_001.txt.gz,C
VLI11_C_S62_L008_R1_001.txt.gz,C
```

- `calcTPM.R` - a function to calculate TPMs from gene counts

- `Figure_clusterProfiler_nes.Rmd` - Takes the results of edgeR analysis from an Excel file, performs GO and KEGG GSEA and plots the results as horizontal barplots, sorted by normalized enrichment score (NES). [Example](Figure_clusterProfiler.pdf)

- `Figure_clusterProfiler_asis.Rmd` - Takes the results of edgeR analysis from an Excel file, performs GO and KEGG GSEA and plots the results as horizontal barplots, sorted by p-value, as they come out of the enrichment analysis.

- `Figure_heatmap.Rmd` - make heatmap for selected genes. Uses `TMP.xlsx` produced by `Analysis.Rmd` and a custom signature of gene names

- `Pathview.Rmd` - visualization of top KEGG pathways. Uses `DEGs.xlsx` produced by `Analysis.Rmd`

- `utils.R` - helper functions

## `scripts` folder

Scripts for running RNA-seq preprocessing steps on a cluster using PBS job submission system. `subread-featurecounts` scripts are in the [dcaf/ngs.rna-seq](https://github.com/mdozmorov/dcaf/tree/master/ngs.rna-seq) repository

- `submit01_trimgalore.sh` - Adapter trimming using [TrimGalore](https://github.com/FelixKrueger/TrimGalore)
- `submit02_STAR-index.sh` - Index the genome for the [STAR](https://github.com/alexdobin/STAR) aligner
- `submit02_STAR.sh` - Align samples using [STAR](https://github.com/alexdobin/STAR). Requires `input01_toStarAlign.list` text file with the list of input files, each string contains (comma-separated) file name(s), space separates first and second read pairs
- [MultiQC](https://multiqc.info/) commands to summarize QC reports generated by TrimGalore and STAR
```
multiqc --filename multiqc_01_trimmed.html --outdir multiqc_01_trimmed 01_trimmed/
multiqc --filename multiqc_02_STAR-align.html --outdir multiqc_02_STAR-align 02_STAR-align/
```


- `submit05_BAFExtract.sh` - [BAFExtract](https://github.com/akdess/BAFExtract) run for [CaSpER](https://github.com/akdess/CaSpER) pipeline detecting CNVs from RNA-seq data

## Misc

- DESeq results to pathways in 60 Seconds with the fgsea package, https://stephenturner.github.io/deseq-to-fgsea/