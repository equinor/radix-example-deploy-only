# radix-example-deploy-only

A demonstration of automated deploy-only workflow in Radix

![CICD](https://github.com/equinor/radix-example-deploy-only/workflows/CICD/badge.svg)

- Building is done by github actions
- Deploy is done by Radix through CLI command

The deploy strategy used here is limited to a single static tag `latest`. This require no changes to the `radixconfig.yaml` file to deploy new builds and will require a flag `alwaysPullImageOnDeploy` to always pull new image when deploy is requested.

## Workflow sequence

- Commit is pushed to this Github source repository
- Github runs CI-CD job defined in `.github/workflows/deploy.yaml`
- Job runs `docker build` command
- Docker reads `helloworld/Dockerfile` and follows its instructions
  - Docker hosts an image with go compiler installed and builds an executable
  - Docker creates a new alpine linux image with the executable copied to it
- Job pushes docker image to Github release repository
- Job calls Radix CLI `deploy` command to idempotently deploy the image to an environment
