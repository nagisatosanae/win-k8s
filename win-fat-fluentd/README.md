https://github.com/fluent/fluentd-docker-image/blob/master/v1.16/windows-ltsc2019/Dockerfile

$env:DOCKER_BUILDKIT = 0

docker build -t win-fat-fluentd .
docker run win-fat-fluentd


zip -9vr test.zip ./test
