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

admin-username : azureuser

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

az vmss extension set \
  --publisher Microsoft.Azure.Extensions \
  --version 2.0 \
  --name CustomScript \
  --resource-group learn \
  --vmss-name webServerScaleSet \
  --settings @vmss_script_extension.json

az vmss show \
    --name webServerScaleSet \
    --resource-group learn \
    --query upgradePolicy.mode

az vmss extension set \
    --publisher Microsoft.Azure.Extensions \
    --version 2.0 \
    --name CustomScript \
    --vmss-name webServerScaleSet \
    --resource-group learn \
    --settings "{\"commandToExecute\": \"echo This is the updated app installed on the Virtual Machine Scale Set ! > /var/www/html/index.html\"}"



ssh -i ~/.ssh/id_rsa azureuser@23.97.245.144 -p 50000

sudo find / -type f -not -type d -iname "CustomScript"

