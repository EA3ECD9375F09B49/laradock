FROM golang:latest

LABEL maintainer="Mahmoud Zalt <mahmoud@zalt.me>"

# Set environment variables for Go
ENV GOPATH=/go
ENV PATH="$PATH:/go/bin"

# Change source settings based on ARG
ARG CHANGE_SOURCE=false

# Modify package sources if needed
RUN if [ "$CHANGE_SOURCE" = "true" ]; then \
        echo "Changing APT sources for China"; \
        sed -i 's/deb.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
        sed -i 's/security.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list && \
        sed -i 's/security-cdn.debian.org/mirrors.tuna.tsinghua.edu.cn/' /etc/apt/sources.list; \
    fi

# Update package lists and install required tools
RUN apt-get update && apt-get install -y vim && \
    rm -rf /var/lib/apt/lists/*

###########################################################################
# GoFrame Installation
###########################################################################

ARG INSTALL_GOF=false
ARG GOF_VERSION=latest

# Install GoFrame CLI if requested
RUN if [ "$INSTALL_GOF" = "true" ]; then \
        echo "Installing GoFrame CLI..."; \
        # Install required tools for wget
        apt-get update && apt-get install -y wget && \
        if [ "$GOF_VERSION" = "latest" ]; then \
            echo "Installing latest version of GoFrame CLI using official method."; \
            # Use the official method from GoFrame documentation
            wget -O gf "https://github.com/gogf/gf/releases/latest/download/gf_$(go env GOOS)_$(go env GOARCH)" && \
            chmod +x gf && \
            ./gf install -y && \
            rm ./gf; \
        else \
            echo "Installing GoFrame version $GOF_VERSION."; \
            # Check if version starts with '2.' for v2, otherwise use v1
            if [[ "$GOF_VERSION" == 2.* ]]; then \
                echo "Installing GoFrame v2..."; \
                wget -O gf "https://github.com/gogf/gf/releases/download/v${GOF_VERSION}/gf_$(go env GOOS)_$(go env GOARCH)" && \
                chmod +x gf && \
                ./gf install -y && \
                rm ./gf; \
            else \
                echo "Installing GoFrame v1..."; \
                wget -O gf "https://github.com/gogf/gf/releases/download/v${GOF_VERSION}/gf_$(go env GOOS)_$(go env GOARCH)" && \
                chmod +x gf && \
                ./gf install -y && \
                rm ./gf; \
            fi; \
        fi && \
        # Update PATH and verify installation
        export PATH=$PATH:/usr/local/bin && \
        which gf && \
        gf version || { echo 'GoFrame installation successful, but version check failed'; exit 1; }; \
    else \
        echo "Skipping GoFrame installation."; \
    fi

# Ensure gf is in PATH for all users
ENV PATH="$PATH:/usr/local/bin"

# Set the working directory
WORKDIR "$GOPATH/src"
