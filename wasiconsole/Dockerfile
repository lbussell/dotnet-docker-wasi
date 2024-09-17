# Learn about building .NET container images:
# https://github.com/dotnet/dotnet-docker/blob/main/samples/README.md
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG TARGETARCH
ARG RID="wasi-wasm"
ENV WASI_SDK_PATH=/opt/wasi-sdk/

WORKDIR /source
RUN dotnet workload install wasm-tools wasi-experimental

COPY --link . .
# The SDK's "version check" currently looks for the existence of a VERSION* file
# https://github.com/dotnet/runtime/blob/657f18dbd8ac9ffae3abb79104d038155387df31/src/mono/wasi/build/WasiApp.targets#L104
# https://github.com/dotnet/runtime/pull/107599#discussion_r1751918087
RUN --mount=type=cache,target=/root/.nuget \
    --mount=type=bind,from=ghcr.io/webassembly/wasi-sdk:sha-754aec3@sha256:919f5346107dadedddd9851db72b92d7cd52115fff3969500c9b6f3c8215cd03,source=/opt/wasi-sdk/,target=/opt/wasi-sdk/,rw=true \
    touch /opt/wasi-sdk/VERSION24 \
        && dotnet publish \
        && chmod a+x /source/bin/Release/net8.0/wasi-wasm/AppBundle/wasiconsole.wasm

FROM scratch
ENTRYPOINT ["/wasiconsole.wasm"]
COPY --link --from=build /source/bin/Release/net8.0/wasi-wasm/AppBundle/wasiconsole.wasm /wasiconsole.wasm
