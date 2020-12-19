chmod -x entry.sh
docker build -t lambda/python .
docker tag lambda/python:latest lambda/python:3.9-alpine3.12
docker run -p 9000:8080 lambda/python:3.9-alpine3.12
curl -XPOST "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{}' | jq


# --image-tag-mutability IMMUTABLE
aws ecr create-repository --repository-name lambda/python --image-scanning-configuration scanOnPush=true
docker tag lambda/python:latest 123456789012.dkr.ecr.ap-northeast-1.amazonaws.com/lambda/python:3.9-alpine3.12
aws ecr get-login-password | docker login --username AWS --password-stdin 123456789012.dkr.ecr.ap-northeast-1.amazonaws.com
docker push 123456789012.dkr.ecr.ap-northeast-1.amazonaws.com/lambda/python:3.9-alpine3.12