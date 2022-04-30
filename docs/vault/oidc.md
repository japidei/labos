Aqui tienes la guia:

https://www.vaultproject.io/docs/auth/jwt

Lo habilitamos:


    vault auth enable oidc

Configuramos en el proveedor OIDC la aplicación con las siguientes Redirect URIs:

https://www.vaultproject.io/docs/auth/jwt/oidc_providers#gitlab

    http://localhost:8250/oidc/callback
    https://vault.japi:8200/ui/vault/auth/oidc/oidc/callback

Configuramos el método de auth:

    vault write \
        auth/oidc/config \
        oidc_discovery_url="https://gitlab.kolokium.com" \
        oidc_client_id="APPLICATION_ID" \
        oidc_client_secret="SECRET" \
        default_role="demo" \
        bound_issuer=“localhost”

Creamos el role:

    $ vault write auth/oidc/role/demo -<<EOF
    {
    "user_claim": "sub",
    "allowed_redirect_uris": "http://localhost:8250/oidc/callback,https://vault.japi:8200/ui/vault/auth/oidc/oidc/callback",
    "bound_audiences": "cca3b951238a75254fea5d7624ebdf64898f93c82de27eba39b01fdc7a2b5a0b",
    "oidc_scopes": "openid",
    "role_type": "oidc",
    "policies": "admin",
    "ttl": "1h"
    }
    EOF
