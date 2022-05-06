---
keywords: Experience Platform；ホーム；人気の高いトピック；ID;ID
solution: Experience Platform
title: ID マッピングのリスト
topic-legacy: API guide
description: マッピングは、クラスターにある、指定した名前空間の全 ID の集まりです。
exl-id: db80c783-620b-4ba3-b55c-75c1fd6e90b1
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 96%

---

# ID マッピングのリストの表示

マッピングは、クラスターにある、指定した名前空間の全 ID の集まりです。

## 単一の ID の ID マッピングの取得

ID を指定し、リクエスト内の ID で表されるものと同じ名前空間から、すべての関連 ID を取得します。

**API 形式**

```http
GET https://platform-{REGION}.adobe.io/data/core/identity/mapping
```

**リクエスト**

オプション 1：ID を名前空間（ID 別 `nsId`）および ID 値（`id`）として指定します。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/mapping?nsId=411&id=WTCpVgAAAFq14FMF' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

オプション 2：ID を名前空間（`ns`名前別）と ID 値（`id`）として指定します。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/mapping?ns=AMO&id=WTCpVgAAAFq14FMF' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

オプション 3：ID を XID（`xid`）として指定します。ID の XID を取得する方法について詳しくは、このドキュメントの [ID の XID の取得](./list-native-id.md)に関する節を参照してください。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/mapping?xid=CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

### 複数の ID の ID マッピングの取得

`POST` メソッドを前述の `GET` メソッドと同等のバッチとして使用して、複数の ID のマッピングを取得します。

>[!NOTE]
>
> リクエストで使用できる ID 数の上限は 1000 です。ID 数が 1000 を超えるリクエストに対しては、ステータスコード 400 が発生します。

**API 形式**

```http
POST https://platform.adobe.io/data/core/identity/mappings
```

**リクエスト本文**

オプション 1：マッピングを取得する XID のリストを指定します。

```shell
{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
    "graph-type": "Private Graph"
}
```

オプション 2：ID のリストを複合 ID として指定します。それぞれの ID は ID 値で、名前空間は名前空間 ID で指定します。この例では、このメソッドを使用すると同時に、「プライベートグラフ」のデフォルトの `graph-type` を上書きしています。

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

**XID を使用**

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/identity/mappings \
  -H 'authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: 111111@AdobeOrg' \
  -d '{
        "xids": ["GesCQXX0CAESEE8wHpswUoLXXmrYy8KBTVgA"],
        "targetNs": "0",
        "graph-type": "Private Graph"
      }' | json_pp
```

**UID を使用**

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

指定した入力内容に関連する ID が見つからなかった場合は、応答コード `HTTP 204` がコンテンツなしで返されます。

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

- `lastAssociationTime`：入力 ID がこの ID に最後に関連付けられたときのタイムスタンプ。
- `regions`：ID が見つかった場所の `regionId` と `lastAssociationTime` を示します。

## 次の手順

次のチュートリアルに進み、[利用可能な名前空間のリストを表示](./list-namespaces.md)します。
