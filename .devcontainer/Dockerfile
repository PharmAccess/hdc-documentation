# Stage 1: Builder - clone the repo and build the CLI binary
FROM condaforge/miniforge3:latest AS builder
ENV PIP_NO_CACHE_DIR=1
ENV DEBIAN_FRONTEND=noninteractive

# Install build dependencies
RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
    ca-certificates \
    git \
    wget \
    dpkg \
    curl \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Go using the official binary distribution
ENV GO_VERSION=1.22.11
RUN wget https://golang.org/dl/go${GO_VERSION}.linux-amd64.tar.gz && \
    tar -C /usr/local -xzf go${GO_VERSION}.linux-amd64.tar.gz && \
    rm go${GO_VERSION}.linux-amd64.tar.gz && \
    ln -s /usr/local/go/bin/go /usr/local/bin/go && \
    ln -s /usr/local/go/bin/gofmt /usr/local/bin/gofmt
ENV PATH="/usr/local/go/bin:${PATH}"

# Clone the repository and build the CLI binary
RUN git clone https://github.com/google/cql.git /tmp/cql
WORKDIR /tmp/cql
RUN go mod tidy
RUN go build -o cql ./cmd/cli

# Stage 2: Final image
FROM condaforge/miniforge3:latest
ENV PIP_NO_CACHE_DIR=1
ENV DEBIAN_FRONTEND=noninteractive

# Install system packages (includes tools for PDF rendering and Quarto)
RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
    ca-certificates \
    git \
    pandoc \
    curl \
    gdebi-core \
    librsvg2-bin \
    wget \
    dpkg \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Download and install Quarto v1.6.42
RUN wget https://github.com/quarto-dev/quarto-cli/releases/download/v1.6.42/quarto-1.6.42-linux-amd64.deb && \
    dpkg -i quarto-1.6.42-linux-amd64.deb || apt-get install -f -y && \
    rm quarto-1.6.42-linux-amd64.deb


# Install Python and other libraries via mamba
RUN mamba install --yes \
    "python>=3.11" \
    ipykernel \
    openjdk=17 \
    go-task \
    jupyter \
    jupyterlab \
    && mamba clean -tipy

# Install Python packages via pip
RUN pip install \
    dbt-core \
    polars \
    duckdb \
    pathling==8.0.0.dev0

# Copy the built CLI binary from the builder stage
COPY --from=builder /tmp/cql/cql /usr/local/bin/cql