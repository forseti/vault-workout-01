### 01_setup ###

This section for setting up HashiCorp Vault's development environment

##### Run HashiCorp Vault #####
```console
vault server -dev
```

##### Set Environment Variables #####
```console
export VAULT_ADDR=http://127.0.0.1:8200
export VAULT_TOKEN=<VAULT_TOKEN>
```

##### Login with Vault Token #####
```console
vault login <VAULT_TOKEN>
```