FROM --platform=$BUILDPLATFORM rust:1.64 AS buildbase

RUN curl https://sh.rustup.rs -sSf | sh -s -- -y --profile=minimal --default-toolchain stable

WORKDIR /src

FROM --platform=$BUILDPLATFORM buildbase AS buildserver
COPY server /src
RUN --mount=type=cache,target=/usr/local/cargo/git/db \
    --mount=type=cache,target=/usr/local/cargo/registry/cache \
    --mount=type=cache,target=/usr/local/cargo/registry/index \
    cargo build --release

FROM scratch AS server
ENTRYPOINT [ "http_server" ]
COPY --from=buildserver /src/target/release/ /
