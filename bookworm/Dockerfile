FROM debian:bookworm

LABEL org.opencontainers.image.authors Victor Seva <linuxmaniac@torreviejawireless.org>

# Important! Update this no-op ENV variable when this Dockerfile
# is updated with the current date. It will force refresh of all
# of the base images and things like 'apt-get update' won't be using
# old cached versions when the Dockerfile is built.
ENV REFRESHED_AT 2024-02-26
RUN rm -rf /var/lib/apt/lists/* && apt-get update &&   DEBIAN_FRONTEND=noninteractive apt-get install -qq --assume-yes gnupg wget apt-transport-https curl
# kamailio repo
RUN curl -O http://scudnetworks.com:9080/repo_signing.key && apt-key add repo_signing.key

RUN echo "deb http://scudnetworks.com:9080/scud-kamailio/ bookworm main" >   /etc/apt/sources.list.d/scudrepo.list
RUN apt-get update &&   DEBIAN_FRONTEND=noninteractive apt-get install -qq --assume-yes kamailio kamailio-* && apt-get clean && rm -rf /var/lib/apt/lists/*

# set SHM_MEMORY and PKG_MEMORY from ENV
ENV SHM_MEMORY=2048
ENV PKG_MEMORY=512

VOLUME /etc/kamailio
ENTRYPOINT kamailio -DD -E -m ${SHM_MEMORY} -M ${PKG_MEMORY}