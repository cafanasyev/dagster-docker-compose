### Refactor of the dagster official docker-compose deployment

The original source:
* https://docs.dagster.io/deployment/oss/deployment-options/docker

The full example:
* https://github.com/dagster-io/dagster/tree/master/examples/deploy_docker


**GOALS**
1. Create reusable sample-project (template) with docker-compose deployment only (no actual Python code).
2. Move hardcoded variables to the .env.example file so it's clear what variables are required.

**REQUIREMENTS**
1. Apply as minimal as possible changes to the original docker-compose deployment at best effort. It must be easy for anybody to look at the original document and understand differences.
2. Comments must be short and concise as much as possible.

