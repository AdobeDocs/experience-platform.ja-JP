---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リストIDマッピング
topic: API guide
translation-type: tm+mt
source-git-commit: df85ea955b7a308e6be1e2149fcdfb4224facc53
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 2%

---


# リストIDマッピング

マッピングとは、クラスター内の、指定した名前空間のすべてのIDの集まりです。

## 単一のIDに対するIDマッピングの取得

IDを指定し、リクエスト内のIDで表されるのと同じ名前空間ーからすべての関連IDを取得します。

**API形式**

```http
GET https://platform-{REGION}.adobe.io/data/core/identity/mapping
```

**リクエスト**

オプション1: IDを名前空間(`nsId`、ID)およびID値(`id`)として指定します。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/mapping?nsId=411&id=WTCpVgAAAFq14FMF' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

オプション2: IDを名前空間(名前`ns`別)およびID値(`id`)として指定します。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/mapping?ns=AMO&id=WTCpVgAAAFq14FMF' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

オプション3: IDをXID (`xid`)として指定します。 IDのXIDを取得する方法について詳しくは、IDのXIDの [取得について説明しているこのドキュメントの節を参照してください](./list-native-id.md)。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/mapping?xid=CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

### 複数のIDのIDマッピングの取得

この `POST``GET` メソッドは、上記のメソッドとバッチ同等に、複数のIDのマッピングを取得するために使用します。

>[!NOTE] 要求は、IDの数が最大1000個に達することを示す必要があります。 IDが1000個を超える要求は、400個のステータスコードになります。

**API形式**

```http
POST https://platform.adobe.io/data/core/identity/mappings
```

**リクエスト本文**

オプション1: マッピングを取得するXIDのリストを指定します。

```shell
{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
    "graph-type": "Private Graph"
}
```

オプション2: IDのリストを複合IDとして指定します。各IDは名前空間IDごとにID値と名前空間を示します。 この例は、「Private Graph」のデフォルト値を上書きする際に、このメソッド `graph-type` を使用する方法を示しています。

```shell
{
    "compositeXids": [{
            "nsid": 411,
            "id": "WRbM7AAAAJ_PBZHl"
        },
        {
            "nsid": 411,
            "id": "WY-RNgAAArI4rGBo"
        }
    ],
    "graph-type": "None"
}
```

**リクエスト**

**XIDの使用**

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/identity/mappings \
  -H 'authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: 111111@AdobeOrg' \
  -d '{
        "xids" : ["GesCQXX0CAESEE8wHpswUoLXXmrYy8KBTVgA"],
        "targetNs": "0",
        "graph-type": "Private Graph"
      }' | json_pp
```

**UIDの使用**

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/identity/mappings \
  -H 'authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: 111111@AdobeOrg' \
  -d '{
            "compositeXids": [{
                    "nsid": 411,
                    "id": "WRbM7AAAAJ_PBZHl"
                },
                {
                    "nsid": 411,
                    "id": "WY-RNgAAArI4rGBo"
                }
            ],
        "targetNs": "0",
        "graph-type": "Private Graph"
      }' | json_pp
```

指定された入力で関連IDが見つからなかった場合、 `HTTP 204` 応答コードはコンテンツなしで返されます。

**応答**

```json
{
    "version": 1,
    "mappings": [{
        "xid": "CAESEPl1uYyma1kMDWxx7dhbwGo",
        "mapping": [{
            "xid": "81218968060697815473313992060878182012",
            "lastAssociationTime": "1493310475047"
        }],
        "compositeXid": {
            "nsid": 411,
            "id": "WY-RNgAAArI4rGBo"
        },
        "mapping": [{
            "compositeXid": {
                "nsid": 411,
                "id": "WY-RNchvdsTSJS"
            },
            "lastAssociationTime": "1493310475047"
        }],

        "regions": [{
            "regionId": "10",
            "lastAssociationTime": "1493310475047"
        }]
    }],
    "unprocessedXids": ["cb0665db616f49758713252d8a335c1e"],
    "unprocessedNids": [{
        "nsid": 411,
        "id": "WY-RNgAAArI4rGBo"
    }]
}
```

- `lastAssociationTime`: 入力IDが最後にこのIDに関連付けられた時のタイムスタンプ。
- `regions`: IDが表示され `regionId` た場所 `lastAssociationTime` のANDを提供します。

## 次の手順

次のチュートリアルに進み、使用可能な [名前空間のリスト](./list-namespaces.md)。
