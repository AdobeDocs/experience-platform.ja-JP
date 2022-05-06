---
keywords: Experience Platform;ホーム;人気のあるトピック;データ準備;api ガイド;スキーマ;
solution: Experience Platform
title: スキーマ API エンドポイント
topic-legacy: schemas
description: Adobe Experience Platform API で「/functions」エンドポイントを使用して、マッピング式を検証し、使用可能なマッピングセット関数をリストできます。
exl-id: dc24bfb4-2d96-4757-a610-0c2ee960d41d
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 100%

---

# 関数エンドポイント

マッピングセット関数を使用すると、ソーススキーマと宛先スキーマの間でデータを変換できます。`/languages/el` エンドポイントを使用すると、式を検証し、使用可能なすべてのマッピングセット関数のリストを取得できます。

## 数式の検証

`/languages/el/validate` エンドポイントに POST リクエストを送信すると、現在の式が有効かどうかを検証できます。

**API 形式**

```
POST /languages/el/validate
```

**リクエスト**

```shell
curl -X POST https://platform.adobe.io/data/foundation/conversion/languages/el/validate \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'Content-Type: application/json' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '
  {
    "expression": "concat(\"Hi\", \",\", \"there\", \"!\")"
  }'
```

**応答**

応答に成功すると、HTTP ステータス 200 と式の検証ステータスが返されます。

```json
{
    "validationStatus": "succeeded",
    "error": "none"
}
```

## マッピングセットのリスト関数

`/languages/el/functions`エンドポイントに対して GET リクエストをおこなうと、使用可能なすべてのマッピングセット関数のリストを取得できます。

**API 形式**

```
GET /languages/el/functions
```

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/conversion/languages/el/functions \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答に成功すると、HTTP ステータス 200 と、使用可能なマッピングセット関数のリストが返されます。

>[!NOTE]
>
>この応答はスペースを節約するために切り捨てられています。

```json
[
    {
        "category": "Date / Time",
        "function": "date",
        "description": "Function that converts date string into a ZonedDateTime object.",
        "syntax": "ZonedDateTime date(String, String, ZonedDateTime)",
        "returns": "Returns the date object that is formatted in given format or a default date if the expression evaluates to a null date.",
        "returnType": "java.time.ZonedDateTime",
        "example": "",
        "result": "",
        "params": [],
        "since": 1
    },
    {
        "category": "Hierarchies - Arrays",
        "function": "first",
        "description": "Function to retrieve the first element of the given array.",
        "syntax": "T first(T...)",
        "returns": "The first element or null if the array is null or empty.",
        "returnType": "java.lang.Object",
        "example": "first(\"1\", \"2\", \"3\")",
        "result": "\"1\"",
        "params": [
            {
                "name": "values",
                "description": "Zero or more arguments",
                "type": "object",
                "dataType": "[Ljava.lang.Object;",
                "position": 1
            }
        ],
        "since": 1
    }
]
```

## リストマッピングセット演算子

`/languages/el/operators`エンドポイントに対して GET リクエストをおこなうと、使用可能なすべてのマッピングセット演算子のリストを取得できます。

**API 形式**

```
GET /languages/el/operators
```

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/conversion/languages/el/operators \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

応答に成功すると、HTTP ステータス 200 と、使用可能なマッピングセット演算子のリストが返されます。

>[!NOTE]
>
>この応答はスペースを節約するために切り捨てられています。

```json
[
    {
        "operatorSymbol": "+",
        "methodName": "add",
        "numberOfOperands": 2,
        "description": "Simple arithmetic addition",
        "example": "1 + 2"
    },
    {
        "operatorSymbol": "/",
        "methodName": "divide",
        "numberOfOperands": 2,
        "description": "Simple arithmetic division",
        "example": "1 / 2"
    },
    {
        "operatorSymbol": "~",
        "methodName": "complement",
        "numberOfOperands": 1,
        "description": "The usual ~ operator is used, e.g.\n~33\n, ~0010 0001 = 1101 1110 = -34.",
        "example": "~44"
    },
    {
        "operatorSymbol": "-",
        "methodName": "negate",
        "numberOfOperands": 1,
        "description": "The unary - operator is used. For example\n-12",
        "example": "-12"
    },
    {
        "operatorSymbol": "!",
        "methodName": "not",
        "numberOfOperands": 1,
        "description": "The usual ! operator can be used as well as the word not, e.g.\n!cond1\nand\nnot cond1\nare equivalent",
        "example": "!cond1"
    }
]
```
