# Use Debian slim as base for smaller image size
FROM debian:bullseye-slim

# Prevent tzdata from requiring interactive input
ENV DEBIAN_FRONTEND=noninteractive

ENV QUARTO_PORT=4040

# Install minimal dependencies in a single layer
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    ca-certificates \
    gdebi-core \
    pandoc \
    && rm -rf /var/lib/apt/lists/*

# Install Quarto in a single layer
RUN curl -LO https://quarto.org/download/latest/quarto-linux-amd64.deb \
    && gdebi --non-interactive quarto-linux-amd64.deb \
    && rm quarto-linux-amd64.deb

# Set up working directory
WORKDIR /docs

COPY . .

EXPOSE 4040

CMD ["quarto", "preview"]