```xml
import boto3

service_name = 's3'
endpoint_url = 'https://kr.object.ncloudstorage.com'
region_name = 'kr-standard'
access_key = '{ACCESS_KEY_ID}'
secret_key = '{SECRET_KEY}'

if __name__ == "__main__":
    s3 = boto3.client(service_name, endpoint_url=endpoint_url,
                         aws_access_key_id=access_key, aws_secret_access_key=secret_key)

    bucket_name = '{버킷명}'

# Define the configuration rules
    cors_configuration = {
        'CORSRules': [{
            'AllowedHeaders': ['*'],
            'AllowedMethods': ['GET', 'PUT'],
            'AllowedOrigins': ['http://cors-test.com'],
            'MaxAgeSeconds': 3000
        }]
    }

    # Set CORS configuration
    response = s3.put_bucket_cors(Bucket=bucket_name, CORSConfiguration=cors_configuration)
    print(response['ResponseMetadata'])

    # Get CORS configuration
    response = s3.get_bucket_cors(Bucket=bucket_name)
    print(response['CORSRules'])
    ```