# Criar uma VM e embutir o NGINX via az cli

1. Criar a VM com Azure Cli:
```
az vm create \
  --resource-group lab01-rg \
  --name mv01-vm \
  --public-ip-sku Standard \
  --image Ubuntu2204 \
  --admin-username azureuser \
  --generate-ssh-keys   
```


2. Com o comando az vm extension set consegue-se configurar o nginx na própria VM:
```
az vm extension set \
  --resource-group lab01-rg \
  --vm-name mv01-vm \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --version 2.1 \
  --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
```


Conteúdo do script configure-nginx.sh:
```
#!/bin/bash

# Update apt cache.
sudo apt-get update

# Install Nginx.
sudo apt-get install -y nginx

# Set the home page.
echo "<html><body><h2>Welcome to Azure! My name is $(hostname).</h2></body></html>" | sudo tee -a /var/www/html/index.html
```

Este lab foi retirado do Microsoft Learning. Para mais informação consultar a secção dos créditos.

# Créditos
https://learn.microsoft.com/en-us/training/modules/describe-azure-compute-networking-services/3-exercise-create-azure-virtual-machine

