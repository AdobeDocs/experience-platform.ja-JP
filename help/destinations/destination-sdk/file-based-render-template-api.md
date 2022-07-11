---
description: このページでは、 /authoring/testing/template/render エンドポイントを使用して、宛先設定で定義されたテンプレート化された顧客データフィールドがどのように表示されるかを視覚化する方法について説明します。
title: テンプレート化された顧客フィールドの検証
source-git-commit: fa092e4d1828d9ecd5bc98e3f225fa377f38065f
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 20%

---


# テンプレート化された顧客フィールドの検証

## 概要 {#overview}

この `/authoring/testing/template/render` エンドポイントは、テンプレート化されている方法を視覚化するのに役立ちます [顧客データフィールド](file-based-destination-configuration.md#customer-data-fields) の宛先設定で定義されたは次のようになります。

エンドポイントは、顧客データフィールドのランダムな値を生成し、応答で返します。 これにより、バケット名やフォルダーパスなど、顧客データフィールドのセマンティック構造を検証できます。

## はじめに {#getting-started}

続ける前に「[はじめる前に](./getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## 前提条件 {#prerequisites}

使用する前に `/template/render` エンドポイントで、次の条件を満たしていることを確認します。

* Destination SDKを通じて作成された既存のファイルベースの宛先があり、 [宛先カタログ](../ui/destinations-workspace.md).
* API リクエストを正常に実行するには、テストする宛先インスタンスに対応する宛先インスタンス ID が必要です。 Platform UI で宛先との接続を参照する際に、API 呼び出しで使用する必要がある宛先インスタンス ID を URL から取得します。

   ![URL から宛先インスタンス ID を取得する方法を示す UI 画像。](assets/get-destination-instance-id.png)

## テンプレート化された顧客フィールドのレンダリング {#render-customer-fields}

**API 形式**

```http
POST /authoring/testing/template/render/destination
```

この API エンドポイントの動作を説明するために、次の顧客データフィールドを設定したファイルベースの宛先について考えてみましょう。

```json
"fileBasedS3Destination":{
   "bucket":{
      "templatingStrategy":"PEBBLE_V1",
      "value":"{{customerData.bucket}}"
   },
   "path":{
      "templatingStrategy":"PEBBLE_V1",
      "value":"{{customerData.path}}"
   }
}
```

**リクエスト**

以下のリクエストは、 `/authoring/testing/template/render` エンドポイント：前述の 2 つの顧客データフィールドに対してランダムに生成された値で応答を返します。

```shell
curl -X POST 'https://platform.adobe.io/data/core/activation/authoring/testing/template/render/destination' \
 -H 'Authorization: Bearer {ACCESS_TOKEN}' \
 -H 'Content-Type: application/json' \
 -H 'x-gw-ims-org-id: {IMS_ORG}' \
 -H 'x-api-key: {API_KEY}' \
 -H 'x-sandbox-name: {SANDBOX_NAME}' \
 -d '
 {
    "destinationId": "{DESTINATION_CONFIGURATION_ID}",
    "templates": {
        "bucket": "{{customerData.bucket}}",
        "path": "{{customerData.bucket}}/{{customerData.path}}"
    }
}'
```

| パラメーター | 説明 |
| -------- | ----------- |
| `destinationId` | の ID [宛先設定](file-based-destination-configuration.md) テスト中の |
| `templates` | テンプレート化されたフィールド名 ( [宛先サーバーの設定](server-and-file-configuration.md). |

**応答**

成功すると、 `HTTP 200 OK` ステータスの場合、本文には、テンプレート化されたフィールドに対してランダムに生成された値が含まれます。

この応答は、顧客データフィールドの正しい構造（バケット名やフォルダーパスなど）を検証するのに役立ちます。


```json
{
    "results": {
        "bucket": "hfWpE-bucket",
        "path": "hfWpE-bucket/ceC"
    }
}
```

## API エラー処理 {#api-error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順 {#next-steps}

このドキュメントを読むと、 [宛先サーバー](server-and-file-configuration.md).
