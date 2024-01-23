# Criar uma VM e embutir o NGINX

Criar a VM com Azure Cli:
```
az vm create \
  --resource-group lab01-rg \
  --name mv01-vm \
  --public-ip-sku Standard \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --generate-ssh-keys   
```

Com o comando az vm extension set consegue-se configurar o nginx na própria VM:
```
az vm extension set \
  --resource-group lab01-rg \
  --vm-name mv01-vm \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --version 2.1 \
  --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
```

Para saber mais informação sonbre estes scriptes consultar secção dos créditos.

# Créditos
https://learn.microsoft.com/en-us/training/modules/describe-azure-compute-networking-services/3-exercise-create-azure-virtual-machine

