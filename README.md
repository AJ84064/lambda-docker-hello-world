
npm init -f

Create app.js

docker build -t lambda-docker-hello-world .

docker run -p 8080:8080 lambda-docker-hello-world

aws lambda invoke --region eu-west-1 --endpoint http://localhost:8080 --no-sign-request --function-name function --cli-binary-format raw-in-base64-out --payload '{"a":"b"}' output.txt

cat output.txt



Push to Private ECR (public ECR and Docker Hub won't work)


(Needs ecr-public:* permissions)
aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws

aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin xxxxxxxxx.dkr.ecr.eu-west-1.amazonaws.com
docker tag lambda-docker-hello-world:latest xxxxxxxxx.dkr.ecr.eu-west-1.amazonaws.com/lambda-docker-hello-world:latest
docker push xxxxxxxxx.dkr.ecr.eu-west-1.amazonaws.com/lambda-docker-hello-world:latest


Make sure updated to latest AWS CLI version


Now create the function. 

aws lambda create-function --package-type Image  --function-name lambda-docker-hello-world --role  arn:aws:iam::xxxxxxxxx:role/lambda-ex --code ImageUri=xxxxxxxxx.dkr.ecr.eu-west-1.amazonaws.com/lambda-docker-hello-world:latest

Now invoke the function

aws lambda --region eu-west-1 invoke --function-name lambda-docker-hello-world --cli-binary-format raw-in-base64-out --payload '{"a":"b"}' output.txt

cat output.txt

---

Reference:

https://aws.amazon.com/blogs/aws/new-for-aws-lambda-container-image-support/

https://docs.aws.amazon.com/cli/latest/reference/lambda/create-function.html



