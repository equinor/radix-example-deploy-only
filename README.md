# radix-example-deploy-only
A demonstration of automated deploy-only workflow in Radix

- Building is done by github actions
- Deploy is done by Radix through CLI command

The deploy strategy used here is limited to a single static tag `latest`. This require no changes to the `radixconfig.yaml` file but will also not cause existing deployments to update their image when new builds are pushed.


## Workflow sequence

- Commit is pushed to this Github source repository
- Github runs CI-CD job defined in `.github/workflows/deploy.yaml`
- Job runs `docker build` command
- Docker reads `helloworld/Dockerfile` and follows its instructions
  - Docker hosts an image with go compiler installed and builds an executable
  - Docker creates a new alpine linux image with the executable copied to it
- Job pushes docker image to Github release repository
- Job calls Radix CLI `deploy` command to idempotently deploy the image to an environment
- Job calls Radix CLI `restart` command to make sure current deployment pulls the latest image