# Generar certificados TLS

https://www.vaultproject.io/docs/secrets/pki

Primero habilitamos el secret:

    vault secrets enable pki

Generamos el certificado raiz para dominios *.japi:

    vault write \
        pki/root/generate/internal
        not_after="9999-12-31T23:59:59Z"
        common_name=japi
        -format=json
    | tee generate.json

Generamos el certificado raiz para dominios *.japi:

    vault write \
        pki/root/generate/internal
        not_after="9999-12-31T23:59:59Z"
        common_name=japi
        -format=json
    | tee generate.json

Ejemplo de la generación de un certificado:

    vault write \
        pki/issue/japi \
        ttl=8760h \
        common_name=vault.japi \
        alt_names=mini.japi \
        ip_sans=127.0.0.1,192.168.1.3 \
    -format=json \
    | tee tls.json
