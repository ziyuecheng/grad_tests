FROM --platform=$BUILDPLATFORM rust:1.64 AS buildbase
RUN rustup target add wasm32-wasi
WORKDIR /src

FROM --platform=$BUILDPLATFORM buildbase AS buildserver
COPY pn /src
RUN --mount=type=cache,target=/usr/local/cargo/git/db \
    --mount=type=cache,target=/usr/local/cargo/registry/cache \
    --mount=type=cache,target=/usr/local/cargo/registry/index \
    cargo build --target wasm32-wasi --release

FROM scratch AS pn
ENTRYPOINT [ "prime_numbers.wasm" ]
COPY --from=buildserver /src/target/wasm32-wasi/release/prime_numbers.wasm prime_numbers.wasm
