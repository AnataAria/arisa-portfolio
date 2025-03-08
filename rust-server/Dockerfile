FROM rustlang/rust:nightly-bookworm-slim AS base

FROM base AS installer
WORKDIR /installer
COPY Cargo.toml Cargo.lock ./
RUN cargo fetch --locked
COPY . .
RUN cargo build --release

FROM debian:bullseye-slim
WORKDIR /app
RUN apt-get update && apt-get install -y libssl-dev && apt-get clean

RUN addgroup --system --gid 1001 ariportfolio 
RUN adduser --system --uid 1001 ariportfolio
USER ariportfolio

COPY --from=installer --chown=ariportfolio:ariportfolio /installer/target/release/rust-server ./app

ENTRYPOINT [ "./app" ]
