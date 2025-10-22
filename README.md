# 🧬 Bacterial-Evolution-Pipeline-Using-Snakemake
Automated workflow for analysing bacterial evolution using Snakemake. Integrates bioinformatics tools for sequence download, alignment, variant calling and genome annotation. This pipeline is developed to compare ancestral and evolved bacterial strains to identify genetic variations responsible for adaptation.


A comprehensive, production-ready **Snakemake pipeline** for identifying and annotating mutations in evolved bacterial strains compared to ancestral strains. Perfect for **experimental evolution studies**.

## ✨ Key Features

- 🔄 **Fully automated** workflow from SRA to annotated variants
- 🧬 **Custom SnpEff database** built from Prokka annotations
- 🌍 **Universal compatibility** - works with ANY bacterial genome
- 📊 **HTML reports** with visual charts and statistics
- ⚡ **Parallel processing** with configurable threads
- 🎯 **Multi-sample support** via JSON configuration
- 🐍 **Python analysis scripts** included

---

## 🔬 Pipeline Overview

```
SRA Download → FASTQ Conversion → Quality Control (FastQC) →
Trimming (fastp) → Alignment (BWA) → BAM Processing (samtools) →
Duplicate Removal (Picard) → Variant Calling (bcftools) →
Variant Comparison → Prokka Annotation → Custom SnpEff DB → 
Functional Annotation → HTML Reports
```

**Identifies**: Mutations unique to evolved strains (de novo mutations from experimental evolution)

**Output**: Annotated VCF files with gene names, amino acid changes, and functional impact predictions

---

## 🚀 Quick Start

### 1. Clone the Repository
```bash
git clone https://github.com/kala02/Bacterial-Evolution-Pipeline-Using-Snakemake.git
cd Bacterial-Evolution-Pipeline
```

### 2. Install Dependencies
## 🧰 Conda Environments

This pipeline uses separate conda environments for each step, defined in the `envs/` folder.
Each rule in the `Snakefile` automatically activates the corresponding environment.  

 
### 3. Configure Your Samples
Edit `config.json`:
```json
{
  "samples": [
    {
      "name": "Sample1",
      "ancestor_sra": "ERR13909728",
      "evolved_sra": "ERR13909733"
    }
  ],
  "reference": {
    "url": "https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/005/845/GCF_000005845.2_ASM584v2/GCF_000005845.2_ASM584v2_genomic.fna.gz",
    "name": "K12_reference.fna"
  },
  "threads": {
    "bwa": 4,
    "bcftools": 2,
    "prokka": 4
  }
}
```

### 4. Run the Pipeline
```bash
# Dry run (see what will execute)
snakemake -n --use-conda     

# Snakemake execution
snakemake --cores 4 --use-conda

#If on MAC M1/M2/M3 
CONDA_SUBDIR=osx-64 snakemake --cores 4 --use-conda
```

### 5. Analyze Results
```bash
4_annotation/Sample1_Evolved_snpEff_summary.html

4_annotation/Sample1_Evolved_annotated.vcf
```

---

## 📁 Repository Structure

```
Bacterial-Evolution-Pipeline/
├── Snakefile                  # Main pipeline (26 rules, 575 lines)
├── config.json                # Sample configuration         
├── envs/                      # Conda dependencies
│   ├── bcftools.yaml          
│   ├── bwa.yaml            
│   ├── fastp.yaml   
│   ├── fastqc.yaml       
│   ├── picard.yaml   
│   └── prokka.yaml
|   |── samtools.yaml    
│   └── snpEff.yaml
│   └── sra-tools.yaml           
│
├── .gitignore                 # Git ignore patterns
├── LICENSE                    # MIT License
└── README.md                  # This file
```

---

## 📊 Pipeline Stages

| Stage | Tool | Description | Output |
|-------|------|-------------|--------|
| 1 | prefetch | Download SRA data | `.sra` files |
| 2 | fasterq-dump | Convert to FASTQ | `.fastq` files |
| 3 | FastQC | Quality control | HTML reports |
| 4 | fastp | Trim & filter reads | `.trimmed.fastq` |
| 5 | wget | Download reference | `.fna.gz` |
| 6 | bwa index | Index reference | BWA index files |
| 7 | bwa mem | Align reads | `.sam` files |
| 8 | samtools | Sort & index BAM | `.sorted.bam` |
| 9 | Picard | Remove duplicates | `.dedup.bam` |
| 10 | bcftools | Call variants | `.vcf.gz` |
| 11 | bcftools isec | Compare variants | Evolved-only VCF |
| 12 | Prokka | Annotate reference | `.gff`, `.faa`, `.ffn` |
| 13 | SnpEff build | Build custom DB | SnpEff database |
| 14 | SnpEff annotate | Annotate variants | `.vcf` + HTML |

---

## 🛠️ Requirements

### Software Dependencies
- Python ≥ 3.8
- Snakemake ≥ 7.32
- SRA Toolkit ≥ 3.0
- FastQC ≥ 0.11
- fastp ≥ 0.23
- BWA ≥ 0.7.17
- samtools ≥ 1.17
- bcftools ≥ 1.17
- Picard ≥ 3.0
- Prokka ≥ 1.14
- SnpEff ≥ 5.1

All dependencies are automatically installed via Conda (see `envs`)

### System Requirements
- **CPU**: 4+ cores recommended
- **RAM**: 8+ GB recommended
- **Storage**: 50-100 GB per sample (depends on coverage)
- **OS**: Linux, macOS

---

## 📄 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

---

