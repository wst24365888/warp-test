FROM --platform=$BUILDPLATFORM rust:1.73 AS buildbase
RUN rustup target add wasm32-wasi
WORKDIR /src

FROM --platform=$BUILDPLATFORM buildbase AS buildstage
COPY . .
RUN --mount=type=cache,target=/usr/local/cargo/git/db \
    --mount=type=cache,target=/usr/local/cargo/registry/cache \
    --mount=type=cache,target=/usr/local/cargo/registry/index \
    cargo build --target wasm32-wasi --release

FROM scratch
COPY --link --from=buildstage /src/target/wasm32-wasi/release/warp.wasm warp.wasm
ENTRYPOINT [ "warp.wasm" ]