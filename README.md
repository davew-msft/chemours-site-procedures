Private site for chemours-site-procedures NLP solutions.  Callable via azfuncs.

Dev Notes:
* Open folder with vscode using WSL
* interpreter is 3.7.3 64 bit


This is the setup for azfuncs

```bash
ACR=chemoursdavew.azurecr.io
ACR_SHORT=chemoursdavew
RESOURCE_GROUP=rgChemours
APP_NAME=

cd azfuncs
func init IllogicalStepSequence --docker
cd IllogicalStepSequence
func new --name IllogicalStepSequence --template "HttpTrigger"

func start

# now build the docker container
docker build \
    --tag $ACR/illogicalstepsequence:v0.0.1 \
    .


docker run \
    -p 7081:80 \
    -it \
    $ACR/illogicalstepsequence:v0.0.1

# http://localhost:7081/
# http://localhost:7081/api/IllogicalStepSequence

az acr login --name $ACR_SHORT
docker push $ACR/illogicalstepsequence:v0.0.1


az functionapp config container show --resource-group $RESOURCE_GROUP --name $APP_NAME
```