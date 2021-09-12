# cryptoBox

## A format to store encrypted data. 

This format is composed of 2 parts the cryptoBox and at least one cryptoBoxProtector. 


Full Format explained:
```
{
   "data":"encrypted data goes here generally as a base64 encoding, but not limited to it.",
   "metaData":"Any extra infromation like version numbers or any other information that the sircustances require. IMPORTANT this field is not encrypted.",
   "protectOptions":"AES iv value, extra values depending on the cypher.",
   "signature":"data field signed by author of data, used to ensure inmutability and owners identity.",
   "signatureOwner":"user id, generally we would use an email",
   "protectType":"this identifies the block cypher used and the key length.",
   "protectors":[
      {
         "username":"a user identifier (could be email) that can decrypt this protector.",
         "type":"type of protector",
         "encryptedKey":"encrypted key using the users encryption type used above."
      }
   ]
}
```


The main part of a *cryptoBox* contains the encrypted *data* using the encryption algorithm, mode and key length defined in *protectorType*, this is signed *signature* by the author identified *signtureOwner*. For some block cyphers extra data is needed for this we use *protectOptions* so for example in the case of AES *iv* if we use something like PBKDF2 we may need *Algorithm*, *salt*, *itirations*, *key length* and so on, future cyphers may have different extra fields requirements.  
The *protectors* section holds the key that can decrypt the data holded by the main part of the *cryptoBox*, this key will be encrypted for each user that can decrypt the data. 

Some examples: 
```
{
    "data": "5tggdYWbwR3hZ15euUiV3PpAkIUhQ2Ds3FJWiVDbB/aKjg1UPO7xvM+PhRQaZ3PKhI72KA1ody9KBW9MlLXRutLnxfIXcFyJQ40=",
    "metaData": "",
    "protectOptions": {
        "iv": "tLnxfIXcFyJQ40"
    },
    "signature": "h/D15heCgWFnU87GLH1g2IVLOMvSctEJ6mc0Tc0kTkGhcYzh4Uk0Os8rDKctPtamTegSoeiAUNXbMTkUE7nQJPmei1vUp2AOUoYVV1KkhA+QUZNej+SIKVd/gLJuSAjkAMG4QNiEYmPRz+AyZaN++hHME8ZDWpwi9leja9sKr5KVirddnV9HXGvRL7MqjrreO46bEIHgt6iCvxSrmwg4fsQSo7WfItfamd6OV3mzbYJ7UMLq/lLTYIbtfxdnCuxuikdzcNkUEdl6rW2DdJNLVBF2Qzq65ZtwPiD/wwYi2KDKlnKA+9wpI8YtIsPDWJoLRqQZ5uyTpZQjiht9hn/k5g==",
    "signatureOwner": "doe@gmail.com",
    "protectType": "AES-GCM-256",
    "protectors": [
        {
            "username": "3f1d68b19b8e4c39981256101c5953b0d0a810c4de79d92d2839fdddbf8c1206",
            "type": "assymetric",
            "typeOptions": "",
            "encryptedKey": "U29tZSBkYXRhZGF0YWRhdGFkYXRhZGF0YWRhdGFkYXRh"
        },
        {
            "username": "smith@gmail.com",
            "type": "assymetric",
            "encryptedKey": "uMz97K2AtFygW8FxILvq42pjauH4o6Nc0PSgY84+lpacyFPR05AhrrQAnWQyMG1Q+g7B7E4gNtSgLphwlHJMQgEFA2CbdxqSeTkGTtlllh0LLxpKhH3zpdjNquMhuV9Z9sV6krcnllF3TiI8aSdkTZepKdSZ8Kd/DMsZX9U2EoEkiy3SfWgOEQK+RUhoML9YG92NsbQ6GtnYmWpQHuSY0KwnuuagsH/dVK5NfgftcYmXidoHYfN/H1qC0V8f2YmfmngJJcnwTkBJSRiTxPrhrWpoD3lg+ae8FlBhpxlDR92CwvCr7ERvgNTuQtx2NxqWXAS4jD18zhRsil+E9YOxAQ=="
        },
        {
            "username": "3f1d68b19b8e4c39981256101c5953b0d0a810c4de79d92d2839fdddbf8c1206",
            "encryptedKey": "6G5rZROEKbNZ76lv8JIL+OGUfomVZJmMoQOZHfR52zNcrIfQ/pKkOA==",
            "type": "password",
            "typeOptions": {
                "name": "PBKDF2",
                "salt": "nfvZf7nJUIwdJouyrG+d0A==",
                "iterations": 10000,
                "hash": "SHA-256"
            }
        },
        {
            "username": "3f1d68b19b8e4c39981256101c5953b0d0a810c4de79d92d2839fdddbf8c1206",
            "encryptedKey": "6G5rZROEKbNZ76lv8JIL+OGUfomVZJmMoQOZHfR52zNcrIfQ/pKkOA==",
            "type": "plain-text",
            "typeOptions": ""
        }
    ]
}
```



