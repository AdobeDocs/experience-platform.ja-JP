---
keywords: Experience Platform；ホーム；人気のあるトピック；Vertica;vertica
solution: Experience Platform
title: フローサービス API を使用した HP Vertica ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して HP Vertica をAdobe Experience Platformに接続する方法を説明します。
exl-id: 37f831c1-7c82-462a-8338-a0bcaaf08cd1
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 11%

---

# [!DNL Flow Service] API を使用して [!DNL HP Vertica] ベース接続を作成する

>[!NOTE]
>
>[!DNL HP Vertica] コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL HP Vertica] の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)[!DNL Platform]：Experience は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL HP Vertica] に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL HP Vertica] と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL HP Vertica] インスタンスへの接続に使用する接続文字列。 [!DNL HP Vertica] の接続文字列パターンは `Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}` です |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL HP Vertica] の接続仕様 ID は次のとおりです。`a8b6a1a4-5735-42b4-952c-85dce0ac38b5` |

接続文字列の取得の詳細については、[ この HP Vertica ドキュメント ](https://www.vertica.com/docs/9.2.x/HTML/Content/Authoring/ConnectingToVertica/ClientJDBC/CreatingAndConfiguringAConnection.htm) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL HP Vertica] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL HP Vertica] のベース接続を作成します。


```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Connection for HP Vertica",
        "description": "Connection for HP Vertica",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                "connectionString": "Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "a8b6a1a4-5735-42b4-952c-85dce0ac38b5",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | [!DNL HP Vertica] アカウントに関連付けられた接続文字列。 [!DNL HP Vertica] の接続文字列パターンは次のとおりです。`Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | [!DNL HP Vertica] 接続仕様 ID:`a8b6a1a4-5735-42b4-952c-85dce0ac38b5`. |

**応答**

正常な応答は、新しく作成された接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6bc13a3b-3546-455f-813a-3b3546a55fb1",
    "etag": "\"3500866c-0000-0200-0000-5e83afa30000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL HP Vertica] 接続を作成し、接続の一意の ID 値を取得しました。 この ID は、次のチュートリアルでフローサービス API](../../explore/database-nosql.md) を使用してデータベースを調べる方法を学ぶ際に使用できます。[
