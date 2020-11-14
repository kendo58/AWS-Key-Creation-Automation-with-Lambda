import os
import boto3

kms = boto3.client('kms')
ssm = boto3.client('ssm')
Desc = 'Development key108'
usage = 'ENCRYPT_DECRYPT'
Alias_Name = 'alias/projectKey108'

def lambda_handler(event, context):
    
    def get_parameters():
        global key_policy
        try:
            response = ssm.get_parameters(
                Names=['keypolicy'],WithDecryption=False
            )
            for key, value in response.items():
                key_policy = value[0].get('Value')
        except IndexError:
            print('success')
    get_parameters()

    key = kms.create_key(
       Description=Desc,
       KeyUsage=usage
    )
    key_id = key["KeyMetadata"]["KeyId"]

    alias = kms.create_alias(
        AliasName=Alias_Name,
        TargetKeyId=str(key_id)
        )
        
    keypolicy = kms.put_key_policy(
        KeyId=key_id,
        PolicyName='default',
        Policy=key_policy
        )
