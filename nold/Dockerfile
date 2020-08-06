FROM vnijs/radiant

USER root

ENV CRAN http://cran.r-project.org

RUN apt-get update \
    && apt-get -y install rsync subversion

RUN cd /tmp \
    && svn co https://svn.r-project.org/R/trunk R-devel

ENV RPREFIX /opt/R-devel

ENV ROPTIONS --with-recommended-packages --enable-R-shlib --enable-R-static-lib \
             --enable-long-double=no

RUN cd /tmp/R-devel \
    && ./tools/rsync-recommended \
    && R_PAPERSIZE=letter                                    \
    R_BATCHSAVE="--no-save --no-restore"                     \
    PERL=/usr/bin/perl                                       \
    R_UNZIPCMD=/usr/bin/unzip                                \
    R_ZIPCMD=/usr/bin/zip                                    \
    R_PRINTCMD=/usr/bin/lpr                                  \
    AWK=/usr/bin/awk                                         \
    CFLAGS="-std=gnu99 -Wall -pedantic"                      \
    CXXFLAGS="-Wall -pedantic"                               \
    LIBS="-lz -lbz2 -llzma"                                  \
    ./configure --prefix=${RPREFIX} ${ROPTIONS}              \
    && make                                                  \
    && make install

# RUN rm /usr/bin/R
RUN ln -s /opt/R-devel/bin/R /usr/local/bin/R

# RUN echo "rsession-which-r=/opt/R-devel/bin/R" >> /etc/rstudio/rserver.conf
RUN echo "rsession-which-r=/usr/local/bin/R" >> /etc/rstudio/rserver.conf

RUN /usr/local/bin/R -e 'install.packages("igraph", repos = "https://cran.rstudio.com")' \
  -e 'options(HTTPUserAgent = sprintf("R/%s R (%s)", getRversion(), paste(getRversion(), R.version$platform, R.version$arch, R.version$os)))' \
  -e 'install.packages(c("radiant", "devtools", "roxygen2"), repos = "https://cran.rstudio.com")'

ENV RHUB_PLATFORM linux-x86_64-debian-gcc

USER ${NB_USER}
ENV HOME /home/${NB_USER}