---
keywords: エクスペリエンス Platform、home、人気のある話題。Google Cloud storage; google cloud storage; google;Google
solution: Experience Platform
title: フローサービス API を使用した Google Cloud ストレージの基礎接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して、Adobe 体験プラットフォームを Google Cloud ストレージアカウントに接続する方法について説明します。
exl-id: 321d15eb-82c0-45a7-b257-1096c6db6b18
source-git-commit: 13bd1254dfe89004465174a7532b4f6aaef54c09
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 11%

---

# [!DNL Google Cloud Storage]API を使用したベース接続の作成 [!DNL Flow Service]

ベース接続は、ソースと Adobe エクスペリエンスプラットフォームとの間の認証された接続を表します。

このチュートリアルでは、API を使用するための基本的な接続を作成する手順について説明し [!DNL Google Cloud Storage] [[!DNL Flow Service]  ](https://www.adobe.io/experience-platform-apis/references/flow-service/) ます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース ](../../../../home.md) : [!DNL Experience Platform] 多種多様なソースからのデータの ingested を可能にするとともに、サービスを使用した受信データを構造化、ラベル付け、拡張するための機能を提供し [!DNL Platform] ます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の各セクションでは、API を使用して Google Cloud ストレージアカウントに問題なく接続するために必要な追加情報を示し [!DNL Flow Service] ます。

### 必要な資格情報の収集

アカウントを使用して [!DNL Flow Service] 接続するには [!DNL Google Cloud Storage] 、次の接続プロパティの値を入力する必要があります。

| Chap | 説明 |
| ---------- | ----------- |
| `accessKeyId` | プラットフォームに対してアカウントを認証するために使用される61文字の英数字のストリング [!DNL Google Cloud Storage] 。 |
| `secretAccessKey` | プラットフォームへのアカウントの認証に使用される 40-character、base-64 エンコードされたストリング [!DNL Google Cloud Storage] です。 |

これらの値について詳しくは、 [ Google Cloud STORAGE HMAC キーガイドを参照してください ](https://cloud.google.com/storage/docs/authentication/hmackeys#overview) 。 独自のアクセスキー ID とシークレットアクセスキーを作成する手順については、 [[!DNL Google Cloud Storage]  「概要」を参照してください ](../../../../connectors/cloud-storage/google-cloud-storage.md) 。

### プラットフォーム Api の使用

プラットフォーム Api の呼び出しを適切に行う方法については、Platform Api の概要を参照してください [ ](../../../../../landing/api-guide.md) 。

## ベース接続の作成

ベース接続を行うと、ソースとプラットフォームの間の情報が保持されます。ソースの認証の資格情報、接続の現在の状態、および一意の基本接続 ID が含まれています。 ベース接続 ID を使用して、ソース内でファイルを検索してナビゲートし、データの種類とフォーマットに関する情報も含めて、取り込む特定のアイテムを指定することができます。

ベース接続 ID を作成するには、そのエンドポイントに POST 要求を行います。この場合は、 `/connections` [!DNL Google Cloud Storage] 要求パラメーターの一部として認証資格情報を指定します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次の要求によって、次のような基本的な接続が作成され [!DNL Google Cloud Storage] ます。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Google Cloud Storage connection",
        "description": "Connector for Google Cloud Storage",
        "auth": {
            "specName": "Basic Authentication for google-cloud",
            "params": {
                "accessKeyId": "accessKeyId",
                "secretAccessKey": "secretAccessKey"
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
| `auth.params.accessKeyId` | アカウントに関連付けられたアクセスキー ID [!DNL Google Cloud Storage] 。 |
| `auth.params.secretAccessKey` | アカウントに関連付けられた秘密のアクセスキー [!DNL Google Cloud Storage] です。 |
| `connectionSpec.id` | [!DNL Google Cloud Storage]コネクション仕様 ID。`32e8f412-cdf7-464c-9885-78184cb113fd` |

**応答**

応答が成功した場合は、一意の識別子 () を含む、新しく作成された接続の詳細が返さ `id` れます。 この ID は、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルでは、Api を使用して接続を作成し、 [!DNL Google Cloud Storage] 応答本体の一部として一意の ID を取得しました。 この接続 ID を使用し [ て、フローサービス API を使用してクラウドストレージを探すことができ ](../../explore/cloud-storage.md) ます。
