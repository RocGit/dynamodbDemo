# dynamodbDemo
Demos dynamodb

### 下载DynamoDB包
[Amazon DynamoDB](https://docs.aws.amazon.com/zh_cn/amazondynamodb/latest/developerguide/DynamoDBLocal.DownloadingAndRunning.html)

### 配置凭证
DynamoDBLocal 凭证随意
```
AWS Access Key ID: "fakeMyKeyId"
AWS Secret Access Key: "fakeSecretAccessKey"
```

### 启动
将下载的包解压缩，在 DynamoDBLocal.jar 的目录 Gitbash或CMD执行：

`java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -sharedDb`

如果是Windows PowerShell则执行：

`java -D"java.library.path=./DynamoDBLocal_lib" -jar DynamoDBLocal.jar`

默认情况下，DynamoDB 使用端口 8000。修改端口启动:

`java -Djava.library.path=./DynamoDBLocal_lib -jar DynamoDBLocal.jar -port 8001`

### 访问本地运行的 DynamoDB
`aws dynamodb list-tables --endpoint-url http://localhost:8001`

执行以上命令需要 AWS CLI环境，参照：https://docs.aws.amazon.com/zh_cn/amazondynamodb/latest/developerguide/Tools.CLI.html
