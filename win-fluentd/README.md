https://github.com/fluent/fluentd-docker-image/blob/master/v1.16/windows-ltsc2019/Dockerfile

$env:DOCKER_BUILDKIT = 0

docker build -t win-fluentd .
docker run win-fluentd


zip -9vr test.zip ./test
