https://github.com/oneclick/rubyinstaller2/blob/master/docker/Dockerfile


$env:DOCKER_BUILDKIT = 0

docker build -t win-ruby .
docker run win-ruby

