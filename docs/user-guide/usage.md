## Module
### - reffa
NGS reference genome usually were [FASTA](https://en.wikipedia.org/wiki/FASTA_format) and `reffa` module mainly focused on FASTA format GENOME file.

`ReffaFile` were a Python class of FASTA format GENOME file, which can be used to run `generate_dict` and `index` by BWA/STAR/Bowtie/Bowtie2.

```python
from iseq.reffa import ReffaFile
from iseq.utils import get_config
config_dict = get_config("/path/config.cfg")
reffa = ReffaFile("/path/hg19.fa", "hg19")
reffa.generate_dict(config_dict)
reffa.bwa_index(config_dict)
reffa.star_index(config_dict)
reffa.bowtie_index(config_dict)
reffa.bowtie2_index(config_dict)
```

### - fastq
NGS sequencing raw data format were [FASTQ](https://en.wikipedia.org/wiki/FASTQ_format) and `fastq` module mainly focused on FASTQ format raw sequencing file.

`FastqFile` were a python class of FASTQ format raw sequencing file, which can be used to run `mapping` using BWA/STAR/Bowtie/Bowtie2/Tophat/Tophat2

```python
from iseq.fastq import FastqFile
from iseq.utils import get_config
config_dict = get_config("/path/config.cfg")
fq_1 = FastqFile("/path/read_1.fq.gz", sampleid = "example")
fq_2 = "/path/read_2.fq.gz"
fq_1.bwa_mapping(config_dict, "/path/outdir", fq_2)
fq_1.star_mapping(config_dict, "/path/outdir", fq_2)
fq_1.bowtie2_mapping(config_dict, "/path/outdir", fq_2)
fq_1.bowtie_mapping(config_dict, "/path/outdir", fq_2)
fq_1.tophat_mapping(config_dict, "/path/outdir", fq_2)
```

### - sam
NGS sequencing format were [SAM](https://en.wikipedia.org/wiki/SAM_(file_format)) after map to FASTA format GENOME without convert to BAM and `sam` module mainly focused on SAM format sequencing file.

`SamFile` were a python class of SAM format sequencing file, which can be used to run `convert2bam` using [samtools](https://github.com/samtools/samtools)

```python
from iseq.sam import SamFile
from iseq.utils import get_config
config_dict = get_config("/path/config.cfg")
sam = BamFile("/path/example.sam", sampleid = "example")
sam.convert2bam(config_dict, "/path/outdir/out.bam")
```

### - bam
NGS sequencing format were [BAM](https://en.wikipedia.org/wiki/SAMtools) after map to FASTA format GENOME and `bam` module mainly focused on BAM format sequencing file.

`BamFile` were a python class of BAM format sequencing file, which can be used to run `index/mpileup` using [samtools](https://github.com/samtools/samtools), `contig_reorder/add_read_group/mark_duplicates` using [Picard](https://github.com/broadinstitute/picard), `realigner_target_creator/indel_realigner/recalibration/print_reads/split_ntrim` using GATK for pre-process step, `/haplotype_caller/unifiedgenotyper_caller/mutect_caller/varscan_caller/torrent_caller/lofreq_caller/pindel_caller/freebayes_caller` for variants discover step.

```python
from iseq.bam import BamFile
from iseq.utils import get_config
config_dict = get_config("/path/config.cfg")
bam = BamFile("/path/example.bam", sampleid = "example")
bam.index(config_dict)
bam.mpileup(config_dict, "/path/outdir/out.mpileup")
bam.contig_reorder(config_dict, "/path/outdir/out.reorder_contig.bam")
...
bam.haplotype_caller(config_dict, "/path/outdir/")
bam.varscan_caller(config_dict, "/path/outdir/")
...
bam.pindel_caller(config_dict, "/path/outdir/")
```

### - vcf
NGS standard Variant Call Format were [VCF](https://en.wikipedia.org/wiki/Variant_Call_Format) and `vcf` module mainly focused on VCF format file.

`VcfFile` were a python class of VCF format file, which can be used to run `fsqd_filter/control_filter/unifiedgenotyper_filter/snpfilter/annovar/merge/varscan2gatkfmt/select_snp/select_indel` using GATK and other tools

```python
from iseq.vcf import VcfFile
from iseq.utils import get_config
config_dict = get_config("/path/config.cfg")
case_vcf = VcfFile("/path/example.case.vcf", sampleid = "example")
control_vcf = "/path/example.control.vcf", sampleid = "example"
case_vcf.fsqd_filter(config_dict, "/path/outdir/out.vcf") # GATK VariantFiltration (FS>;QD<)
case_vcf.control_filter(config_dict, control_vcf, "/path/outdir/out.vcf")
case_vcf.snpfilter(config_dict, "/path/outdir/out.vcf") # Filter all position including 'rs' flag
case_vcf.annovar(config_dict, "/path/outdir/") # Using ANNOVAR to annotation variants record
...
```

### - csv / mpileup / result

`csv` module for ANNOVAR CSV foramt output; `mpileup` module for `samtools mpileup` output format; `result` for user friendly format result file which can be used to publish or report. Which mainly including `CsvFile / MpileupFile/ ResultFile` class.

- Compute variants depth and variant alle depth.
- Using samtools to mpileup that position
- Others


## Pipeline

**Without paired normal control sample**

```bash
# fastq2vcf mode
panel -c config.cfg \
         -s A01A \
         -m fastq2vcf \
         -1 A01A_1.fq.gz \
         -2 A01A_2.fq.gz \
         --bamprocess 00101111 \
         -o outdir

# fastq2bam
panel -c config.cfg \
         -s A01A \
         -m fastq2bam \
         -1 A01A_1.fq.gz \
         -2 A01A_2.fq.gz \
         --bamprocess 00101111 \
         -o outdir

# bam2vcf
panel -c config.cfg \
         -s A01A \
         -m bam2vcf \
         --in_bam A01A.bam \
         --bamprocess 00000000 \
         -o outdir

# genomeindex mode
panel -c config.cfg -m genomeindex
```

**With paired normal control sample**

```bash
# fastq2vcf mode
panel_somatic -c config.cfg \
                 -s A01 \
                 -m fastq2vcf \
                 -1 A01A_1.fq.gz \
                 -2 A01A_2.fq.gz \
                 -3 A01C_1.fq.gz \
                 -4 A01C_2.fq.gz \
                 --bamprocess 00101111 \
                 -o outdir

# fastq2bam mode
panel_somatic -c config.cfg \
                 -s A01 \
                 -m fastq2bam \
                 -1 A01A_1.fq.gz \
                 -2 A01A_2.fq.gz \
                 -3 A01C_1.fq.gz \
                 -4 A01C_2.fq.gz \
                 --bamprocess 00101111 \
                 -o outdir

# bam2vcf mode
panel_somatic -c config.cfg \
                 -s A01 \
                 -m bam2vcf \
                 --case_in_bam A01A.bam \
                 --control_in_bam A01C.bam \
                 --bamprocess 00000000 \
                 -o outdir

# genomeindex mode
panel_somatic -c config.cfg -m genomeindex
```

<div id="disqus_thread"></div>
<script>
    /**
     *  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
     *  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables
     */
    /*
    var disqus_config = function () {
        this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
        this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
    };
    */
    (function() {  // DON'T EDIT BELOW THIS LINE
        var d = document, s = d.createElement('script');

        s.src = '//doc-iseq.disqus.com/embed.js';

        s.setAttribute('data-timestamp', +new Date());
        (d.head || d.body).appendChild(s);
    })();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
