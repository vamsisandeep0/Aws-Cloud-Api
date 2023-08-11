# Aws-Cloud-Api
We are collecting the csv file from the api and storing in the S3 Bucket

Here we are calling api by triggering Lambda function and storing the file in S3 Bucket

Lambda Function Name : lambda-api
S3 Bucket Name : mycloud-api

Lambda Function Code :
___________________________________________________________________________________________________________________
import json
import urllib3
import boto3

s3 = boto3.client('s3')
def lambda_handler(event, context):
    # TODO implement
    bucket = 'mycloud-api'
    http = urllib3.PoolManager()
    r = http.request('GET', 'https://data.nasdaq.com/api/v3/datatables/ZILLOW/DATA?indicator_id=ZSFH&region_id=99999&api_key=__afpDhxkYvt5XZzfEB-')
    json_data = json.loads(r.data.decode('utf-8').replace("'", '"'))
    filename = 'sample.json'
    uploadByteStream = bytes(json.dumps(json_data).encode('UTF-8'))
    s3.put_object(Bucket=bucket, Key=filename, Body= json.dumps(json_data, indent=4, default=str))
    return {
        'statusCode': 200,
        'body': json_data
    }

________________________________________________________________________


