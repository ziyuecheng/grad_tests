FROM --platform=$BUILDPLATFORM rust:1.77 AS buildbase
#RUN echo '54.152.209.167 registry-1.docker.io' >> /etc/hosts
WORKDIR /src

FROM --platform=$BUILDPLATFORM buildbase AS buildserver
COPY pn /src
RUN --mount=type=cache,target=/usr/local/cargo/git/db \
    --mount=type=cache,target=/usr/local/cargo/registry/cache \
    --mount=type=cache,target=/usr/local/cargo/registry/index \
    cargo build --release

FROM scratch AS pn
ENTRYPOINT [ "prime_numbers" ]
COPY --from=buildserver /src/target/release/prime_numbers /

