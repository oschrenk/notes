# Azure

Azure CLI

```
# install
brew install azure-cli

azure login
# follow the instructions on screen

# configure arm mode
azure config mode arm

# Figure out which subscription is the right one
azure account list

# Switch to that one
azure account set <id>

# Now you can interact with it
# ---------------------------

# list resource groups
azure group list

# list Network security groups
azure network nsg list <resource group>

# create a new rule for group
azure network nsg rule create <resource group> <network security group> \
testName \
--description 'testDesc' \
--protocol 'Tcp' \
--source-address-prefix '10.10.10.10/32' \
--source-port-range '*' \
--destination-address-prefix '*' \
--destination-port-range '443' \
--access 'Allow' \
--priority '4000' \
--direction 'Inbound'
```
