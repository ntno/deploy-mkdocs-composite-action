# deploy-mkdocs-composite-action
reusable github action to deploy mkdocs site to S3

## features
- configures AWS credentials
- gets mkdocs site archive
- deploys mkdocs site

## usage
*__note:__* Secrets for composite actions must be configured using an [`environment`](https://docs.github.com/en/actions/using-jobs/using-environments-for-jobs).  The `ci` environment is used in the following example.  It contains the secrets `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY`.

```
jobs:
  deploy-pr:
    runs-on: ubuntu-latest
    environment: ci
    steps:
      - name: Checkout
        uses: actions/checkout@v3   
      - name: Deploy PR
        id: deploy-mkdocs
        uses: ntno/deploy-mkdocs-composite-action@v1
        with:         
          version: 1.0.3-pr
          env-name: prod
          s3-bucket: my-s3-bucket-1.0.3-pr
          aws-region: us-east-2
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
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
  s3-bucket:
    description: 'S3 Bucket to deploy to.'
    required: true
    type: string    
  aws-region:
    description: 'AWS Region.  Ex: us-east-1'
    default: 'us-east-1'
    required: true
    type: string                      
  aws-access-key-id:
    description: 'AWS Access Key ID.'
    required: false
  aws-secret-access-key:
    description: 'AWS Secret Access Key.'
    required: false
  role-to-assume:
    description: >-
      Use the provided credentials to assume an IAM role and configure the Actions
      environment with the assumed role credentials rather than with the provided
      credentials
    required: false
  role-duration-seconds:
    description: "Role duration in seconds (default: 6 hours, 1 hour for OIDC/specified aws-session-token)"
    required: false
  role-session-name:
    description: 'Role session name (default: GitHubActions)'
    required: false
  role-external-id:
    description: 'The external ID of the role to assume'
    required: false
  role-skip-session-tagging:
    description: 'Skip session tagging during role assumption'
    required: false
```

## prerequisites
expects Makefile in mkdocs project with the following directives:
```
get-mkdocs-archive: env region version download-directory
deploy-mkdocs: env region bucket-name
```
### get-mkdocs-archive
use this directive to retrieve and unpack mkdocs artifact (ex: download archive from S3 and unzip)

### deploy-mkdocs
use this directive to deploy mkdocs site to S3 bucket
