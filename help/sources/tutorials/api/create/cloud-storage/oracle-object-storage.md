---
keywords: Experience Platform；ホーム；人気のトピック；Oracle オブジェクトストレージ；oracle オブジェクトストレージ
solution: Experience Platform
title: Flow Service API を使用したOracle オブジェクトのストレージベース接続の作成
type: Tutorial
description: Flow Service API を使用してAdobe Experience PlatformをOracle オブジェクトストレージに接続する方法について説明します。
exl-id: a85faa44-7d5a-42a2-9052-af01744e13c9
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 31%

---

# [!DNL Flow Service] API を使用した [!DNL Oracle Object Storage] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Oracle Object Storage] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL Oracle Object Storage] ています。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Oracle Object Storage] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `serviceUrl` | 認証に必要な [!DNL Oracle Object Storage] エンドポイント。 エンドポイントの形式は `https://{OBJECT_STORAGE_NAMESPACE}.compat.objectstorage.eu-frankfurt-1.oraclecloud.com` です。 |
| `accessKey` | 認証に必要な [!DNL Oracle Object Storage] アクセスキー ID。 |
| `secretKey` | 認証に必要な [!DNL Oracle Object Storage] パスワード。 |
| `bucketName` | ユーザーがアクセスを制限している場合に必要な、許可されたバケット名。 バケット名は 3 ～ 63 文字で、先頭と末尾は文字または数字である必要があります。また、小文字、数字、ハイフン （`-`）のみを含めることができます。 バケット名を IP アドレスのようにフォーマットすることはできません。 |
| `folderPath` | ユーザーがアクセスを制限している場合は、許可されたフォルダーパスが必要です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Oracle Object Storage] の接続仕様 ID は `c85f9425-fb21-426c-ad0b-405e9bd8a46c` です。 |

これらの値の取得方法について詳しくは、[Oracle オブジェクトストレージ認証ガイド ](https://docs.oracle.com/en-us/iaas/Content/Identity/Concepts/usercredentials.htm#User_Credentials) を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

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
| `auth.params.serviceUrl` | 認証に必要な [!DNL Oracle Object Storage] エンドポイント。 |
| `auth.params.accessKey` | 認証に必要な [!DNL Oracle Object Storage] アクセスキー ID。 |
| `auth.params.secretKey` | 認証に必要な [!DNL Oracle Object Storage] パスワード。 |
| `auth.params.bucketName` | ユーザーがアクセスを制限している場合に必要な、許可されたバケット名。 |
| `auth.params.folderPath` | ユーザーがアクセスを制限している場合は、許可されたフォルダーパスが必要です。 |
| `connectionSpec.id` | [!DNL Oracle Object Storage] 接続仕様 ID：`c85f9425-fb21-426c-ad0b-405e9bd8a46c`。 |

**応答**

リクエストが成功した場合は、新しく作成した接続の接続 ID が返されます。 この ID は、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Oracle Object Storage] 接続を作成し、その一意の接続 ID を取得しました。 この接続 ID を使用して [Flow Service API を使用したクラウドストレージの調査 ](../../explore/cloud-storage.md) を行うことができます。
