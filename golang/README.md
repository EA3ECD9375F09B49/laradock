# Golang Container

This container provides a Go development environment with optional GoFrame framework support.

## Usage

### Basic Usage

1. Start the container:
   ```bash
   docker-compose up -d golang
   ```

2. Enter the container:
   ```bash
   docker-compose exec golang bash
   ```

### GoFrame Installation

To install GoFrame framework in the golang container, follow these steps:

1. Edit your `.env` file and set the following variables:
   ```bash
   # Install GoFrame CLI tool in the golang container
   GOLANG_INSTALL_GOF=true
   
   # GoFrame version to install (use "latest" for the latest version)
   GOLANG_GOF_VERSION=latest
   ```

2. Rebuild the golang container:
   ```bash
   docker-compose build golang
   ```

3. Start the container:
   ```bash
   docker-compose up -d golang
   ```

4. Verify GoFrame installation:
   ```bash
   docker-compose exec golang gf version
   ```

### GoFrame Version Options

You can specify a specific version of GoFrame by setting `GOLANG_GOF_VERSION` to a version tag:

- `GOLANG_GOF_VERSION=latest` - Installs the latest version of GoFrame (default)
- `GOLANG_GOF_VERSION=2.6.1` - Installs GoFrame v2.6.1
- `GOLANG_GOF_VERSION=2.5.0` - Installs GoFrame v2.5.0
- `GOLANG_GOF_VERSION=1.16.9` - Installs GoFrame v1.16.9

Note: The installation follows the official GoFrame documentation method. It automatically detects the operating system and architecture, downloads the appropriate binary, and installs it using the `gf install -y` command.

### Creating a GoFrame Project

After installing GoFrame, you can create a new project:

1. Enter the container:
   ```bash
   docker-compose exec golang bash
   ```

2. Create a new GoFrame project:
   ```bash
   gf init my-project
   cd my-project
   gf run main.go
   ```

### Working with Go Projects

Your Go projects should be placed in the `/go/src` directory inside the container, which is mapped to `${APP_CODE_PATH_HOST}/golangProject/src` on your host machine.

## Environment Variables

- `CHANGE_SOURCE`: Set to `true` to change package sources to mirrors (useful in China)
- `GOLANG_INSTALL_GOF`: Set to `true` to install GoFrame CLI tool
- `GOLANG_GOF_VERSION`: Specify the GoFrame version to install (default: `latest`)

## Ports

- `16001`: Default port for Go applications (can be changed in docker-compose.yml)

## Volumes

- `${APP_CODE_PATH_HOST}/golangProject/src:/go`: Maps your local Go projects to the container