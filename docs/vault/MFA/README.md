# Configurar MFA

Elegimos la entidad a la que le queremos forzar el MFA:


    $ vault list identity/entity/name
    Keys
    ----
    alice
    entity_4f7b555b
    japi

Obtenemos el id de la entidad (ENTITY_ID):


    $ vault read identity/entity/name/alice
    Key                    Value
    ---                    -----
    aliases                [map[canonical_id:f88a7846-40ff-2d9a-5939-2df9ceaa4585 creation_time:2022-04-09T12:34:25.047356Z custom_metadata:<nil> id:0c5abb03-e95e-a3e0-cb02-d23e3c57eca0 last_update_time:2022-04-09T12:34:25.047356Z local:false merged_from_canonical_ids:<nil> metadata:<nil> mount_accessor:auth_userpass_c7fa2f53 mount_path:auth/userpass/ mount_type:userpass name:alice]]
    creation_time          2022-04-09T12:33:45.969261Z
    direct_group_ids       []
    disabled               false
    group_ids              []
    id                     f88a7846-40ff-2d9a-5939-2df9ceaa4585
    inherited_group_ids    []
    last_update_time       2022-04-09T12:33:45.969261Z
    merged_entity_ids      <nil>
    metadata               <nil>
    name                   alice
    namespace_id           root
    policies               []

Habilitamos el método MFA y obtenemos su id (METHOD_ID:


    $ vault write identity/mfa/method/totp issuer=vault
    Key          Value
    ---          -----
    method_id    540bda78-3719-ef80-226f-4090eb99fb8f

Creamos un login enforcement para un auth:


    $ vault write \
        identity/mfa/login-enforcement/learn \
        mfa_method_ids=540bda78-3719-ef80-226f-4090eb99fb8f \
        auth_method_accessors=auth_userpass_c7fa2f53
    Success! Data written to: identity/mfa/login-enforcement/learn
