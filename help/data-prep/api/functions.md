---
keywords: Experience Platform；ホーム；人気の高いトピック；データ準備；apiガイド；スキーマ;
solution: Experience Platform
title: スキーマAPIエンドポイント
topic: schemas
description: 'Adobe Experience PlatformAPIの`/functions''エンドポイントを使用すると、マッピング式とリストで使用可能なマッピングセット関数を検証できます。 '
translation-type: tm+mt
source-git-commit: 60c80a73deb8c77f19d5963cc3319d46143fb4c3
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 12%

---


# 関数エンドポイント

マッピングセット関数を使用すると、ソーススキーマとターゲットノードの間でデータを変換できます。 `/languages/el`エンドポイントを使用して式を検証でき、使用可能なすべてのマッピングセット関数のリストを取得できます。

## 式の検証

`/languages/el/validate`エンドポイントにPOSTリクエストを行うことで、現在の式が有効かどうかを検証できます。

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
  -H 'x-gw-ims-org-id: {IMS_ORG}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -d '
  {
    "expression": "concat(\"Hi\", \",\", \"there\", \"!\")"
  }'
```

**応答** 

成功した応答は、式の検証ステータスと共にHTTPステータス200を返します。

```json
{
    "validationStatus": "succeeded",
    "error": "none"
}
```

## リストマッピングセット関数

`/languages/el/functions`エンドポイントにGETリクエストを行うことで、使用可能なすべてのマッピングセット関数のリストを取得できます。

**API 形式**

```
GET /languages/el/functions
```

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/conversion/languages/el/functions \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答が成功すると、使用可能なすべてのマッピングセット関数のリストと共にHTTPステータス200が返されます。

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

`/languages/el/operators`エンドポイントにGETリクエストを行うことで、使用可能なすべてのマッピングセット演算子のリストを取得できます。

**API 形式**

```
GET /languages/el/operators
```

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/foundation/conversion/languages/el/operators \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \ 
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

応答が成功すると、使用可能なすべてのマッピングセット演算子のリストと共にHTTPステータス200が返されます。

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
