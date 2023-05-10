---
title: フローサービス API を使用したGoogle Cloud Storage Base 接続の作成
description: フローサービス API を使用してAdobe Experience PlatformをGoogleクラウドストレージアカウントに接続する方法を説明します。
exl-id: 321d15eb-82c0-45a7-b257-1096c6db6b18
source-git-commit: 3636b785d82fa2e49f76825650e6159be119f8b4
workflow-type: tm+mt
source-wordcount: '560'
ht-degree: 48%

---

# [!DNL Flow Service] API を使用した [!DNL Google Cloud Storage] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Google Cloud Storage] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、 [!DNL Flow Service] API

### 必要な資格情報の収集

次のために [!DNL Flow Service] を [!DNL Google Cloud Storage] アカウントの場合、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `accessKeyId` | 61 文字の英数字から成る文字列で、 [!DNL Google Cloud Storage] アカウントを Platform に送信します。 |
| `secretAccessKey` | ユーザーの認証に使用される 40 文字のベース 64 エンコードされた文字列 [!DNL Google Cloud Storage] アカウントを Platform に送信します。 |
| `bucketName` | お客様の [!DNL Google Cloud Storage] バケット。 クラウドストレージ内の特定のサブフォルダーへのアクセスを提供する場合は、バケット名を指定する必要があります。 |
| `folderPath` | アクセス権を付与するフォルダーのパスです。 |

これらの値について詳しくは、[Google Cloud Storage の HMAC キー](https://cloud.google.com/storage/docs/authentication/hmackeys#overview)ガイドを参照してください。独自のアクセスキー ID と秘密アクセスキーを生成する手順については、 [[!DNL Google Cloud Storage] 概要](../../../../connectors/cloud-storage/google-cloud-storage.md).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Google Cloud Storage] 認証資格情報をリクエストパラメーターの一部として使用します。

>[!TIP]
>
>この手順では、サブフォルダーのバケット名とパスを定義して、アカウントがアクセスできるサブフォルダーを指定することもできます。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Google Cloud Storage] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Google Cloud Storage connection",
      "description": "Connector for Google Cloud Storage",
      "auth": {
          "specName": "Basic Authentication for google-cloud",
          "params": {
              "accessKeyId": "accessKeyId",
              "secretAccessKey": "secretAccessKey",
              "bucketName": "acme-google-cloud-bucket",
              "folderPath": "/acme/customers/sales"
          }
      },
      "connectionSpec": {
          "id": "32e8f412-cdf7-464c-9885-78184cb113fd",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.accessKeyId` | に関連付けられたアクセスキー ID [!DNL Google Cloud Storage] アカウント |
| `auth.params.secretAccessKey` | に関連付けられた秘密アクセスキー [!DNL Google Cloud Storage] アカウント |
| `auth.params.bucketName` | お客様の [!DNL Google Cloud Storage] バケット。 クラウドストレージ内の特定のサブフォルダーへのアクセスを提供する場合は、バケット名を指定する必要があります。 |
| `auth.params.folderPath` | アクセス権を付与するフォルダーのパスです。 |
| `connectionSpec.id` | [!DNL Google Cloud Storage] 接続仕様 ID：`32e8f412-cdf7-464c-9885-78184cb113fd` |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Google Cloud Storage] 応答本文の一部として、API と一意の ID を使用した接続を取得しました。 この接続 ID を [フローサービス API を使用したクラウドストレージの調査](../../explore/cloud-storage.md).
