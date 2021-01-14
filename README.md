# AWS S3 bucket

- S3 stands for simple storage service, and is an easy and low cost way to store things on the cloud. 

## Setting up awscli
- to be able to list the buckets run
```bash
sudo snap install aws-cli --classic
sudo apt install awscli
```
- Then you need to configure the aws credentials with
```bash
aws configure
```
- once the correct access key and secret key are entered we can list our buckets with 
```bash
aws s3api list-buckets
```

## creating a bucket with a python script
```python
import logging
import boto3
from botocore.exceptions import ClientError


def create_bucket(bucket_name, region=None):
    """Create an S3 bucket in a specified region

    If a region is not specified, the bucket is created in the S3 default
    region (us-east-1).

    :param bucket_name: Bucket to create
    :param region: String region to create bucket in, e.g., 'us-west-2'
    :return: True if bucket created, else False
    """

    # Create bucket
    try:
        if region is None:
            s3_client = boto3.client('s3')
            s3_client.create_bucket(Bucket=bucket_name)
        else:
            s3_client = boto3.client('s3', region_name=region)
            location = {'LocationConstraint': region}
            s3_client.create_bucket(Bucket=bucket_name,
                                    CreateBucketConfiguration=location)
    except ClientError as e:
        logging.error(e)
        return False
    return True
```

## list existing buckets with python script
```python
# Retrieve the list of existing buckets
s3 = boto3.client('s3')
response = s3.list_buckets()

# Output the bucket names
print('Existing buckets:')
for bucket in response['Buckets']:
    print(f'  {bucket["Name"]}')
```
[AWScli documentation](https://pypi.org/project/awscli/)
[amazon s3 bucket boto3 documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3-example-creating-buckets.html)