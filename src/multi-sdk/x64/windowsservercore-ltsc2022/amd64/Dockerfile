# escape=`

# Installer image
FROM mcr.microsoft.com/windows/servercore:ltsc2022-amd64 AS installer

RUN powershell -Command `
        $ErrorActionPreference = 'Stop'; `
        $ProgressPreference = 'SilentlyContinue'; `
        Invoke-WebRequest `
            -UseBasicParsing `
            -Uri https://dot.net/v1/dotnet-install.ps1 `
            -OutFile dotnet-install.ps1; `
        ./dotnet-install.ps1 `
            -InstallDir '\Program Files\dotnet' `
            -Architecture x64 `
            -Channel 3.1; `
        ./dotnet-install.ps1 `
            -InstallDir '\Program Files\dotnet' `
            -Architecture x64 `
            -Channel 5.0.4xx; `
        ./dotnet-install.ps1 `
            -InstallDir '\Program Files\dotnet' `
            -Architecture x64 `
            -Channel 6.0.4xx; `
        ./dotnet-install.ps1 `
            -InstallDir '\Program Files\dotnet' `
            -Architecture x64 `
            -Channel 7.0.4xx; `
        ./dotnet-install.ps1 `
            -InstallDir '\Program Files\dotnet' `
            -Architecture x64 `
            -Channel 8.0.1xx; `
        Remove-Item -Force dotnet-install.ps1

# SDK/runtime image
# We currently have to use servercore as nanoserver is ONLY AMD64 and does not support x64 execution #https://github.com/microsoft/Windows-Containers/issues/118
FROM mcr.microsoft.com/windows/servercore:ltsc2022-amd64
LABEL org.opencontainers.image.source="https://github.com/smkanadl/dotnet-docker-multi-framework"

ENV `
    # Enable detection of running in a container
    DOTNET_RUNNING_IN_CONTAINER=true `
    # Do not generate certificate
    DOTNET_GENERATE_ASPNET_CERTIFICATE=false `
    # Do not show first run text
    DOTNET_NOLOGO=true `
    # Enable correct mode for dotnet watch (only mode supported in a container)
    DOTNET_USE_POLLING_FILE_WATCHER=true `
    # Skip extraction of XML docs - generally not useful within an image/container - helps performance
    NUGET_XMLDOC_MODE=skip

# In order to set system PATH, ContainerAdministrator must be used
USER ContainerAdministrator
RUN setx /M PATH "%PATH%;C:\Program Files\dotnet"
USER ContainerUser

COPY --from=installer ["/Program Files/dotnet", "/Program Files/dotnet"]
