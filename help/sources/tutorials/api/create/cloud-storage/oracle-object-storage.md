---
keywords: Experience Platform；ホーム；人気のあるトピック；Oracleオブジェクトストレージ;oracleオブジェクトストレージ
solution: Experience Platform
title: Flow Service APIを使用したOracleオブジェクトストレージソース接続の作成
topic: overview
type: Tutorial
description: Flow Service APIを使用して、Adobe Experience PlatformをOracleオブジェクトストレージに接続する方法を説明します。
translation-type: tm+mt
source-git-commit: c1453a9f0be42f834d35af871051324df8dadf80
workflow-type: tm+mt
source-wordcount: '625'
ht-degree: 28%

---


# [!DNL Flow Service] APIを使用して[!DNL Oracle Object Storage]ソース接続を作成する

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)を使用して、Adobe Experience Platformを[!DNL Oracle Object Storage]に接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込むことができ、Platform Servicesを使用して、データの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用して[!DNL Oracle Object Storage]に正しく接続するために知っておく必要のある追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Oracle Object Storage]に接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `serviceUrl` | [!DNL Oracle Object Storage]エンドポイントは認証に必要です。 エンドポイントの形式は次のとおりです。`https://{OBJECT_STORAGE_NAMESPACE}.compat.objectstorage.eu-frankfurt-1.oraclecloud.com` |
| `accessKey` | 認証に必要な[!DNL Oracle Object Storage]アクセスキーIDです。 |
| `secretKey` | 認証に必要な[!DNL Oracle Object Storage]パスワードです。 |
| `bucketName` | ユーザーが制限付きアクセス権を持つ場合に必要な許可されたバケット名。 バケット名は3 ～ 63文字で、先頭と末尾は文字または数字で、小文字、数字、ハイフン(`-`)のみを含める必要があります。 バケット名は、IPアドレスと同じように形式設定できません。 |
| `folderPath` | ユーザーが制限付きアクセス権を持つ場合に必要な許可されたフォルダーパスです。 |

これらの値の取得方法の詳細については、『[Oracleオブジェクトストレージ認証ガイド](https://docs.oracle.com/en-us/iaas/Content/Identity/Concepts/usercredentials.htm#User_Credentials)』を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、Experience Platform トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。

### 必須ヘッダーの値の収集

Platform API への呼び出しを実行する前に、[認証に関するチュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。認証に関するチュートリアルを完了すると、すべての Experience Platform API 呼び出しで使用する、以下のような各必須ヘッダーの値が提供されます。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成するのに使用できるため、[!DNL Oracle Object Storage]アカウントごとに1つの接続のみが必要です。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL Oracle Object Storage]接続を作成するには、POST要求の一部として一意の接続仕様IDを指定する必要があります。 [!DNL Oracle Object Storage]の接続仕様IDは`c85f9425-fb21-426c-ad0b-405e9bd8a46c`です。

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
| `auth.params.serviceUrl` | [!DNL Oracle Object Storage]エンドポイントは認証に必要です。 |
| `auth.params.accessKey` | 認証に必要な[!DNL Oracle Object Storage]アクセスキーIDです。 |
| `auth.params.secretKey` | 認証に必要な[!DNL Oracle Object Storage]パスワードです。 |
| `auth.params.bucketName` | ユーザーが制限付きアクセス権を持つ場合に必要な許可されたバケット名。 |
| `auth.params.folderPath` | ユーザーが制限付きアクセス権を持つ場合に必要な許可されたフォルダーパスです。 |
| `connectionSpec.id` | [!DNL Oracle Object Storage]接続仕様ID:`c85f9425-fb21-426c-ad0b-405e9bd8a46c`. |

**応答** 

正常に完了した場合は、新たに作成された接続の接続IDが返されます。 このIDは、次のチュートリアルでクラウドストレージデータを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用して[!DNL Oracle Object Storage]接続を作成し、一意の接続IDを取得することができます。 この接続IDを使用して、Flow Service API](../../explore/cloud-storage.md)を使用して、[クラウドストレージを探索できます。