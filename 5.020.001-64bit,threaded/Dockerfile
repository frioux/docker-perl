FROM buildpack-deps
MAINTAINER Peter Martini <PeterCMartini@GMail.com>

RUN apt-get update && apt-get install -y curl procps

RUN mkdir /usr/src/perl
WORKDIR /usr/src/perl
RUN curl -SL http://www.cpan.org/src/5.0/perl-5.20.1.tar.gz \
                | tar -xz --strip-components=1

RUN ./Configure -Dusethreads -Duse64bitall -des \
        && make -j$(nproc) \
        && TEST_JOBS=$(nproc) make test_harness \
        && make install \
        && make veryclean

WORKDIR /usr/src
RUN curl -LO https://raw.githubusercontent.com/miyagawa/cpanminus/master/cpanm \
        && chmod +x cpanm \
        && ./cpanm App::cpanminus \
        && rm ./cpanm

WORKDIR /root

CMD ["perl5.20.1","-de0"]