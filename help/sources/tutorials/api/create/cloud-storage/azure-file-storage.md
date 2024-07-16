---
keywords: Experience Platform；ホーム；人気のトピック；Azure;Azure File Storage;Azure file storage
solution: Experience Platform
title: Flow Service API を使用した Azure File Storage ベース接続の作成
type: Tutorial
description: Flow Service API を使用して Azure File Storage をAdobe Experience Platformに接続する方法について説明します。
exl-id: 0c585ae2-be2d-4167-b04b-836f7e2c04a9
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '463'
ht-degree: 63%

---

# [!DNL Flow Service] API を使用した [!DNL Azure File Storage] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Azure File Storage] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL Azure File Storage] ています。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Azure File Storage] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | アクセスする [!DNL Azure File Storag]e インスタンスのエンドポイント。 |
| `userId` | [!DNL Azure File Storage] エンドポイントに十分なアクセス権を持つユーザー。 |
| `password` | [!DNL Azure File Storage] インスタンスのパスワード |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Azure File Storage] の接続仕様 ID は `be5ec48c-5b78-49d5-b8fa-7c89ec4569b8` です。 |

基本について詳しくは、[Azure ファイルストレージのドキュメント ](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-windows) を参照してください。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Azure File Storage] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Azure File Storage] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
        -d '{
        "name": "Azure File Storage connection",
        "description": "An Azure File Storage test connection",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                    "host": "{HOST}",
                    "userId": "{USER_ID}",
                    "password": "{PASSWORD}"
                }
        },
        "connectionSpec": {
            "id": "be5ec48c-5b78-49d5-b8fa-7c89ec4569b8",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.host` | アクセスする [!DNL Azure File Storage] インスタンスのエンドポイント。 |
| `auth.params.userId` | [!DNL Azure File Storage] エンドポイントに十分なアクセス権を持つユーザー。 |
| `auth.params.password` | [!DNL Azure File Storage] アクセスキー。 |
| `connectionSpec.id` | [!DNL Azure File Storage] 接続仕様 ID: `be5ec48c-5b78-49d5-b8fa-7c89ec4569b8`。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "f9377f50-607a-4818-b77f-50607a181860",
    "etag": "\"2f0276fa-0000-0200-0000-5eab3abb0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Azure File Storage] 接続を作成し、接続の一意の ID 値を取得しました。 次のチュートリアルでは、この ID を使用した、[Flow Service API を使用したサードパーティのクラウドストレージの調査 ](../../explore/cloud-storage.md) 方法を説明します。
