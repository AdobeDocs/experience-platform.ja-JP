---
keywords: Experience Platform；ホーム；人気のトピック；Oracleオブジェクトストレージ；oracleオブジェクトストレージ
solution: Experience Platform
title: フローサービス API を使用したOracleオブジェクトストレージベース接続の作成
type: Tutorial
description: フローサービス API を使用してAdobe Experience PlatformをOracleオブジェクトストレージに接続する方法を説明します。
exl-id: a85faa44-7d5a-42a2-9052-af01744e13c9
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 50%

---

# の作成 [!DNL Oracle Object Storage] を使用したベース接続 [!DNL Flow Service] API

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Oracle Object Storage] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：Experience Platform を使用すると、様々なソースからデータを取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL Oracle Object Storage] の使用 [!DNL Flow Service] API

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL Oracle Object Storage] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `serviceUrl` | この [!DNL Oracle Object Storage] 認証に必要なエンドポイント。 エンドポイントの形式は次のとおりです。 `https://{OBJECT_STORAGE_NAMESPACE}.compat.objectstorage.eu-frankfurt-1.oraclecloud.com` |
| `accessKey` | この [!DNL Oracle Object Storage] 認証に必要なアクセスキー ID。 |
| `secretKey` | この [!DNL Oracle Object Storage] 認証に必要なパスワード。 |
| `bucketName` | ユーザーがアクセスを制限している場合に必要な許可されたバケット名です。 バケット名は、3～63 文字で、先頭と末尾には文字または数字を使用し、小文字、数字、ハイフン (`-`) をクリックします。 バケット名は IP アドレスと同じ形式にできません。 |
| `folderPath` | ユーザーがアクセスを制限している場合に許可されるフォルダーパス。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Oracle Object Storage] の接続仕様 ID は `c85f9425-fb21-426c-ad0b-405e9bd8a46c` です。 |

これらの値の取得方法について詳しくは、 [Oracleオブジェクトストレージ認証ガイド](https://docs.oracle.com/en-us/iaas/Content/Identity/Concepts/usercredentials.htm#User_Credentials).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Oracle Object Storage] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Oracle Object Storage] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `auth.params.serviceUrl` | この [!DNL Oracle Object Storage] 認証に必要なエンドポイント。 |
| `auth.params.accessKey` | この [!DNL Oracle Object Storage] 認証に必要なアクセスキー ID。 |
| `auth.params.secretKey` | この [!DNL Oracle Object Storage] 認証に必要なパスワード。 |
| `auth.params.bucketName` | ユーザーがアクセスを制限している場合に必要な許可されたバケット名です。 |
| `auth.params.folderPath` | ユーザーがアクセスを制限している場合に許可されるフォルダーパス。 |
| `connectionSpec.id` | [!DNL Oracle Object Storage] 接続仕様 ID：`c85f9425-fb21-426c-ad0b-405e9bd8a46c`。 |

**応答**

正常な応答は、新しく作成された接続の接続 ID を返します。 この ID は、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Oracle Object Storage] を使用した接続 [!DNL Flow Service] API と呼ばれ、一意の接続 ID を取得している。 この接続 ID を [フローサービス API を使用したクラウドストレージの調査](../../explore/cloud-storage.md).
