---
keywords: Experience Platform；ホーム；人気のあるトピック；Oracleオブジェクトストレージ；oracleオブジェクトストレージ
solution: Experience Platform
title: フローサービスAPIを使用したOracleオブジェクトストレージベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAdobe Experience PlatformをOracleオブジェクトストレージに接続する方法を説明します。
exl-id: a85faa44-7d5a-42a2-9052-af01744e13c9
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 9%

---

# [!DNL Flow Service] APIを使用して[!DNL Oracle Object Storage]ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Oracle Object Storage]の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platformサービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL Oracle Object Storage]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Oracle Object Storage]に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `serviceUrl` | 認証に必要な[!DNL Oracle Object Storage]エンドポイント。 エンドポイントの形式は次のとおりです。`https://{OBJECT_STORAGE_NAMESPACE}.compat.objectstorage.eu-frankfurt-1.oraclecloud.com` |
| `accessKey` | 認証に必要な[!DNL Oracle Object Storage]アクセスキーID。 |
| `secretKey` | 認証に必要な[!DNL Oracle Object Storage]パスワード。 |
| `bucketName` | ユーザーがアクセスを制限している場合に必要な許可されたバケット名。 バケット名は、3～63文字で、先頭と末尾には文字または数字を使用し、小文字、数字、ハイフン(`-`)のみを含める必要があります。 バケット名は、IPアドレスのような形式にはできません。 |
| `folderPath` | ユーザーがアクセスを制限している場合に必要な許可されたフォルダーパス。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Oracle Object Storage]の接続仕様IDは次のとおりです。`c85f9425-fb21-426c-ad0b-405e9bd8a46c`. |

これらの値の取得方法の詳細については、『[Oracleオブジェクトストレージ認証ガイド](https://docs.oracle.com/en-us/iaas/Content/Identity/Concepts/usercredentials.htm#User_Credentials)』を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL Oracle Object Storage]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Oracle Object Storage]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Oracle Object Storage connection",
        "description": "Oracle Object Storage connection",
        "auth": {
            "specName": "Access Key",
            "params": {
                "serviceUrl": "{SERVICE_URL}",
                "accessKey": "{ACCESS_KEY}",
                "secretKey": "{SECRET_KEY}",
                "bucketName": "{BUCKET_NAME}",
                "folderPath", "{FOLDER_PATH}"
            }
        },
        "connectionSpec": {
            "id": "c85f9425-fb21-426c-ad0b-405e9bd8a46c",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.serviceUrl` | 認証に必要な[!DNL Oracle Object Storage]エンドポイント。 |
| `auth.params.accessKey` | 認証に必要な[!DNL Oracle Object Storage]アクセスキーID。 |
| `auth.params.secretKey` | 認証に必要な[!DNL Oracle Object Storage]パスワード。 |
| `auth.params.bucketName` | ユーザーがアクセスを制限している場合に必要な許可されたバケット名。 |
| `auth.params.folderPath` | ユーザーがアクセスを制限している場合に必要な許可されたフォルダーパス。 |
| `connectionSpec.id` | [!DNL Oracle Object Storage]接続仕様ID:`c85f9425-fb21-426c-ad0b-405e9bd8a46c`. |

**応答**

正常な応答は、新しく作成された接続の接続IDを返します。 このIDは、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Oracle Object Storage]接続を作成し、一意の接続IDを取得しました。 この接続IDを使用して[フローサービスAPI](../../explore/cloud-storage.md)を使用してクラウドストレージを調べることができます。
