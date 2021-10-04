---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure;Azure File Storage;Azure ファイルストレージ
solution: Experience Platform
title: フローサービス API を使用した Azure ファイルストレージベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して Azure File Storage をAdobe Experience Platformに接続する方法を説明します。
exl-id: 0c585ae2-be2d-4167-b04b-836f7e2c04a9
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '473'
ht-degree: 11%

---

# [!DNL Flow Service] API を使用して [!DNL Azure File Storage] ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Azure File Storage] の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL Azure File Storage] に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL Azure File Storage] と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | アクセスする [!DNL Azure File Storag] インスタンスのエンドポイント。 |
| `userId` | [!DNL Azure File Storage] エンドポイントへの十分なアクセス権を持つユーザー。 |
| `password` | [!DNL Azure File Storage] インスタンスのパスワード |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Azure File Storage] の接続仕様 ID は次のとおりです。`be5ec48c-5b78-49d5-b8fa-7c89ec4569b8`. |

使い始める方法については、[ この Azure File Storage のドキュメント ](https://docs.microsoft.com/en-us/azure/storage/files/storage-how-to-use-files-windows) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL Azure File Storage] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

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
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
| `auth.params.host` | アクセスする [!DNL Azure File Storage] インスタンスのエンドポイント。. |
| `auth.params.userId` | [!DNL Azure File Storage] エンドポイントへの十分なアクセス権を持つユーザー。 |
| `auth.params.password` | [!DNL Azure File Storage] アクセスキー。 |
| `connectionSpec.id` | [!DNL Azure File Storage] 接続仕様 ID:`be5ec48c-5b78-49d5-b8fa-7c89ec4569b8`. |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次の手順でソース接続を作成する際に必要です。

```json
{
    "id": "f9377f50-607a-4818-b77f-50607a181860",
    "etag": "\"2f0276fa-0000-0200-0000-5eab3abb0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Azure File Storage] 接続を作成し、接続の一意の ID 値を取得しました。 この ID は、次のチュートリアルで [ フローサービス API](../../explore/cloud-storage.md) を使用してサードパーティのクラウドストレージを調べる方法を学ぶ際に使用できます。
