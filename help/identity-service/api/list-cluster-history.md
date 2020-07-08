---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: IDのクラスター履歴の取得
topic: API guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 1%

---


# IDのクラスター履歴の取得

IDは、様々なデバイスグラフの実行中にクラスターを移動できます。 [!DNL Identity Service] 特定のIDに対するクラスターの関連付けを経時的に表示します。

オプションの `graph-type` パラメーターを使用して、クラスターの取得元の出力形式を指定します。 オプションは次のとおりです。

- `None` - IDの切り替えを実行しません。
- `Private Graph`  — プライベートIDグラフに基づいてIDの切り替えを実行します。 値が指定され `graph-type` ていない場合、これがデフォルトです。

## 単一IDのクラスタ履歴の取得

**API形式**

```http
GET https://platform-{REGION}.adobe.io/data/core/identity/cluster/history
```

**リクエスト**

オプション1: IDを名前空間(`nsId`、ID)およびID値(`id`)として指定します。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/cluster/history?nsId=411&id=WTCpVgAAAFq14FMF' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

オプション2: IDを名前空間(名前`ns`別)およびID値(`id`)として指定します。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/cluster/history?ns=AMO&id=WTCpVgAAAFq14FMF' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

オプション3: IDをXID (`xid`)として指定します。 IDのXIDを取得する方法について詳しくは、IDのXIDの [取得について説明しているこのドキュメントの節を参照してください](./list-native-id.md)。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/cluster/history?xid=CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

## 複数のIDのクラスター履歴の取得

複数のIDのクラスター履歴を返すには、 `POST` このメソッドを上記の `GET` メソッドとバッチして使用します。

>[!NOTE]
>
>要求は、IDの数が最大1000個に達することを示す必要があります。 IDが1000個を超える要求は、400個のステータスコードになります。

**API形式**

```http
POST https://platform-va7.adobe.io/data/core/identity/clusters/history
```

**リクエスト本文**

オプション1: クラスターメンバーを取得するXIDのリストを指定します。

```shell
{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"],
    "graph-type": "Private Graph"
}
```

オプション2: IDのリストを複合IDとして指定します。各IDには名前空間コード別のID値と名前空間が付けられます。

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

ヘッダーを使用すると、スタブ化された応答が返され `x-uis-cst-ctx: stub` ます。 これは、サービスの完了中に、初期の統合開発の進展を容易にする一時的なソリューションです。 これは不要になった時点で廃止されます。

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

>[!NOTE]
>
>要求のXIDが同じクラスターに属しているか、1つ以上のXIDにクラスターが関連付けられているかに関係なく、要求で提供される各XIDに対して、応答には常に1つのエントリが存在します。

## 次の手順

次のチュートリアルに進み、 [リストIDマッピングを行います](./list-identity-mappings.md)