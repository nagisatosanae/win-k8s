FROM fluent/fluentd:v1.14-windows-ltsc2019-1 as core



# RUN powershell icacls c:\\ruby27 /grant "BUILTIN\Administrators:F" /t

# FROM mcr.microsoft.com/windows/nanoserver:1809
FROM mcr.microsoft.com/windows/nanoserver:ltsc2019
COPY --from=core c:\\fluent c:\\fluent
COPY --from=core c:\\ruby27 c:\\ruby27
ENV FLUENTD_CONF="fluent.conf"
ENV Path="${Path};C:\\ruby27\\bin"
EXPOSE 24224 5140
ENTRYPOINT ["cmd", "/k", "fluentd", "--config", "C:\\fluent\\conf\\fluent.conf"]
# CMD [ "ping", "localhost", "-t" ]


