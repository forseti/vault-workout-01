### 03_managing_secrets_engine ###

Differences between Secrets Engine's Versions:

| Version 1 | Version 2 |
|---|---|
| No versioning, last key wins | Versioning of past secrets |
| Faster with fewer storage calls | Possibly less performant |
| Deleted items are gone | Deleted items and metadata retained |
| Can be upgraded to Version 2 | Cannot be downgraded |
| Default version on creation | Can be specified at creation |


#### Enable a secrets engine ####

##### Commands #####

CLI:
```console
vault secrets enable -path=<STOREPATH> <TYPE>
```

CLI (with `-version=`, e.g. `2`:
```console
vault secrets enable -version=<VERSION> -path=<STOREPATH> <TYPE>
```

API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request POST --data @<JSONFILE> $VAULT_ADDR/v1/sys/mounts/<STOREPATH>
```

#### View the secrets engine paths ####

##### Commands #####

CLI:
```console
vault secrets list
```

CLI (with `-format=`, e.g. `json`):
```console
vault secrets list -format=<FORMAT>
```

API:
To view the secrets engine paths:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" $VAULT_ADDR/v1/sys/mounts
```

#### Add a secret (V1 only) ####

##### Commands #####

CLI:
```console
vault write <STOREPATH>/<KEYNAME> <KEY>=<VALUE>( <KEY>=<VALUE> ...)
```

#### Read a secret (V1 only) ####

##### Commands #####

CLI:
```console
vault read <STOREPATH>/<KEYNAME>
```

#### Move the secrets engine path ####

##### Commands #####

CLI:
```console
vault secrets move <SOURCE> <DESTINATION>
```

API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request POST --data @<JSONFILE> $VAULT_ADDR/v1/sys/remount
```

#### Upgrade the secrets engine to V2 ####

##### Commands #####

CLI:
```console
vault kv enable-versioning <STOREPATH>
```

API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request POST --data @<JSONFILE> $VAULT_ADDR/v1/sys/mounts/<STOREPATH>/tune
```

#### Disable a secrets engine ####

##### Commands #####

CLI:
```console
vault secrets disable <STOREPATH>
```

API:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request DELETE $VAULT_ADDR/v1/sys/mounts/<STOREPATH>
```

#### Tune a secrets engine ####

##### Commands #####
CLI:
```console
vault secrets tune (<OPTION> ...) <PATH>
```



#### Demo ####

###### Enable a secret engine ######

Using CLI with `-path=pirate-a` and `kv` for `<TYPE>`:
```console
vault secrets enable -path=pirate-a kv
```

Using API with `pirate-b.json` as `<JSONFILE>` and `pirate-b` as `<PATH>`:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request POST --data @pirate-b.json $VAULT_ADDR/v1/sys/mounts/pirate-b
```

Using CLI to list the secrets:
```console
vault secrets list
```

Using API to list the secrets:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" $VAULT_ADDR/v1/sys/mounts
```

Using CLI to add new secrets to secrets engine path:
```console
vault kv put pirate-a/luffy navigator=nami swordsman=zoro
```

Using CLI to get new secrets from the secrets engine path:
```console
vault kv get pirate-a/luffy
```

Using CLI command `write` (V1 only) to update the secrets (and remove the previous ones):
```console
vault write pirate-a/luffy doctor=chopper
```

Using CLI to get the secrets:
```console
vault kv get pirate-a/luffy
```

Using CLI to move the secrets engine path:
```console
vault secrets move pirate-a pirate-a-moved
```

Using API to move the secrets engine path:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request POST --data @pirate-b-moved.json $VAULT_ADDR/v1/sys/remount
```

Using CLI to list the secrets on their new paths:
```console
vault secrets list
```

Using CLI command `read` (V1 only) to get the secrets from the new path:
```console
vault read pirate-a-moved/luffy
```

Using CLI to upgrade the secrets engine to V2:
```console
vault kv enable-versioning pirate-a-moved
```

Using API to upgrade the secrets engine to V2:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request POST --data @pirate-b-moved.v2.json $VAULT_ADDR/v1/sys/mounts/pirate-b-moved/tune
```

Using API to check the secrets engine if it is in V2:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" $VAULT_ADDR/v1/sys/mounts/pirate-b-moved/tune
```

Using CLI to create new secrets engine with V2:
```console
vault secrets enable -version=2 -path=pirate-c kv
```

Using CLI to disable the secrets engine:
```console
vault secrets disable pirate-a-moved
```

Using API to disable the secrets engine:
```console
curl --header "X-Vault-Token: $VAULT_TOKEN" --request DELETE $VAULT_ADDR/v1/sys/mounts/pirate-b-moved
```