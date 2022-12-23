# deploy-mkdocs-composite-action
reusable github action to deploy mkdocs site 

## features
- configures AWS credentials
- gets mkdocs site archive
- deploys mkdocs site

## usage
*__note:__* Secrets for composite actions must be configured using an [`environment`](https://docs.github.com/en/actions/using-jobs/using-environments-for-jobs).  The `ci` environment is used in the following example.  It contains the secrets `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.

```
todo
```

## inputs
```
  version:
    description: 'Build version to deploy.  Used to get archive at unique S3 key.  Ex: sha-####, 3.0.0'
    required: true
    type: string
  env-name:
    description: 'Environment name.  Ex: dev, prod'
    default: 'dev'
    required: true
    type: string
  aws-region:
    description: 'AWS Region.  Ex: us-east-1'
    default: 'us-east-1'
    required: true
    type: string                      
  aws-access-key-id:
    description: 'AWS Access Key ID.'
    required: true
    type: string
  aws-secret-access-key:
    description: 'AWS Secret Access Key.'
    required: true
    type: string
```

## prerequisites
expects Makefile in mkdocs project with the following directives:
```
get-mkdocs-archive: env region version
deploy-mkdocs: env region
```
### get-mkdocs-archive
use this directive to retrieve and unpack mkdocs artifact (ex: download archive from S3 and unzip to ./site/ folder)

### deploy-mkdocs
use this directive to deploy mkdocs site (ex: copy ./site/ files to S3)
