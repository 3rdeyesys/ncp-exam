```XML
<?php
    use Aws\S3\S3Client;
    use Aws\Exception\AwsException;

    $s3Client = new S3Client ([
        'endpoint' => 'https://kr.object.ncloudstorage.com',
        'region'   => 'kr-standard',
        'credentials' => array(
            'key'      => '{ACCESS_KEY_ID}',
            'secret'   => '{SECRET_KEY}'
        ),
        'version' => 'latest'
    ]);


    $bucketName = "{버킷명}";

    # Set CORS configuration
    try
    {
        $result = $s3Client->putBucketCors([
            'Bucket' => $bucketName, // REQUIRED
            'CORSConfiguration' => [ // REQUIRED
                'CORSRules' => [ // REQUIRED
                    [
                        'AllowedHeaders' => ['*'],
                        'AllowedMethods' => ['GET', 'PUT'], // REQUIRED
                        'AllowedOrigins' => ['http://cors-test.com'], // REQUIRED
                        'ExposeHeaders' => [],
                        'MaxAgeSeconds' => 3000
                    ],
                ],
            ]
        ]);

        var_dump($result["@metadata"]);
    }
    catch (AwsException $e)
    {   // output error message if fails
        error_log($e->getMessage());
    }

    echo("<hr />");

    # Get CORS configuration
    try
    {
        $result = $s3Client->getBucketCors([
            'Bucket' => $bucketName, // REQUIRED
        ]);

        var_dump($result["CORSRules"]);
    }
    catch (AwsException $e)
    {   // output error message if fails
        error_log($e->getMessage());
    }

?>
```