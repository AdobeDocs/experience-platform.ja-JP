---
keywords: Experience Platform；ホーム；人気のトピック；ID；クラスター履歴
solution: Experience Platform
title: ID のクラスター履歴の取得
topic-legacy: API guide
description: ID を使用して、様々なデバイスグラフの実行中にクラスターを移動できます。ID サービスは、特定の ID のクラスター関連付けを経時的に表示します。
exl-id: e52edb15-e3d6-4085-83d5-212bbd952632
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 92%

---

# ID のクラスター履歴の取得

ID を使用して、様々なデバイスグラフの実行中にクラスターを移動できます。[!DNL Identity Service] は、時間の経過に伴う特定の ID のクラスター関連付けを表示します。

オプションの `graph-type` パラメーターを使用して、クラスターの取得元の出力タイプを指定します。オプションは次のとおりです。

- `None` - ID ステッチを実行しない。
- `Private Graph` - 個人の ID グラフに基づいて ID ステッチを実行する。`graph-type` を指定しない場合、このオプションがデフォルトです。

## 単一の ID のクラスター履歴の取得

**API 形式**

```http
GET https://platform-{REGION}.adobe.io/data/core/identity/cluster/history
```

**リクエスト**

オプション 1：ID を名前空間（ID 別 `nsId`）および ID 値（`id`）として指定します。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/cluster/history?nsId=411&id=WTCpVgAAAFq14FMF' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

オプション 2：ID を名前空間（`ns`名前別）と ID 値（`id`）として指定します。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/cluster/history?ns=AMO&id=WTCpVgAAAFq14FMF' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

オプション 3：ID を XID（`xid`）として指定します。ID の XID を取得する方法について詳しくは、このドキュメントの [ID の XID の取得](./list-native-id.md)に関する節を参照してください。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/cluster/history?xid=CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

## 複数の ID のクラスター履歴の取得

`POST` メソッドを上記の `GET` メソッドと同等のバッチとして使用して、複数の ID のクラスター履歴を返します。

>[!NOTE]
>
> リクエストで使用できる ID 数の上限は 1000 です。ID 数が 1000 を超えるリクエストに対しては、ステータスコード 400 が発生します。

**API 形式**

```http
POST https://platform-va7.adobe.io/data/core/identity/clusters/history
```

**リクエスト本文**

オプション 1：クラスターメンバーを取得する XID のリストを指定します。

```shell
{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
    "graph-type": "Private Graph"
}
```

オプション 2：ID のリストを複合 ID として指定します。各 ID では、ID 値と名前空間が名前空間コードで指定されます。

```shell
{
    "compositeXids": [{
            "ns": "AdCloud",
            "id": "WRbM7AAAAJ_PBZHl"
        },
        {
            "ns": "AddCloud",
            "id": "WY-RNgAAArI4rGBo"
        }
    ]
}
```

**リクエスト**

**スタブリクエスト**

`x-uis-cst-ctx: stub` ヘッダーを使用すると、スタブ化された応答が返されます。これは、サービスが完了している間、早期の統合開発の進行を容易にするための一時的なソリューションです。このソリューションは不要になった時点で廃止されます。

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/identity/clusters/history \
  -H 'authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'x-uis-cst-ctx: stub' \
  -d '{
        "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
        "graph-type": "Private Graph"
      }'
```

**XID の使用**

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/identity/clusters/history \
  -H 'authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
        "graph-type": "Private Graph"
      }' | json_pp
```

**UID の使用**

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/identity/clusters/history \
  -H 'authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
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
        "graph-type": "Private Graph"
      }' | json_pp
```

**応答**

```json
{
    "version": 1,
    "xidsClusterHistory": [{
            "xid": "GZsBQnHQaGtL46ZKSvO9bNRE1DcUyQA",
            "compositeXid": {
                "nsid": 411,
                "id": "WY-RNgAAArI4rGBo"
            },
            "clusterHistory": [{
                    "clusterId": "4c686f23-0871-41c2-b4f4-adef89f6bd2c",
                    "cRecordedTS": "1504741401382"
                },
                {
                    "clusterId": "29bf066c-971a-11e7-abc4-cec278b6b50a",
                    "cRecordedTS": "1502063001629"
                },
                {
                    "clusterId": "aeb2f60c-b0f1-446a-91dd-d28ab6a44ff9",
                    "cRecordedTS": "1499384601763"
                }
            ]
        },
        {
            "xid": "CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8",
            "compositeXid": {
                "nsid": 411,
                "id": "WY-RNgAAArI4rGBo"
            },
            "clusterHistory": [{
                "clusterId": "4c686f23-0871-41c2-b4f4-adef89f6bd2c",
                "cRecordedTS": "1504741401937"
            }]
        }
    ],
    "unprocessedXids": ["cb0665db616f49758713252d8a335c1e"],
    "unprocessedNids": [{
        "nsid": 411,
        "id": "WY-RNgAAArI4rGBo"
    }]

}
```

>[!NOTE]
>
> リクエストの XID が同じクラスターに属しているかどうか、または 1 つ以上にクラスターが関連付けられているかどうかに関係なく、応答には、リクエストで提供された XID ごとに 1 つのエントリがあります。

## 次の手順

次のチュートリアルに進み、[ID マッピングをリスト](./list-identity-mappings.md)します。
