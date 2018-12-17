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

### 注意事项：
> 如果您使用 -sharedDb 选项，则 DynamoDB 将创建一个名为 shared-local-instance.db 的数据库文件。连接到 DynamoDB 的每个程序都将访问此文件。如果删除此文件，则将丢失存储在此文件中的所有数据。
> 
> 如果省略 -sharedDb，则数据库文件出现在应用程序配置中时，将命名为 myaccesskeyid_region.db（包含 AWS 访问密钥 ID 和区域）。如果删除此文件，则将丢失存储在此文件中的所有数据。
> 
> 如果使用 -inMemory 选项，则 DynamoDB 完全不会编写任何数据库文件。相反，所有数据将被写入内存中，并且在您终止 DynamoDB 时不会保存这些数据。
> 
> 如果使用 -optimizeDbBeforeStartup 选项，则还必须指定 -dbPath 参数，以便 DynamoDB 可找到其数据库文件。
> 
> 适用于 DynamoDB 的 AWS 开发工具包要求您的应用程序配置指定访问密钥值和 AWS 区域值。除非您使用的是 -sharedDb 或 -inMemory 选项，否则 DynamoDB 将使用这些值来命名本地数据库文件。
> 
> 这些值不必是有效的 AWS 值也能在本地运行。但是，您可能发现使用有效值非常方便，因为您以后只需更改当前使用的终端节点即可在云中运行您的代码。

### 命令行选项
> -cors value — 实现对适用于 JavaScript 的跨源资源共享 (CORS) 的支持。您必须提供特定域的逗号分隔“允许”列表。-cors 的默认设置是星号 (*)，这将允许公开访问。
> 
> -dbPath value — DynamoDB 写入数据库文件的目录。如果您未指定此选项，则文件将写入到当前目录。您不能同时指定 -dbPath 和 -inMemory。
> 
> -delayTransientStatuses — 使 DynamoDB 为某些操作引入延迟。DynamoDB (下载版本) 几乎可以即时执行某些任务，如针对表和索引的创建/更新/删除操作。但是，DynamoDB 服务需要更多时间才能完成这些任务。设置此参数可帮助在您的计算机上运行的 DynamoDB 更逼真地模拟 DynamoDB Web 服务的行为。（目前，此参数仅为处于 CREATING 或 DELETING 状态的全局二级索引引入延迟。）
> 
> -help — 打印使用摘要和选项。
> 
> -inMemory — DynamoDB 将在内存中运行，而不使用数据库文件。停止 DynamoDB 时，不会保存任何数据。您不能同时指定 -dbPath 和 -inMemory。
> 
> -optimizeDbBeforeStartup — 在计算机上启动 DynamoDB 之前优化底层数据库表。使用此参数时，您还必须指定 -dbPath。
> 
> -port value — DynamoDB 用于与您的应用程序进行通信的端口号。如果您未指定此选项，则默认端口是 8000。
> 
> -sharedDb — 如果您指定 -sharedDb，则 DynamoDB 将使用单个数据库文件，而不是针对每个凭证和区域使用不同的文件。

### DynamoDB Local与DynamoDB Web 服务的差别如下：

> 在客户端层面上不支持区域和不同的 AWS 账户。
> 
> 可下载的 DynamoDB 中会忽略预置的吞吐量设置，即使 CreateTable 操作需要这些设置也是如此。对于 CreateTable，您可以为预置读取和写入吞吐量指定任何数字，即使不使用这些数字也是如此。您每天可以按需调用 UpdateTable 任意次数。但是，对预置吞吐量值的任何更改都会忽略。
> 
> Scan 操作将按顺序执行。不支持并行扫描。Segment 操作的 TotalSegments 和 Scan 参数将被忽略。
> 
> 对表数据的读取和写入操作的速度仅受计算机速度的限制。CreateTable、UpdateTable 和 DeleteTable 操作立即发生，表状态始终为“ACTIVE”。仅更改表或全局二级索引的预置吞吐量设置的 UpdateTable 操作将立即执行。如果 UpdateTable 操作创建或删除任何全局二级索引，则这些索引在常规状态（如，分别为 CREATING 和 DELETING）之后变为 ACTIVE 状态。在此期间，表保持为 ACTIVE 状态。
> 
> 读取操作具有最终一致性。但是，由于 DynamoDB 在计算机上的运行速度，大多数读取操作会表现为强一致性。
> 
> 不跟踪占用的容量单位。在操作响应中返回 Null，而不是容量单位。
> 
> 将不跟踪项目集合指标和项目集合大小。在操作响应中返回 Null，而不是项目集合指标。
> 
> 在 DynamoDB 中，每个结果集返回的数据有 1 MB 的限制。DynamoDB Web 服务和下载版本均将强制执行此限制。但是，在查询索引时，DynamoDB 服务将仅计算投影键和属性的大小。而 DynamoDB 的可下载版本则会计算整个项目的大小。
> 
> 如果使用的是 DynamoDB 流，则创建分片的速率可能不同。在 DynamoDB Web 服务中，分片创建行为部分受表分区活动的影响。在本地运行 DynamoDB 时，不存在表分区活动。在任一情况下，分区都是临时的，因此您的应用程序不应依赖分区行为。