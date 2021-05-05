Private site for chemours-site-procedures NLP solutions.  Callable via azfuncs.

Dev Notes:
* Open folder with vscode using WSL
* interpreter is 3.7.3 64 bit


This is the setup for azfuncs

```bash
ACR=chemoursdavew.azurecr.io
ACR_SHORT=chemoursdavew
RESOURCE_GROUP=rgChemours
APP_NAME=chemours

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

# show which c ontainer version is in use
az functionapp config container show --resource-group $RESOURCE_GROUP --name $APP_NAME

# change the deployed container
az functionapp config container set \
  --docker-custom-image-name $ACR/illogicalstepsequence:v0.0.1 \
  --resource-group $RESOURCE_GROUP \
  --name $APP_NAME

# if you look in the portal you should see the newly deployed function and can retrieve the url for testing.  
# DefaultEndpointsProtocol=https;AccountName=storageaccountrgche94e1;AccountKey=+/OAGoy8vhfsvr+k4LvFGNID/qA8Iz7f2EUoSa4xYQSEYd9KuDm2Ig/xQJbZ+AcuK/WzpGSh3CpQKYcBTHXCFQ==;EndpointSuffix=core.windows.net

```