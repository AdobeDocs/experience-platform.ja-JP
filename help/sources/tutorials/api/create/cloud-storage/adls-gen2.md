---
keywords: Experience Platform；ホーム；人気のトピック；Azure Data Lake Storage Gen2;azure data lake storage;Azure
solution: Experience Platform
title: Flow Service API を使用した Azure Data Lake Storage Gen2 ベース接続の作成
type: Tutorial
description: Flow Service API を使用してAdobe Experience Platformを Azure Data Lake Storage Gen2 に接続する方法を説明します。
exl-id: cad5e2a0-e27c-4130-9ad8-888352c92f04
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 30%

---

# [!DNL Flow Service] API を使用した [!DNL Azure Data Lake Storage Gen2] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Azure Data Lake Storage Gen2] のベース接続（以下「ADLS Gen2」と呼びます）を作成する手順について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md):[!DNL Experience Platform] には、単一のExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して ADLS Gen2 ソース接続を正しく作成するために必要な追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] を ADLS Gen2 に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `url` | ADLS Gen2 のエンドポイント。 エンドポイントパターンは `https://<accountname>.dfs.core.windows.net` です。 |
| `servicePrincipalId` | アプリケーションのクライアント ID。 |
| `servicePrincipalKey` | アプリケーションのキー。 |
| `tenant` | アプリケーションを含むテナント情報。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。ADLS Gen2 の接続仕様 ID は `b3ba5556-48be-44b7-8b85-ff2b69b46dc4` です。 |

これらの値の詳細については、[&#x200B; この ADLS Gen2 ドキュメント &#x200B;](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-storage) を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に ADLS Gen2 認証資格情報をリクエストパラメーターの一部として指定します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、ADLS Gen2 のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "adls-gen2",
        "description": "Connection for adls-gen2",
        "auth": {
            "specName": "Basic Authentication for adls-gen2",
            "params": {
                "url": "{URL}",
                "servicePrincipalId": "{SERVICE_PRINCIPAL_ID}",
                "servicePrincipalKey": "{SERVICE_PRINCIPAL_KEY}",
                "tenant": "{TENANT}"
            }
        },
        "connectionSpec": {
            "id": "b3ba5556-48be-44b7-8b85-ff2b69b46dc4",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.url` | ADLS Gen2 アカウントの URL エンドポイント。 |
| `auth.params.servicePrincipalId` | ADLS Gen2 アカウントのサービスプリンシパル ID。 |
| `auth.params.servicePrincipalKey` | ADLS Gen2 アカウントのサービスプリンシパルキー。 |
| `auth.params.tenant` | ADLS Gen2 アカウントのテナント情報。 |
| `connectionSpec.id` | ADLS Gen2 接続仕様 ID: `b3ba5556-48be-44b7-8b85-ff2b69b46dc41`。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "7497ad71-6d32-4973-97ad-716d32797304",
    "etag": "\"23005f80-0000-0200-0000-5e1d00a20000\""
}
```

## 次の手順

このチュートリアルでは、API を使用して ADLS Gen2 接続を作成し、一意の ID を応答本文の一部として取得しました。 この接続 ID を使用して [Flow Service API を使用したクラウドストレージの調査 &#x200B;](../../explore/cloud-storage.md) を行うことができます。
