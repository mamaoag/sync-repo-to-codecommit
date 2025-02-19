# Sync up to AWS CodeCommit Action

Synchronize from GitHub repository to AWS CodeCommit via GitHub Actions.  
No need to ssh-private-key. Need to AWS IAM Credentials only.

## Example usage
1. Standard way of configuring it.
```yaml
name: sync up to codecommit

on:
  push:
    tags-ignore:
      - '*'
    branches:
      - '*'

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.TEST_AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.TEST_AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1

      - name: Sync up to CodeCommit
        uses: youyo/sync-up-to-codecommit-action@v1
        with:
          repository_name: test_repo
          aws_region: us-east-1
```
2. Using GitHub OpenID
```yaml
name: sync up to codecommit

on:
  push:
    tags-ignore:
      - '*'
    branches:
      - '*'

jobs:
  sync:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1.7.0
        with:
          aws-region: us-east-1
          role-to-assume: ${{ secrets.TEST_AWS_IAM_ROLE_ARN }}
          role-duration-seconds: 900
          role-session-name: GitHubActions-${{ github.run_id }}

      - name: Sync up to CodeCommit
        uses: youyo/sync-up-to-codecommit-action@v1
        with:
          repository_name: test_repo
          aws_region: us-east-1
```

## Inputs

- `repository_name` **Required** CodeCommit repository name.
- `aws_region` **Required** Region of the CodeCommit repository.

## License

[MIT](LICENSE)

## Author

[youyo](https://github.com/youyo)
