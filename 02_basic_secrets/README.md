### 02_basic_secrets_mgmt ###

#### Add a secret to KV store ####

##### Commands #####

CLI:
```console
vault kv put <STOREPATH>/<KEYNAME> <KEY>=<VALUE>( <KEY>=<VALUE> ...)
```

API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request POST --data @<JSONFILE> $VAULT_ADDR/v1/secret/data/<KEYNAME>
```


#### Retrieve a secret from KV store ####

##### Commands #####

CLI:
```console
vault kv get <STOREPATH>/<KEYNAME>
```

API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" $VAULT_ADDR/v1/secret/data/<KEYNAME>
```

#### Update a secret in KV store ####

##### Commands #####

CLI:
```console
vault kv put <STOREPATH>/<KEYNAME> <KEY>=<VALUE>( <KEY>=<VALUE> ...)
```

#### Delete a secret ####

##### Commands #####

CLI:
```console
vault kv delete <STOREPATH>/<KEYNAME>
```

API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request DELETE $VAULT_ADDR/v1/secret/data/<KEYNAME>
```

#### Demo ####

###### Create a secret ######

Using CLI:
```console
vault kv put secret/strawhat luffy=captain
```

Using API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request POST --data @luffy.json $VAULT_ADDR/v1/secret/data/luffy
```

###### Retrieve a secret ######

Using CLI:
```console
vault kv get secret/strawhat
```

Using API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" $VAULT_ADDR/v1/secret/data/luffy
```

###### Update a secret ######

Using CLI:
```console
vault kv put secret/strawhat luffy=captain nami=navigator
```

###### Delete a secret ######

Using CLI:
```console
vault kv delete secret/strawhat
```

Using API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request DELETE $VAULT_ADDR/v1/secret/data/luffy
```