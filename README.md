# ARM STORAGE ACCOUNT

https://learn.microsoft.com/en-us/training/modules/create-azure-resource-manager-template-vs-code/1-introduction

templateFile="Azure_ARM_Storage_deploy.json"
today=$(date +"%d-%b-%Y")
DeploymentName="addSkuParameter-"$today

az deployment group create \
  --name $DeploymentName \
  --template-file $templateFile \
  --parameters storageSKU=Standard_GRS storageName=storage10102023

# VM 

https://learn.microsoft.com/en-us/training/modules/build-app-with-scale-sets/1-introduction

az group create --location westeurope --name learn

az vmss create \
  --resource-group learn \
  --name webServerScaleSet \
  --image Ubuntu2204 \
  --upgrade-policy-mode automatic \
  --custom-data cloud-init.yaml \
  --admin-username azureuser \
  --generate-ssh-keys


az network lb probe create \
  --lb-name webServerScaleSetLB \
  --resource-group learn \
  --name webServerHealth \
  --port 80 \
  --protocol Http \
  --path /

az network lb rule create \
  --resource-group learn \
  --name webServerLoadBalancerRuleWeb \
  --lb-name webServerScaleSetLB \
  --probe-name webServerHealth \
  --backend-pool-name webServerScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp


  az vmss scale \
    --name webServerScaleSet \
    --resource-group learn \
    --new-capacity 4

