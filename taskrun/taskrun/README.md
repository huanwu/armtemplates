# Task run

## Create a resource group

```bash
az group create \
  -n mytaskrunrg \
  -l westus
```

## Crate a Registry

```bash
az acr create \
   -n myreg -g mytaskrunrg --sku Standard
```

## Crate a Task

```bash
az acr task create \
   -t hello-world:{{.Run.ID}} -n hello-world -r mytaskrunrg \
   -c https://github.com/Azure-Samples/acr-build-helloworld-node.git \
   -f Dockerfile --commit-trigger-enabled false --pull-request-trigger-enabled false
```

## Deploy a task run
Fill the right taskname and taskid in azuredeploy.json

```bash
az group deployment create \
  --resource-group "mytaskrunrg" --template-file azuredeploy.json --parameters azuredeploy.parameters.json \
  --parameters registryName="mytaskrunrg" --parameters taskRunName="mytaskname"
```

