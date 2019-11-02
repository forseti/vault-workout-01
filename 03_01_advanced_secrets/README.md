### 03_01_advanced_secrets_mgmt ###

#### Retrieve a secret from KV store ####

##### Commands #####

CLI (with `-format=`, e.g. `json`):
```console
vault kv get -format=<FORMAT> <STOREPATH>/<KEYNAME>
```

CLI (with `-version=`):
```console
vault kv get -version=<VERSION> <STOREPATH>/<KEYNAME>
```

API (with `?version=`):
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" $VAULT_ADDR/v1/secret/data/<KEYNAME>?version=<VERSION>
```

#### Undelete a secret (or more) in KV store ####

##### Commands #####

CLI:
```console
vault kv undelete -versions=<VERSION>(,<VERSION> ...) <STOREPATH>/<KEYNAME>
```

API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request POST $VAULT_ADDR/v1/secret/undelete/<KEYNAME> --data '{"versions": [<VERSION>(,<VERSION> ...)]}'
```

#### Destroy a secret (or more) in KV store ####

##### Commands #####

CLI:
```console
vault kv destroy -versions=<VERSION>(,<VERSION> ...) <STOREPATH>/<KEYNAME>
```

API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request POST $VAULT_ADDR/v1/secret/destroy/<KEYNAME> --data '{"versions": [<VERSION>(,<VERSION> ...)]}'
```

#### Delete a metadata ####

##### Commands #####

CLI:
```console
vault kv metadata delete <STOREPATH>/<KEYNAME>
```

API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request DELETE $VAULT_ADDR/v1/secret/metadata/<KEYNAME>
```

#### Demo ####

###### Create a secret ######

Using CLI:
```console
vault kv put secret/strawhat luffy=captain
```

###### Update the secret ######

Using CLI:
```console
vault kv put secret/strawhat luffy=captain nami=navigator
```

###### Retrieve the secret ######

Using CLI with `-format=json`:
```console
vault kv get -format=json secret/strawhat
```

Using CLI with `-version=1`:
```console
vault kv get -version=1 secret/strawhat
```

Using CLI with `-version=2`:
```console
vault kv get -version=2 secret/strawhat
```

Using API with `?version=1`:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" $VAULT_ADDR/v1/secret/data/strawhat?version=1
```

###### Delete the current secret (version 2) ######

Using CLI:
```console
vault kv delete secret/strawhat
``` 

Alternatively, with API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request DELETE $VAULT_ADDR/v1/secret/data/strawhat
```

###### Undelete a secret (version 2) ######

Using CLI with `-versions=2`:
```console
vault kv undelete -versions=2 secret/strawhat
```

Alternatively, using API with `--data '{"versions": [2]}'`
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request POST $VAULT_ADDR/v1/secret/undelete/strawhat --data '{"versions": [2]}'
```

###### Destroy the secrets (version 1 and version 2) ######

Using CLI with `-versions=1,2`
```console
vault kv destroy -versions=1,2 secret/strawhat
```

Using API with `--data '{"versions": [1,2]}`
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request POST $VAULT_ADDR/v1/secret/destroy/<KEYNAME> --data '{"versions": [1,2]}'
```

###### Delete the metadata ######

Using CLI:
```console
vault kv metadata delete secret/strawhat
```

Using API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request DELETE $VAULT_ADDR/v1/secret/metadata/strawhat
```