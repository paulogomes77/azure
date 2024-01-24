# Criar uma VM e abrir o porto 80 para acesso inbound.

Antes de prosseguir, vamos criar a VM conforme descrito em: 

https://github.com/paulogomes77/azure/blob/main/create-vm-with-nginx.md.

Verificar se a VM existe:

```
az vm list
```


1. Colocar o endereço IP numa variavel e mostra-lo no ecrã:
```
IPADDRESS="$(az vm list-ip-addresses \
  --resource-group lab01-rg \
  --name mv01-vm \
  --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" \
  --output tsv)"

echo $IPADDRESS
```


2. Testar o acesso
```
curl --connect-timeout 5 http://$IPADDRESS
```


3. Verificar qual o nome do Network Security Group (NSG) associado à nossa VM:
```
az network nsg list \
  --resource-group lab01-rg \
  --query '[].name' \
  --output tsv   
```

Resultado do comando:
```
mv01-vmNSG
``` 

Nota: No Azure todas as VM estão associadas a pelo menos um NSG.


4. Listar as NSG rules filtrando apenas o nome, prioridade, portos e tipo de acessos para cada regra:
```
az network nsg rule list \
  --resource-group lab01-rg \
  --nsg-name mv01-vmNSG \
  --query '[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}' \
  --output table
```

Resultado do comando:
```
Name              Priority    Port    Access
-----------------  ----------  ------  --------
default-allow-ssh  1000        22      Allow
```


5. Criar a Network Security Rule
   
```
az network nsg rule create \
  --resource-group lab01-rg \
  --nsg-name mv01-vmNSG \
  --name allow-http \
  --protocol tcp \
  --priority 100 \
  --destination-port-range 80 \
  --access Allow
```


6. Verificar a as regras em vigor

```
az network nsg rule list \
  --resource-group lab01-rg \
  --nsg-name my-vmNSG \
  --query '[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}' \
  --output table  
```

Resultado do comando:
```
Copy
Name              Priority    Port    Access
-----------------  ----------  ------  --------
default-allow-ssh  1000        22      Allow
allow-http          100        80      Allow   
```

7. Testar o acesso.
```
curl --connect-timeout 5 http://$IPADDRESS
```
Podem-se também testar com um navegador de Internet.



# Créditos
https://learn.microsoft.com/en-us/training/modules/describe-azure-compute-networking-services/9-exercise-configure-network-access


