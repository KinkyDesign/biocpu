# biocpu
Docker container with Bioconductor and Opencpu
Biocpu let's you have bioconductor and OpenCPU on the same image.

This way you can create rest API's on bioconductor packages. 

Example usage:
```
from pantelispanka/biocpu:latest

RUN R -e "install.packages(c('jsonlite', 'RCurl','Matrix', 'vegan'), repos='http://cran.cc.uoc.gr/mirrors/CRAN/')"

RUN Rscript -e 'source("http://bioconductor.org/biocLite.R")' -e 'biocLite(c("org.Hs.eg.db", "GSEABase", "GOstats", "Category", "GO.db"))'

COPY yourPackage.tar.gz /packages/

COPY server.conf /etc/opencpu/

USER root

RUN R CMD INSTALL /packages/yourPackage.tar.gz --library=/usr/local/lib/R/site-library

CMD /usr/sbin/apache2ctl -D FOREGROUND
```
explanation:

 **RUN R -e "install.packages(c('jsonlite', 'RCurl','Matrix', 'vegan'), repos='http://cran.cc.uoc.gr/mirrors/CRAN/')"** 

installs your package dependencies

**RUN Rscript -e 'source("http://bioconductor.org/biocLite.R")' -e 'biocLite(c("org.Hs.eg.db", "GSEABase", "GOstats", "blockcluster", "Category", "GO.db"))'**

installs your bioconductor dependencies

**COPY server.conf /etc/opencpu/** 

Replaces the server.conf for the OpenCPU server. This may be useful since bioconductor may need more time than the default time of OpenCPU


Install your application and run the image


**USER root**

**RUN R CMD INSTALL /packages/yourPackage.tar.gz --library=/usr/local/lib/R/site-library**

**CMD /usr/sbin/apache2ctl -D FOREGROUND**










