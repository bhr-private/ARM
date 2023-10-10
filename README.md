# ARM STORAGE ACCOUNT

https://learn.microsoft.com/en-us/training/modules/create-azure-resource-manager-template-vs-code/1-introduction

templateFile="Azure_ARM_Storage_deploy.json"
today=$(date +"%d-%b-%Y")
DeploymentName="addSkuParameter-"$today

az deployment group create \
  --name $DeploymentName \
  --template-file $templateFile \
  --parameters storageSKU=Standard_GRS storageName=storage10102023
