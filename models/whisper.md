# Whisper.cpp Setup on Ubuntu

This guide covers the installation, compilation, and usage of the `whisper.cpp` library with Go bindings on Ubuntu. It includes prerequisites, model downloads, building examples, and linking for Go applications.

## Quick Start

A fast way to get started if the repository and environment are already set up

```bash
cd whisper.cpp/bindings/go
make whisper

cd -
source setup-env.sh
go run ./cmd/main.go
```

## Prerequisites

These are the software and tools required before building the library.

### 1. Install Go

Go is required to compile the Go bindings. The commands below install Go 1.24.5. Replace 1.24.5 with the latest version if needed.

```bash
# Navigate to home
cd ~

# Download Go
wget https://go.dev/dl/go1.24.5.linux-amd64.tar.gz

# Remove any previous installation and extract
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.24.5.linux-amd64.tar.gz

# Add Go to PATH
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc

# Verify installation
go version

# Return to previous directory
cd -
```

### 2. Install Build Tools

`make` and `cmake` are required to build the C++ components and compile the static library.

```bash
sudo apt update
sudo apt install -y make cmake
```

## Download Whisper Model

Whisper models are required to perform transcription. Download the base model at [ggml-base.bin](https://huggingface.co/ggerganov/whisper.cpp/tree/main).

The model file should be stored in a convenient directory for use with examples or your Go application.

## Build and Test

### 1. Clone Whisper.cpp

```bash
git clone https://github.com/ggml-org/whisper.cpp.git
cd whisper.cpp/bindings/go
```

Cloning the repository fetches the source code and Go bindings.

### 2. Compile and Test

```bash
make test
```

This builds the static library `libwhisper.a` and runs test programs to ensure the compilation is successful.

### Build Example

Reference: [ggml-org/whisper.cpp](https://github.com/ggml-org/whisper.cpp/tree/master/bindings/go)

Examples demonstrate usage of the library with different models and audio files.

```bash
# Build examples
make examples

# Build examples with CUDA support
GGML_CUDA=1 make examples

# Download all models
./build/go-model-download -out models

# Run an example
./build/go-whisper -model models/ggml-tiny.en.bin samples/jfk.wav
```

Examples provide a quick way to verify that the library works and to experiment with transcription.

## Build Whisper Library for Go

To build the library for integration into Go applications without running tests:

```bash
cd whisper.cpp/bindings/go
make whisper
```

Then link your go binary against whisper by setting the environment variables C_INCLUDE_PATH and LIBRARY_PATH to point to the whisper.h file directory and libwhisper.a file directory respectively.

These variables allow the Go compiler to find `whisper.cpp` headers and the static library.

```bash
export C_INCLUDE_PATH=/path/to/whisper.cpp/include:/path/to/whisper.cpp/ggml/include
export LIBRARY_PATH=/path/to/whisper.cpp/build_go/src:/path/to/build_go/ggml/src
```
Unset after building

```bash
unset C_INCLUDE_PATH
unset LIBRARY_PATH
```

## Cloning Repository with Submodules

Some dependencies are included as submodules. Using `--recurse-submodules` ensures all required repositories are cloned.

```bash
git clone <repo>
git submodule update --init --recursive

# Or in one step
git clone --recurse-submodules <repo>
```

