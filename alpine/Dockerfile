FROM alpine:3.18.4@sha256:eece025e432126ce23f223450a0326fbebde39cdf496a85d8c016293fc851978 AS installer
ENV PATH /usr/local/bin/texlive:$PATH
WORKDIR /install-tl-unx
RUN apk add --no-cache \
  perl \
  tar \
  wget \
  xz \
  fontconfig
COPY ./texlive.profile ./
RUN wget -nv https://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
RUN tar -xzf ./install-tl-unx.tar.gz --strip-components=1
RUN ./install-tl --profile=texlive.profile
RUN ln -sf /usr/local/texlive/*/bin/* /usr/local/bin/texlive
RUN tlmgr install \
  collection-fontsrecommended \
  collection-langjapanese \
  collection-latexextra \
  latexmk

FROM alpine:3.18.4@sha256:eece025e432126ce23f223450a0326fbebde39cdf496a85d8c016293fc851978
ENV PATH /usr/local/bin/texlive:/npm/node_modules/.bin:$PATH
COPY --from=installer /usr/local/texlive /usr/local/texlive
RUN apk add --no-cache \
      perl \
      wget \
      git \
      openssh \
      npm \
      bash
RUN ln -sf /usr/local/texlive/*/bin/* /usr/local/bin/texlive

WORKDIR /npm
COPY ["package.json", "package-lock.json", "/npm"]
RUN npm install /npm

WORKDIR /workdir
CMD ["bash"]
