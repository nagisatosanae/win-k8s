$env:DOCKER_BUILDKIT = 0

docker build -t win-bit .
docker run win-bit

https://learn.microsoft.com/ko-kr/cpp/windows/latest-supported-vc-redist?view=msvc-170

 docker build -t win-bit -f .\ExeDockerfile .
 