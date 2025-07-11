# Build stage
FROM rust:1.82-alpine AS builder

WORKDIR /usr/src/sv2-custom-proxy
COPY ./sv2-custom-proxy .

# Install necessary dependencies for building
RUN apk add musl-dev pkgconfig libressl-dev

# Build the project in release mode
RUN cargo build --release

# Final stage
FROM alpine:latest

# Install necessary runtime dependencies
RUN apk update && apk add --no-cache \
        iproute2 \
        iputils-ping \
        iptables \
        curl

# Copy the binary from the builder stage
COPY --from=builder /usr/src/sv2-custom-proxy/target/release/sv2-custom-proxy /usr/local/bin/sv2-custom-proxy

# Set the working directory
WORKDIR /usr/local/bin/

# Copy the script and make it executable
COPY ./pools-latency-calculator/monitor_and_apply_latency.sh /usr/local/bin/monitor_and_apply_latency.sh
RUN chmod +x /usr/local/bin/monitor_and_apply_latency.sh