# escape=`

ARG WINDOWS_VERSION=1809

#
# Builder Image - Windows Server Core
# Assemble the components required to run Fluent Bit
#
FROM mcr.microsoft.com/windows/servercore:$WINDOWS_VERSION as builder

# ARG FLUENTBIT_URL=https://releases.fluentbit.io/1.9/td-agent-bit-1.9.9-win64.zip
ARG FLUENTBIT_URL=https://releases.fluentbit.io/2.1/fluent-bit-2.1.7-win64.zip
# ARG FLUENTBIT_URL=https://releases.fluentbit.io/1.9/td-agent-bit-1.9.9-win64.zip

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

RUN Write-Host ('Creating folders'); `
    New-Item -Type Directory -Path /installation; `
    New-Item -Type Directory -Path /fluent-bit;

WORKDIR /installation

RUN Write-Host ('Installing Fluent Bit'); `
    Invoke-WebRequest -Uri $Env:FLUENTBIT_URL -OutFile /installation/td-agent-bit.zip; `
    Expand-Archive -Path /installation/td-agent-bit.zip -Destination /installation/fluent-bit; `
    Move-Item -Path /installation/fluent-bit/*/* -Destination /fluent-bit/;

#
# Technique from https://github.com/StefanScherer/dockerfiles-windows/blob/master/mongo/3.6/Dockerfile
# 
RUN Write-Host ('Installing Visual C++ Redistributable Package'); `
    $ProgressPreference = 'SilentlyContinue'; `
    Invoke-WebRequest -Uri 'https://aka.ms/vs/16/release/vc_redist.x64.exe' -OutFile /installation/vc_redist.x64.exe; `
    $ProgressPreference = 'Continue'; `
    Start-Process /installation/vc_redist.x64.exe -ArgumentList '/install', '/quiet', '/norestart' -NoNewWindow -Wait; `
    Copy-Item -Path /Windows/System32/msvcp140.dll -Destination /fluent-bit/bin; `
    Copy-Item -Path /Windows/System32/vccorlib140.dll -Destination /fluent-bit/bin; `
    Copy-Item -Path /Windows/System32/vcruntime140.dll -Destination /fluent-bit/bin;



FROM fluent/fluent-bit:windows-2019-2.1.7 as bit



#
# Runtime Image - Windows Server Nano
# Run Fluent Bit with appropriate configuration 
#
FROM mcr.microsoft.com/windows/servercore:ltsc2019

USER ContainerAdministrator
COPY fluent-bit-2.1.7-win64.exe fluent-bit-2.1.7-win64.exe
RUN fluent-bit-2.1.7-win64.exe /S /D=C:\fluent-bit

COPY --from=builder /Windows/System32/msvcp140.dll /fluent-bit/msvcp140.dll
COPY --from=builder /Windows/System32/vccorlib140.dll /fluent-bit/vccorlib140.dll
COPY --from=builder /Windows/System32/vcruntime140.dll /fluent-bit/vcruntime140.dll


# COPY --from=builder /fluent-bit /fluent-bit
# COPY --from=bit /fluent-bit/etc/ /fluent-bit/etc
# RUN setx /M PATH "%PATH%;C:\fluent-bit\bin"

# ENTRYPOINT ["c:\\fluent-bit\\bin\\fluent-bit.exe", "-i", "dummy", "-o", "stdout"]
# ENTRYPOINT [ "/fluent-bit.exe" ]
# ENTRYPOINT [ "fluent-bit.exe", "-c", "/fluent-bit/etc/fluent-bit.conf" ]



CMD [ "ping", "-t","localhost" ]
