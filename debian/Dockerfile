FROM tianon/debian-devel

# since we're uploading to experimental, let's build with deps from it too
RUN echo 'APT::Default-Release "experimental";' > /etc/apt/apt.conf.d/default

# start by adding just "debian/control" so we can get mk-build-deps with maximum caching
COPY control /usr/src/gosu/debian/
WORKDIR /usr/src/gosu

# get all the build deps of _this_ package in a nice repeatable way
RUN apt-get update && mk-build-deps -irt'apt-get --no-install-recommends -yV' debian/control && dpkg-checkbuilddeps

# need our debian/ directory to compile _this_ package
COPY . /usr/src/gosu/debian

# go download and unpack our upstream source
RUN uscan --force-download --verbose --download-current-version
RUN /tianon/extract-origtargz.sh

# tianon is _really_ lazy, and likes a preseeded bash history
RUN echo '/tianon/extract-origtargz.sh && dpkg-buildpackage -us -uc && lintian -EvIL+pedantic' >> /root/.bash_history
