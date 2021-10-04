---
keywords: Experience Platform；ホーム；人気のあるトピック；リスト ID；リストクラスター
solution: Experience Platform
title: クラスター内のすべての ID のリスト
topic-legacy: API guide
description: ID グラフで関連付けられている ID は、名前空間に関係なく、その ID グラフでは同じ「クラスター」の一部と見なされます。以下のオプションは、すべてのクラスターメンバーにアクセスする手段を提供します。
exl-id: 0fb9eac9-2dc2-4881-8598-02b3053d0b31
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 95%

---

# クラスター内のすべての ID のリスト

ID グラフで関連付けられている ID は、名前空間に関係なく、その ID グラフでは同じ「クラスター」の一部と見なされます。以下のオプションは、すべてのクラスターメンバーにアクセスする手段を提供します。

## 単一の ID に関連付けられた ID の取得

単一の ID のすべてのクラスターメンバーを取得します。

オプションの `graph-type` パラメーターを使用して、クラスターの取得元の ID グラフを示すことができます。オプションは次のとおりです。

- なし — ID を結合しません。
- プライベートグラフ — プライベート ID グラフに基づいて ID を結合します。`graph-type` を指定しない場合、これがデフォルトです。

**API 形式**

```http
GET https://platform-{REGION}.adobe.io/data/core/identity/cluster/members?{PARAMETERS}
```

**リクエスト**

オプション 1：ID を名前空間（ID 別 `nsId`）および ID 値（`id`）として指定します。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/cluster/members?nsId=411&id=WTCpVgAAAFq14FMF' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

オプション 2：ID を名前空間（`ns`名前別）と ID 値（`id`）として指定します。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/cluster/members?ns=AMO&id=WTCpVgAAAFq14FMF' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

オプション 3：ID を XID（`xid`）として指定します。ID の XID を取得する方法について詳しくは、このドキュメントの [ID の XID の取得](./list-native-id.md)に関する節を参照してください。

```shell
curl -X GET \
  'https://platform-va7.adobe.io/data/core/identity/cluster/members?xid=CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

## 複数の ID に関連付けられた ID の取得

`POST` を上記の `GET` メソッドと同等のバッチとして使用して、複数の ID のクラスター内の ID を返します。

>[!NOTE]
>
> リクエストで使用できる ID 数の上限は 1000 です。ID 数が 1000 を超えるリクエストに対しては、ステータスコード 400 が発生します。

**API 形式**

```http
POST https://platform-{REGION}.adobe.io/data/core/identity/clusters/members
```

**リクエスト**

次のリクエストは、クラスターメンバーを取得する XID のリストの提供を示しています。

**スタブリクエスト**

`x-uis-cst-ctx: stub` ヘッダーを使用すると、スタブ化された応答が返されます。これは、サービスが完了している間、早期の統合開発の進行を容易にするための一時的なソリューションです。これは不要になった時点で廃止されます。

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/identity/clusters/members \
  -H 'authorization: Bearer {ACCESS_TOKEN}' \
  -H 'content-type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-uis-cst-ctx: stub' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '{
    "xids": ["GYMBWaoXbMtZ1j4eAAACepuQGhs","b2NJK9a5X7x4LVE4rUqkMyM"]
}'
```

**XID を使用した呼び出し**

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/identity/clusters/members \
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

**UID を使用した呼び出し**

```shell
curl -X POST \
  https://platform-va7.adobe.io/data/core/identity/clusters/members \
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

**「スタブ」応答**

```json
{
   "version": 1,
   "clusters": [{
           "xid": "GZsBQnHQaGtL46ZKSvO9bNRE1DcUyQA",
           "compositeXid": {
               "nsid": 411,
               "id": "WRbM7AAAAJ_PBZHl"
           },
           "members": ["e8138f65-d3d3-4485-a7e1-6712e047349d", "21312343536983537571245438594"],
           "members": [{
                   "nsid": 0,
                   "id": "27064814400205787570627663430729680462"
               },
               {
                   "nsid": 411,
                   "id": "86826386186182763871263871263876128612"
               }
           ]
       },
       {
           "xid": "CJsDEAMaEAHmCKwPCQYNvzxD9JGDHZ8",
           "compositeXid": {
               "nsid": 411,
               "id": "WRbM7AAAAJ_PBZHl"
           },
           "members": [],
           "members": []
       }
   ],
   "unprocessedXids": ["cb0665db616f49758713252d8a335c1e"],
   "unprocessedNids": [{
       "nsid": 411,
       "id": "WY-RNgAAArI4rGBo"
   }]
}
```

**完全な応答**

```json
{
   "unprocessedXids": [],
   "unprocessedNids": [],
   "version": "1.0.0",
   "clusters": [{
           "xid": "411|WRbM7AAAAJ_PBZHl",
           "members": [
               "411|WRbM7AAAAJ_PBZHl",
               "0|47713142741924778930324734610798294416"
           ],
           "compositeXid": {
               "nsid": 411,
               "id": "WRbM7AAAAJ_PBZHl"
           },
           "members": [{
                   "nsid": 411,
                   "id": "WRbM7AAAAJ_PBZHl"
               },
               {
                   "nsid": 0,
                   "id": "47713142741924778930324734610798294416"
               }
           ]
       },
       {
           "xid": "411|WY-RNgAAArI4rGBo",
           "compositeXid": {
               "nsid": 411,
               "id": "WY-RNgAAArI4rGBo"
           },
           "members": [
               "411|WY-RNgAAArI4rGBo",
               "411|WY-RNgAAArI4rGGy"
           ],
           "members": [{
                   "nsid": 411,
                   "id": "WY-RNgAAArI4rGBo"
               },
               {
                   "nsid": 411,
                   "id": "WY-RNgAAArI4rGGy"
               }
           ]

       }
   ]
}
```

>[!NOTE]
>
> リクエストの XID が同じクラスターに属しているかどうか、または 1 つ以上にクラスターが関連付けられているかどうかに関係なく、応答には、リクエストで提供された XID ごとに 1 つのエントリがあります。

## 次の手順

次のチュートリアルに進み、[ID のクラスター履歴をリスト](./list-cluster-history.md)します。
