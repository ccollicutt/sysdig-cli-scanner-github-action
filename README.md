# Sysdig CLI Scanner Action

This GitHub action runs the Sysdig CLI Scanner on your code.

## Inputs

| Input              | Description                       | Required |
|--------------------|-----------------------------------|----------|
| `sysdig-api-key`   | Your Sysdig Secure API Key        | Yes      |
| `sysdig-api-url`   | Your Sysdig Secure API URL        | Yes      |
| `image-name`       | The name of the image to scan     | Yes      |
| `sysdig-policy-name` | The policy name for Sysdig CLI Scanner | No   |


## Example Configuration

Use:

```
ccollicutt/sysdig-cli-scanner-github-action@main
```

Example workflow:

```
jobs:
  build_and_scan:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout repository
      uses: actions/checkout@v3

    - name: Set up Docker Buildx
      uses: docker/setup-buildx-action@v2

    - name: Build and push Docker image
      uses: docker/build-push-action@v2
      with:
        context: .
        # to add it to the local docker instance so that it can be found by
        # the sysdig cli scanner action set load to true
        load: true
        # but we are only building the image
        push: false
        tags: ${{ vars.IMAGE_NAME }}:latest

    - name: Run Sysdig CLI Scanner Action
      uses: ccollicutt/sysdig-cli-scanner-github-action@main
      with:
        sysdig-api-key: ${{ secrets.SECURE_API_TOKEN }}
        sysdig-api-url: ${{ secrets.SECURE_API_URL }}
        image-name: ${{ vars.IMAGE_NAME }}:latest
        sysdig-policy-name: ${{ vars.SYSDIG_POLICY_NAME }}
```