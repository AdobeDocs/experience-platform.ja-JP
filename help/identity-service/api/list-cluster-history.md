---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: IDのクラスター履歴の取得
topic: API guide
translation-type: tm+mt
source-git-commit: df85ea955b7a308e6be1e2149fcdfb4224facc53

---


# IDのクラスター履歴の取得

IDは、様々なデバイスグラフの実行中にクラスターを移動できます。 IDサービスは、特定のIDのクラスター関連付けを経時的に表示します。

オプションのパ `graph-type` ラメーターを使用して、クラスターの取得元の出力タイプを指定します。 オプションは次のとおりです。

- `None` - IDステッチを実行しない。
- `Private Graph`  — 個人のIDグラフに基づいてIDステッチを実行します。 を指定しな `graph-type` い場合、これがデフォルトです。

## 単一のIDのクラスタ履歴の取得

**API形式**

```http
GET https://platform-{REGION}.adobe.io/data/core/identity/cluster/history
```

**リクエスト**

オプション1:IDを名前空間(ID`nsId`別)およびID値(`id`)として指定します。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/cluster/history?nsId=411&id=WTCpVgAAAFq14FMF' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

オプション2:IDを名前空間(`ns`名前順)とID値(`id`)として指定します。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/cluster/history?ns=AMO&id=WTCpVgAAAFq14FMF' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

オプション3:IDをXID (`xid`)として指定します。 IDのXIDを取得する方法について詳しくは、IDのXIDの取得に関するこのドキュメントの [節を参照してください](./list-native-id.md)。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/cluster/history?xid=CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

## 複数のIDのクラスター履歴の取得

複数のIDのク `POST` ラスター履歴を返すには、このメソッド `GET` を上記のメソッドと同等のバッチとして使用します。

>[!NOTE] 要求は、IDの数が最大1000個以下であることを示す必要があります。 IDが1000を超える要求は、400のステータスコードになります。

**API形式**

```http
POST https://platform-va7.adobe.io/data/core/identity/clusters/history
```

**リクエスト本文**

オプション1:クラスターリストを取得するXIDのメンバーを指定します。

```shell
{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
    "graph-type": "Private Graph"
}
```

オプション2:IDのリストを複合IDとして指定します。各IDは、ID値と名前空間を名前空間コードで指定します。

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

**スタブ要求**

ヘッダーを使 `x-uis-cst-ctx: stub` 用すると、スタブ化された応答が返されます。 これは、サービスの完了中に、初期の統合開発の進展を容易にする一時的なソリューションです。 これは不要になった時点で廃止されます。

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/identity/clusters/history \
  -H 'authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'x-uis-cst-ctx: stub' \
  -d '{
        "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
        "graph-type": "Private Graph"
      }'
```

**XIDの使用**

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/identity/clusters/history \
  -H 'authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
        "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
        "graph-type": "Private Graph"
      }' | json_pp
```

**UIDの使用**

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/identity/clusters/history \
  -H 'authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
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

>[!NOTE] 要求のXIDが同じクラスターに属しているか、1つ以上のクラスターが関連付けられているかに関係なく、応答には、要求で提供される各XIDに対して常に1つのエントリが含まれます。

## 次の手順

次のチュートリアルに進み、 [リストIDマッピング](./list-identity-mappings.md)