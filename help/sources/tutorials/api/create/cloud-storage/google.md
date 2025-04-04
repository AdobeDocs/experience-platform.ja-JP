---
title: Flow Service API を使用したGoogle Cloud Storage ベース接続の作成
description: Flow Service API を使用してAdobe Experience PlatformをGoogle クラウドストレージアカウントに接続する方法について説明します。
exl-id: 321d15eb-82c0-45a7-b257-1096c6db6b18
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '556'
ht-degree: 38%

---

# [!DNL Flow Service] API を使用した [!DNL Google Cloud Storage] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Google Cloud Storage] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してGoogle Cloud Storage アカウントに正常に接続するために必要な追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Google Cloud Storage] アカウントに接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `accessKeyId` | [!DNL Google Cloud Storage] アカウントをExperience Platformに認証するために使用される 61 文字の英数字の文字列。 |
| `secretAccessKey` | [!DNL Google Cloud Storage] アカウントをExperience Platformに認証するために使用される 40 文字の base-64 エンコード文字列。 |
| `bucketName` | [!DNL Google Cloud Storage] バケットの名前。 クラウドストレージ内の特定のサブフォルダーへのアクセスを提供する場合は、バケット名を指定する必要があります。 |
| `folderPath` | アクセス権を付与するフォルダーへのパス。 |

これらの値について詳しくは、[Google Cloud Storage の HMAC キー](https://cloud.google.com/storage/docs/authentication/hmackeys#overview)ガイドを参照してください。独自のアクセスキー ID と秘密アクセスキーを生成する手順については、[[!DNL Google Cloud Storage]  概要 ](../../../../connectors/cloud-storage/google-cloud-storage.md) を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Google Cloud Storage] 認証資格情報をリクエストパラメーターの一部として使用します。

>[!TIP]
>
>この手順では、バケット名とサブフォルダーへのパスを定義して、アカウントがアクセスできるサブフォルダーを指定することもできます。

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
| `auth.params.accessKeyId` | [!DNL Google Cloud Storage] アカウントに関連付けられたアクセスキー ID。 |
| `auth.params.secretAccessKey` | [!DNL Google Cloud Storage] アカウントに関連付けられた秘密アクセスキー。 |
| `auth.params.bucketName` | [!DNL Google Cloud Storage] バケットの名前。 クラウドストレージ内の特定のサブフォルダーへのアクセスを提供する場合は、バケット名を指定する必要があります。 |
| `auth.params.folderPath` | アクセス権を付与するフォルダーへのパス。 |
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

このチュートリアルでは、API を使用して [!DNL Google Cloud Storage] 接続を作成し、一意の ID を応答本文の一部として取得しました。 この接続 ID を使用して [Flow Service API を使用したクラウドストレージの調査 ](../../explore/cloud-storage.md) を行うことができます。
