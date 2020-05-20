---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: PQL変換
topic: developer guide
translation-type: tm+mt
source-git-commit: 45a196d13b50031d635ceb7c5c952e42c09bd893
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 5%

---


# PQL変換

導入

- pql/textとpql/jsonからフォーマットを変換

## はじめに

このガイドで使用されるAPIエンドポイントは、セグメント化APIの一部です。 先に進む前に、 [セグメント化開発ガイドを参照してください](./getting-started.md)。

特に、セグメント化開発ガイドの「 [はじめに](./getting-started.md#getting-started) 」の節には、関連トピックへのリンク、ドキュメント内のサンプルAPI呼び出しを読むためのガイド、Experience Platform APIの呼び出しを正常に行うために必要なヘッダーに関する重要な情報が含まれています。

## pql/textとpql/jsonからフォーマットを変換

**API形式**

```http
POST /segment/conversion
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/core/ups/segment/conversion \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}'
 -d '
{
  "name": "People who ordered in the last 30 days",
  "expression": {
    "type": "PQL",
    "format": "pql/text",
    "value": "workAddress.country = \"US\""
  },
  "schema": {
    "name": "_xdm.context.profile"
  },
  "ttlInDays": 60
}
 '
```

| プロパティ | 説明 |
| -------- | ----------- |
| `name` | セグメント定義の一意の名前。 |
| `expression.type` | 式タイプ。 またはを指定でき `PQL` ま `ARL`す。 |
| `expression.format` | 式形式。 またはを指定でき `pql/text` ま `pql/json`す。 |
| `expression.value` | 変換する式のクエリ文字列。 |
| `schema.name` | 参照しているスキーマのクラスID。 |
| `ttlInDays` | 有効期間(TTL)を表す整数です。 |

**応答**

正常に応答すると、HTTPステータス200が返され、変換されたセグメントの詳細が示されます。

```json
{
    "ttlInDays": 60,
    "imsOrgId": "E95186D65A28ABF00A495D82@AdobeOrg",
    "sandbox": {
        "sandboxId": "28e74200-e3de-11e9-8f5d-7f27416c5f0d",
        "sandboxName": "prod",
        "type": "production",
        "default": true
    },
    "expression": {
        "type": "PQL",
        "format": "pql/json",
        "value": "{\"nodeType\":\"fnApply\",\"fnName\":\"=\",\"params\":[{\"nodeType\":\"fieldLookup\",\"fieldName\":\"country\",\"object\":{\"nodeType\":\"fieldLookup\",\"fieldName\":\"workAddress\",\"object\":{\"nodeType\":\"parameterReference\",\"position\":1}}},{\"nodeType\":\"literal\",\"literalType\":\"String\",\"value\":\"US\"}]}"
    }
}
```

## 次の手順
