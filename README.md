# Windows docker images for multiples versions of the .NET SDK

This repo aims to provide docker images that contain multiple versions of the .NET SDK to make builds more stables. Microsoft tends to remove unsupported version of the .NET SKD quite quickly which makes build pipelines prone for errors and in need of installing missing SDKs or runtimes on each run.

## Disclaimer

Images are published to ghcr.io/smkanadl/multi-sdk. Use at your own risk! Dockerfiles are heavily inspired by the [official documentation](https://github.com/dotnet/dotnet-docker).

## Problems

All images are based on the windowsservercore
 * nanoserver images are not capable of running native x86 binaries https://github.com/microsoft/Windows-Containers/issues/118
 * .NET 4.8 framework cannot run on nanoserver https://learn.microsoft.com/en-us/dotnet/architecture/microservices/net-core-net-framework-containers/net-container-os-targets
