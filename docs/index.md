## Introduction
[iseq](http://bioinfo.rjh.com.cn/labs/jhuang/tools/iseq) is a lightweight analysis pipeline for gene mutation analysis using NGS data, which can be used to execute several bioinfomatics softwares command in python and a integrity analysis work-flow for gene panel sequencing data.

Genome reference, Fastq, Sam, Bam, Vcf and other format files be packaged in divided module that you can implement some of foundamental or specific manipulation.

Mainly Feature:

- Multiple sequence alignment softwares
- Multiple variantion detection softwares
- Easy to use
- Extend easily
- Single configuration file do all things
- Output log be divided into in individual file


## Requirements
- Java
- R
- BioInstaller
- GATK
- tvc
- bwa
- lofreq
- varscan2
- samtools
- vcftools
- picard
- ANNOVAR

## Work Flow
Gene panel sequencing work flow:

![Work Flow](http://i.imgur.com/9U6rJXD.jpg)

FASTQ and/or BAM can be as the input and several quality control tools, like FastqQC/PRINSEQ/GATK, be used to filter raw data and get the clean FASTQ and/or BAM file. The mainly analysis step as followed.

- Map to Reference (FASTQ)
- Mark Duplicates (BAM)
- Base Recalibration (BAM)
- Indel Realignment (BAM)
- Variants Calling (BAM)
- Variants Annotation (VCF/CSV)
- Variants Filtration (VCF/CSV/Result)
- Visuliztion (Result)



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