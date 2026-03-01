Here are the 20 most important Azure CLI commands for **API Management** in the AZ-204 exam:

---

## Instance Creation & Management

1. `az apim create --name <apim> --resource-group <rg> --publisher-email admin@example.com --publisher-name "My Org" --sku-name Consumption` Create an APIM instance. Know the SKU tiers: `Consumption` (serverless, no VNet), `Developer` (no SLA, testing only), `Basic`, `Standard`, `Premium` (multi-region). Consumption is most commonly tested for AZ-204.
    
2. `az apim show --name <apim> --resource-group <rg>` Show details of an APIM instance including gateway URL, state, and SKU.
    
3. `az apim update --name <apim> --resource-group <rg> --publisher-email new@example.com` Update APIM instance properties.
    
4. `az apim delete --name <apim> --resource-group <rg>` Delete an APIM instance.
    
5. `az apim check-name --name <apim>` Check whether an APIM service name is globally available before creating.
    

---

## APIs

6. `az apim api create --service-name <apim> --resource-group <rg> --api-id myapi --display-name "My API" --path /myapi --protocols https` Create a new API. Know that `--path` is the URL suffix appended to the gateway URL, and `--protocols` should typically be `https` only in production.
    
7. `az apim api import --service-name <apim> --resource-group <rg> --api-id myapi --path /myapi --specification-format OpenApi --specification-url https://example.com/openapi.json` Import an API from an OpenAPI spec. Also know `--specification-format` values: `OpenApi`, `OpenApiJson`, `Swagger`, `Wadl`, `Wsdl`. Heavily tested.
    
8. `az apim api show --service-name <apim> --resource-group <rg> --api-id myapi` Show details of a specific API.
    
9. `az apim api list --service-name <apim> --resource-group <rg>` List all APIs in an APIM instance.
    
10. `az apim api delete --service-name <apim> --resource-group <rg> --api-id myapi` Delete an API.
    

---

## API Operations

11. `az apim api operation create --service-name <apim> --resource-group <rg> --api-id myapi --operation-id getUsers --display-name "Get Users" --method GET --url-template /users` Create an operation on an API. Know the key fields: `--method` (GET, POST, PUT, DELETE), `--url-template` (relative path within the API).
    
12. `az apim api operation list --service-name <apim> --resource-group <rg> --api-id myapi` List all operations on a given API.
    

---

## Products

13. `az apim product create --service-name <apim> --resource-group <rg> --product-id myproduct --product-name "My Product" --state published --subscription-required true` Create a product. Know that products group APIs and control access via subscriptions. `--subscription-required true` means callers need a subscription key. `--state published` makes it visible to developers.
    
14. `az apim product api add --service-name <apim> --resource-group <rg> --product-id myproduct --api-id myapi` Add an API to a product. Commonly tested — APIs must be added to a product before developers can subscribe and call them.
    
15. `az apim product list --service-name <apim> --resource-group <rg>` List all products in an APIM instance.
    

---

## Named Values (formerly Properties)

16. `az apim nv create --service-name <apim> --resource-group <rg> --named-value-id mySecret --display-name "MySecret" --value "supersecret" --secret true` Create a named value. These are key-value pairs used in policies. `--secret true` masks the value. Know that named values can also reference Key Vault secrets instead of storing values directly.
    
17. `az apim nv update --service-name <apim> --resource-group <rg> --named-value-id mySecret --value "newsecret"` Update an existing named value.
    

---

## Subscriptions

18. `az apim subscription create --service-name <apim> --resource-group <rg> --sid mysub --display-name "My Subscription" --scope /apis/myapi` Create a subscription scoped to a specific API or product. Know the `--scope` formats: `/apis/<api-id>` (API-level), `/products/<product-id>` (product-level), `/` (all APIs).
    
19. `az apim subscription show --service-name <apim> --resource-group <rg> --sid mysub` Show subscription details including the primary and secondary keys.
    

---

## Backup & Restore

20. `az apim backup --name <apim> --resource-group <rg> --backup-name mybackup --storage-account-name <sa> --storage-account-container backups --storage-account-key <key>` Back up an APIM instance to blob storage. Know that backup/restore is not available on the Consumption tier — this is a common exam gotcha.

---

## Key Concepts the Exam Tests Beyond Commands

**Policies** are XML-based and applied at four scopes: Global → Product → API → Operation. The exam expects you to know common policy elements:

- `<rate-limit-by-key>` — throttle calls per key
- `<set-backend-service>` — redirect to a different backend
- `<cache-lookup>` / `<cache-store>` — response caching
- `<validate-jwt>` — validate a JWT token (Entra ID auth)
- `<inbound>`, `<backend>`, `<outbound>`, `<on-error>` — the four policy sections

**Subscription keys** are passed via `Ocp-Apim-Subscription-Key` header or `subscription-key` query parameter. Know both.

**Consumption tier limitations** come up repeatedly: no VNet integration, no backup/restore, no built-in cache, scales automatically, billed per call.