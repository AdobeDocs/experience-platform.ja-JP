---
keywords: Experience Platform;home;popular topics;SFTP;sftp;Secure File Transfer Protocol;secure file transfer protocol
solution: Experience Platform
title: Flow Service APIを使用したSFTPコネクタの作成
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用して、Experience PlatformをSFTP(Secure File Transfer Protocol)サーバーに接続する手順を順を追って説明します。
translation-type: tm+mt
source-git-commit: 7b638f0516804e6a2dbae3982d6284a958230f42
workflow-type: tm+mt
source-wordcount: '749'
ht-degree: 20%

---


# APIを使用したSFTPコネクタの作成 [!DNL Flow Service]

>[!NOTE]
>
>SFTPコネクタはベータ版です。 機能とドキュメントは変更される場合があります。ベータラベル付きのコネクタの使用について詳しくは、 [ソースの概要](../../../../home.md#terms-and-conditions) 「」を参照してください。

このチュートリアルでは、 [!DNL Flow Service] APIを使用して、Experience PlatformをSFTP(Secure File Transfer Protocol)サーバに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込むことができ、Platform Servicesを使用して、データの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully connect to an SFTP server using the [!DNL Flow Service] API.

### 必要な資格情報の収集

SFTPに接続 [!DNL Flow Service] するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | SFTPサーバーに関連付けられている名前またはIPアドレス。 |
| `username` | SFTPサーバーへのアクセス権を持つユーザー名。 |
| `password` | SFTPサーバーのパスワードです。 |
| `privateKeyContent` | Base64エンコードされたSSH秘密鍵のコンテンツ。 SSH秘密鍵のOpenSSH(RSA/DSA)形式。 |
| `passPhrase` | 鍵ファイルや鍵の内容がパスフレーズで保護されている場合に、秘密鍵を復号化するためのパスフレーズまたはパスワード。 PrivateKeyContentがパスワードで保護されている場合は、PrivateKeyContentのパスフレーズを値として使用する必要があります。 |

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../../../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* Authorization: Bearer `{ACCESS_TOKEN}`
* x-api-key: `{API_KEY}`
* x-gw-ims-org-id: `{IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to the [!DNL Flow Service], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* x-sandbox-name: `{SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* Content-Type: `application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、SFTPアカウントごとに必要な接続は1つだけです。

### 基本認証を使用したSFTP接続の作成

基本的な認証を使用してSFTP接続を作成するには、接続の、およびに値を指定しながら、 [!DNL Flow Service] APIにPOSTリクエストを行い `host`ま `userName`す `password`。

**API 形式**

```http
POST /connections
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d  '{
        "name": "SFTP connector with password",
        "description": "SFTP connector password",
        "auth": {
            "specName": "Basic Authentication for sftp",
            "params": {
                "host": "{HOST}",
                "userName": "{USERNAME}",
                "password": "{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "b7bf2577-4520-42c9-bae9-cad01560f7bc",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.host` | SFTPサーバーのホスト名です。 |
| `auth.params.username` | SFTPサーバーに関連付けられているユーザー名です。 |
| `auth.params.password` | SFTPサーバーに関連付けられているパスワードです。 |
| `connectionSpec.id` | SFTPサーバー接続仕様ID: `b7bf2577-4520-42c9-bae9-cad01560f7bc` |

**応答** 

正常な応答は、新たに作成された接続の固有な識別子(`id`)を返します。 このIDは、次のチュートリアルでSFTPサーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

### SSH公開鍵認証を使用したSFTP接続の作成

SSH公開鍵認証を使用してSFTPPOSTを作成するには、接続の [!DNL Flow Service] 、 `host`、およびに値を指定しながら、 `userName`APIに接続リクエストを行いま `privateKeyContent``passPhrase`す。

**API 形式**

```http
POST /connections
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "SFTP connector with SSH authentication",
        "description": "SFTP connector with SSH authentication",
        "auth": {
            "specName": "SSH PublicKey Authentication for sftp",
            "params": {
                "host": "{HOST}",
                "userName": "{USERNAME}",
                "privateKeyContent": "{PRIVATE_KEY_CONTENT}",
                "passPhrase": "{PASSPHRASE}"
            }
        },
        "connectionSpec": {
            "id": "b7bf2577-4520-42c9-bae9-cad01560f7bc",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.host` | SFTPサーバーのホスト名です。 |
| `auth.params.username` | SFTPサーバーに関連付けられているユーザー名です。 |
| `auth.params.privateKeyContent` | base64エンコードされたSSH秘密鍵のコンテンツ。 SSH秘密鍵のOpenSSH(RSA/DSA)形式。 |
| `auth.params.passPhrase` | 鍵ファイルや鍵の内容がパスフレーズで保護されている場合に、秘密鍵を復号化するためのパスフレーズまたはパスワード。 PrivateKeyContentがパスワードで保護されている場合は、PrivateKeyContentのパスフレーズを値として使用する必要があります。 |
| `connectionSpec.id` | SFTPサーバー接続仕様ID: `b7bf2577-4520-42c9-bae9-cad01560f7bc` |

**応答** 

正常な応答は、新たに作成された接続の固有な識別子(`id`)を返します。 このIDは、次のチュートリアルでSFTPサーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

## 次の手順

このチュートリアルに従うことで、 [!DNL Flow Service] APIを使用してSFTP接続を作成し、接続の一意のID値を取得したことになります。 この接続IDを使用して、Flow Service APIを使用してクラウドストレージを [調べたり、Flow Service APIを使用してパーケーデータを](../../explore/cloud-storage.md) 取り込んだりできます [](../../cloud-storage-parquet.md)。
