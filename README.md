# .NET + WASI + Docker

## Prerequisites

### For building and running the container

1. Docker Desktop - See the [Wasm workloads](https://docs.docker.com/desktop/wasm/) documentation

### For building and running locally

TBD - or see the original .NET `wasiconsole` template README.

## Build

```bash
docker buildx build \
    --platform wasi/wasm \
    --load \
    -t wasiconsole \
    -f ./wasiconsole/Dockerfile \
    ./wasiconsole/
```

## Run using Docker Desktop

```bash
docker run --rm --runtime=io.containerd.wasmedge.v1 --platform=wasi/wasm wasiconsole
```

## Changes from `wasiconsole` template

```diff
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
    <RuntimeIdentifier>wasi-wasm</RuntimeIdentifier>
    <OutputType>Exe</OutputType>
    <PublishTrimmed>true</PublishTrimmed>

+   <WasmSingleFileBundle>true</WasmSingleFileBundle>
+   <InvariantGlobalization>true</InvariantGlobalization>
+   <Nullable>enable</Nullable>
  </PropertyGroup>
</Project>
```
