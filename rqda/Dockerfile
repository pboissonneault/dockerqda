# based on debian 8 (2014)
FROM debian:jessie-slim

# provide information about the maintainer of the image
MAINTAINER Frédéric M. Vanwindekens (f.vanwindekens@cra.wallonie.be)

WORKDIR /work/

# Setting up
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DISPLAY :0

# Installing systems dependencies
RUN apt-get update \
    && apt-get install -qqy wget \
    && apt-get install -qqy x11-apps \
    && apt-get install -qqy libgtk2.0-dev \
    && apt-get install -qqy libcanberra-gtk-module libcanberra-gtk3-module

## Installing a basic R + minimal dependencies for RQDA
RUN apt-get install -qqy r-base \
    && wget -P $HOME https://cran.r-project.org/src/contrib/Archive/cairoDevice/cairoDevice_2.24.tar.gz \
    && wget -P $HOME https://cran.r-project.org/src/contrib/Archive/gWidgets/gWidgets_0.0-54.2.tar.gz \
    && wget -P $HOME https://cran.r-project.org/src/contrib/Archive/gWidgetsRGtk2/gWidgetsRGtk2_0.0-86.1.tar.gz \
    && wget -P $HOME https://cran.r-project.org/src/contrib/Archive/RQDA/RQDA_0.2-8.tar.gz \
    && wget -P $HOME https://cran.r-project.org/src/contrib/Archive/RSQLite/RSQLite_1.0.0.tar.gz \
    && wget -P $HOME https://cran.r-project.org/src/contrib/Archive/DBI/DBI_0.3.1.tar.gz \
    && wget -P $HOME https://cran.r-project.org/src/contrib/Archive/igraph/igraph_0.6.tar.gz \
    && R CMD INSTALL --no-test-load $HOME/cairoDevice_2.24.tar.gz \
    && R CMD INSTALL $HOME/gWidgets_0.0-54.2.tar.gz \
    && apt-get install -qqy r-cran-RGtk2 \
    && R CMD INSTALL $HOME/DBI_0.3.1.tar.gz \
    && R CMD INSTALL $HOME/RSQLite_1.0.0.tar.gz \
    && R CMD INSTALL $HOME/igraph_0.6.tar.gz \
    && rm -rf /tmp/downloaded_packages/ /tmp/*.rds \
    && rm -rf /var/lib/apt/lists/*
    
COPY ./entrypoint.sh /
RUN chmod a+x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]