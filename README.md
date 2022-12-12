# Manage AWS Lambda via CLI

1. Create security politics file
2. Create aws security role

```bash
  aws iam create-role \
    --role-name lambda-exemplo \
    --assume-role-policy-document file://politics.json \
    | tee logs/role.log
```

3. Create index.js and zip it

```bash
  zip function.zip index.js
```

4. Post function on AWS

```bash
  aws lambda create-function \
    --function-name hello-cli \
    --zip-file fileb://function.zip \
    --handler index.handler \
    --runtime nodejs12.x \
    --role arn:aws:iam::201807860611:role/lambda-exemplo \
   | tee logs/lambda-create.log
```

5. Call Lambda Function

```bash
  aws lambda invoke \
    --function-name hello-cli \
    --log-type Tail \
    logs/lambda-exec.log
```

6. Update index.js and zip it again

```bash
  zip function.zip index.js
```

7. Update function on AWS

```bash
  aws lambda update-function-code \
    --zip-file fileb://function.zip \
    --function-name hello-cli \
    --publish \
    | tee logs/lambda-update.log
```

8. Call Lambda Function, and verify changes

```bash
  aws lambda invoke \
    --function-name hello-cli \
    --log-type Tail \
    logs/lambda-exec-update.log
```

9. Clear AWS environment

```bash
  aws lambda delete-function \
    --function-name hello-cli
```

```bash
  aws iam delete-role \
    --role-name lambda-exemplo
```
