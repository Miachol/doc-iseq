## iseq main package

Due to institutional restrictions, I cannot put source code in GitHub.
Please [email](mailto:lee_jianfeng@foxmail.com) me directly if you want to download the iseq source code.

```bash
# Download the source code and gunzip
python setup.py install
# Or use pip
pip install .
```

## iseq required softwares and databases

```r
# Using BioInstaller
# install.packages("BioInstaller")
library(BioInstaller)
install.packages("optparse")
requirements = c("bwa", "samtools", "picard", "vcftools", "gatk",
				 "varscan2", "ANNOVAR", "tvc", "lofreq", "gatk_bundle")
dest.dir="~/opt/packages"
download.dir=paste0("~/opt/source_code/", requirements)
req.len <- length(requirements)
install.bioinfo(requirements, destdir = rep(dest.dir, req.len), download.dir = download.dir)
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
