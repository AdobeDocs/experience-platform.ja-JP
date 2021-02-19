---
keywords: Experience Platform；ホーム；人気の高いトピック；ファイル転送プロトコル；ファイル転送プロトコル
solution: Experience Platform
title: フローサービスAPIを使用したFTPソース接続の作成
topic: overview
type: Tutorial
description: Flow Service APIを使用して、Adobe Experience PlatformをFTP（ファイル転送プロトコル）サーバーに接続する方法を説明します。
translation-type: tm+mt
source-git-commit: a489ab248793a063295578943ad600d8eacab6a2
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 26%

---


# [!DNL Flow Service] APIを使用したFTPソース接続の作成

>[!NOTE]
>
>FTPコネクタはベータ版です。 機能とドキュメントは変更される場合があります。ベータラベル付きコネクタの使用方法の詳細については、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Experience Platform]をFTP(File Transfer Protocol)サーバに接続する手順を順を追って説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを個別の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

[!DNL Flow Service] APIを使用してFTPサーバに正しく接続するために必要な追加情報については、以下の節で説明します。

### 必要な資格情報の収集

[!DNL Flow Service]がFTPに接続するには、次の接続プロパティの値を指定する必要があります。

| Credential | 説明 |
| ---------- | ----------- |
| `host` | FTPサーバーに関連付けられている名前またはIPアドレス。 |
| `username` | FTPサーバーへのアクセス権を持つユーザー名。 |
| `password` | FTPサーバーのパスワードです。 |

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Experience Platform]内のすべてのリソース（[!DNL Flow Service]に属するリソースを含む）は、特定の仮想サンドボックスに分離されます。 [!DNL Platform] APIへのすべてのリクエストには、操作が行われるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続は、ソースを指定し、そのソースの資格情報を含みます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、FTPアカウントごとに1つの接続が必要です。

### 基本認証を使用したFTP接続の作成

基本的な認証を使用してFTPPOSTを作成するには、[!DNL Flow Service] APIに接続リクエストを行い、接続の`host`、`userName`、`password`に値を指定します。

**API 形式**

```http
POST /connections
```

**リクエスト**

FTP接続を作成するには、POSTリクエストの一部として、一意の接続指定IDを指定する必要があります。 FTPの接続指定IDは`fb2e94c9-c031-467d-8103-6bd6e0a432f2`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d  '{
        "name": "FTP connector with password",
        "description": "FTP connector password",
        "auth": {
            "specName": "Basic Authentication for FTP",
            "params": {
                "host": "{HOST}",
                "userName": "{USERNAME}",
                "password": "{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "fb2e94c9-c031-467d-8103-6bd6e0a432f2",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.host` | FTPサーバーのホスト名です。 |
| `auth.params.username` | FTPサーバに関連付けられているユーザ名。 |
| `auth.params.password` | FTPサーバーに関連付けられているパスワードです。 |
| `connectionSpec.id` | FTPサーバー接続仕様ID:`fb2e94c9-c031-467d-8103-6bd6e0a432f2` |

**応答** 

正常な応答は、新たに作成された接続の固有な識別子(`id`)を返します。 このIDは、次のチュートリアルでFTPサーバを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

## 次の手順

このチュートリアルに従うと、[!DNL Flow Service] APIを使用してFTP接続を作成し、接続の固有のID値を取得したことになります。 この接続IDを使用して、Flow Service API](../../explore/cloud-storage.md)または[Flow Service API](../../cloud-storage-parquet.md)を使用してParketストレージを[調査できます。
