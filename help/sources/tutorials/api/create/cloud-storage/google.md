---
keywords: Experience Platform；ホーム；人気のあるトピック；Google Cloud Storage;google cloud storage;google;Google
solution: Experience Platform
title: フローサービスAPIを使用したGoogle Cloud Storage Base接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAdobe Experience PlatformをGoogle Cloud Storageアカウントに接続する方法を説明します。
exl-id: 321d15eb-82c0-45a7-b257-1096c6db6b18
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 10%

---

# [!DNL Flow Service] APIを使用して[!DNL Google Cloud Storage]ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Google Cloud Storage]の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用してGoogle Cloud Storageアカウントに正しく接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Google Cloud Storage]アカウントと接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `accessKeyId` | [!DNL Google Cloud Storage]アカウントをPlatformに対して認証するために使用される61文字の英数字の文字列。 |
| `secretAccessKey` | [!DNL Google Cloud Storage]アカウントをPlatformに対して認証するために使用される、40文字のベース64エンコードされた文字列。 |

これらの値について詳しくは、『[Google Cloud Storage HMAC keys](https://cloud.google.com/storage/docs/authentication/hmackeys#overview)』ガイドを参照してください。 独自のアクセスキーIDと秘密アクセスキーを生成する手順については、[[!DNL Google Cloud Storage] 概要](../../../../connectors/cloud-storage/google-cloud-storage.md)を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL Google Cloud Storage]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Google Cloud Storage]のベース接続を作成します。

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
| `auth.params.accessKeyId` | [!DNL Google Cloud Storage]アカウントに関連付けられたアクセスキーID。 |
| `auth.params.secretAccessKey` | [!DNL Google Cloud Storage]アカウントに関連付けられた秘密アクセスキー。 |
| `connectionSpec.id` | [!DNL Google Cloud Storage]接続仕様ID:`32e8f412-cdf7-464c-9885-78184cb113fd` |

**応答**

正常な応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルでは、APIを使用して[!DNL Google Cloud Storage]接続を作成し、応答本文の一部として一意のIDを取得しました。 この接続IDを使用して[フローサービスAPI](../../explore/cloud-storage.md)または[フローサービスAPI](../../cloud-storage-parquet.md)を使用してParquetデータを取り込み、クラウドストレージを調べることができます。
