FROM --platform=$BUILDPLATFORM rust:1.73 AS buildstage
WORKDIR /src
COPY . .
RUN --mount=type=cache,target=/usr/local/cargo/git/db \
    --mount=type=cache,target=/usr/local/cargo/registry/cache \
    --mount=type=cache,target=/usr/local/cargo/registry/index \
    cargo build --release

FROM debian:bookworm-slim
COPY --link --from=buildstage /src/target/release/warp warp
ENTRYPOINT [ "./warp" ]