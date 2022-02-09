# Data Engineering on AWS

## Platform Design
### Selecting the tools
### Client
### Connect
### Buffer
### Process
### Store
### Visualize

## Data Pipelines
### Data Ingestion Pipeline
### Stream to Raw Storage Pipeline
### Stream to DynamoDB Pipeline
### Visualization API Pipeline
### Visualization Redshift Data Warehouse
### Batch Processing Pipeline

---

## Data Ingestion Pipeline

### Create Lambda for API
1. Open <ins>Lambda</ins> on AWS
2. Go to Lambda Functions and press "Create Function"
3. Fill the requeriments
   - **Function name:** "test"
   - **Runtinme**: Python 3.9
   - **Execution role:** Create a new role with basic Lambda permissions
4. Press *Create Function*



### Create API Gateway
1. Open <ins>API Gateway</ins> on AWS
2. Select "Build" on REST API option
3. Fill the requeriments
   - **Select whether you would like to create a REST API or a WebSocket API:** REST
   - **In Amazon API Gateway, a REST API refers to a collection of resources and methods that can be invoked through HTTPS endpoints**: New API
   - **API Name:** "test"  
   - **Endpoint type:** Regional
4. Press *Create API*
5. On actions press "Create Resource"
   - Type on **Resource Name:** "hello"
   - Press *Create Resource*
6. On actions press "Create Method"
   - Create methods for **GET, POST & PUT**
   - All three methods, select the following requirements:
     - **Integration type:** Lambda Function
     - **Lambda Function:** test
7. Press *Save*
8. Select **GET** method
   - Press on "Integration Request"
   - Press on "Mapping templates"
   - **Content-Type:** application/json
   - Press "Yes, secure this integration"
   - **Generate template:** Method Request passthrough.

    Of this way all the content coming through the API is forwarded to Lambda function.

### Setup Kinesis 
1. Open <ins>Kinesis</ins> on AWS
2. Select "Create data stream" on Kinesis Data Stream
3. Fill the requeriments
   - **Data Stream name:** "APIdata"
   - **Capacity Mode**: Provisioned
   - Press "Create Data Stream"

### Setup IAM for API
Now, setting up IAM will make sure that Lambda function delivers data into Kinesis.
1. Open <ins>Lambda</ins> on AWS
2. Go to "test" function
3. Press on "Configuration" and then in "Permissions"
4. Inside "Edit" after the "Existing role" press in "View the *rolename*"
5. Go to "Policies" and create a policy
6. Fill the requeriments
   - **Service:** Kinesis
   - **Actions:** PutRecord & PutRecords
   - **Resources:** All resources
   - **Name:** "MyKinesisWriteAPIData"
   - Create policy
7. Go to Roles and inside "test-role" add the recently created policy

