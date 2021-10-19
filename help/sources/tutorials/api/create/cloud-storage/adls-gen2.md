---
keywords: エクスペリエンス Platform、home、人気のある話題。Azure Data Lake Storage Gen2。 azure data lake storage。Backup
solution: Experience Platform
title: フローサービス API を使用した Azure Data Lake Storage Gen2 ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して、Adobe エクスペリエンスプラットフォームを Azure Data Lake Storage Gen2 に接続する方法について説明します。
exl-id: cad5e2a0-e27c-4130-9ad8-888352c92f04
source-git-commit: 13bd1254dfe89004465174a7532b4f6aaef54c09
workflow-type: tm+mt
source-wordcount: '511'
ht-degree: 6%

---

# [!DNL Azure Data Lake Storage Gen2]API を使用したベース接続の作成 [!DNL Flow Service]

ベース接続は、ソースと Adobe エクスペリエンスプラットフォームとの間の認証された接続を表します。

このチュートリアルでは、 [!DNL Azure Data Lake Storage Gen2] API を使用して、(hereinafter と呼ばれる) の基本的な接続を作成する手順について説明し [[!DNL Flow Service]  ](https://www.adobe.io/experience-platform-apis/references/flow-service/) ます。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース ](../../../../home.md) : [!DNL Experience Platform] 多種多様なソースからのデータの ingested を可能にするとともに、サービスを使用した受信データを構造化、ラベル付け、拡張するための機能を提供し [!DNL Platform] ます。
* [サンドボックス ](../../../../../sandboxes/home.md) : [!DNL Experience Platform] 仮想サンドボックスを使用して、1つのプラットフォームインスタンスを個別の仮想環境に分割します。これにより、デジタルエクスペリエンスアプリケーションを開発および発展させるのに役立ちます。

以下の各セクションでは、この API を使用して ADLS Gen2 ソース接続を正常に作成するために必要な追加情報を示し [!DNL Flow Service] ます。

### 必要な資格情報の収集

[!DNL Flow Service]ADLS Gen2 に接続するには、次の接続プロパティの値を指定する必要があります。

| Chap | 説明 |
| ---------- | ----------- |
| `url` | ADLS Gen2 のエンドポイント。 エンドポイントパターンは、次のとおりです `https://<accountname>.dfs.core.windows.net` 。 |
| `servicePrincipalId` | アプリケーションのクライアント ID。 |
| `servicePrincipalKey` | アプリケーションのキー。 |
| `tenant` | アプリケーションが含まれているテナント情報を表示します。 |
| `connectionSpec.id` | コネクション仕様は、ベースおよびソース接続の作成に関連付けられた認証仕様を含む、ソースのコネクタプロパティを返します。 ADLS Gen2 の接続指定 ID は、次のとおりです `0ed90a81-07f4-4586-8190-b40eccef1c5a` 。 |

これらの値について詳しくは、ADLS Gen2 のマニュアルを参照してください [ ](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-storage) 。

### プラットフォーム Api の使用

プラットフォーム Api の呼び出しを適切に行う方法については、Platform Api の概要を参照してください [ ](../../../../../landing/api-guide.md) 。

## ベース接続の作成

ベース接続を行うと、ソースとプラットフォームの間の情報が保持されます。ソースの認証の資格情報、接続の現在の状態、および一意の基本接続 ID が含まれています。 ベース接続 ID を使用して、ソース内でファイルを検索してナビゲートし、データの種類とフォーマットに関する情報も含めて、取り込む特定のアイテムを指定することができます。

ベース接続 ID を作成するには、エンドポイントへの POST 要求を行いますが、 `/connections` Gen2 認証資格情報を要求パラメーターの一部として提供します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次の要求によって、ADLS Gen2 の基本的な接続が作成されます。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
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
            "id": "0ed90a81-07f4-4586-8190-b40eccef1c5a",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.url` | ADLS Gen2 アカウントの URL エンドポイント。 |
| `auth.params.servicePrincipalId` | ADLS Gen2 アカウントのサービスプリンシパル ID。 |
| `auth.params.servicePrincipalKey` | ADLS Gen2 アカウントのサービスプリンシパルキー |
| `auth.params.tenant` | ADLS Gen2 アカウントのテナント情報 |
| `connectionSpec.id` | ADLS Gen2 コネクションスペシフィケーション ID: `0ed90a81-07f4-4586-8190-b40eccef1c5a1` . |

**応答**

応答が成功した場合は、一意の識別子 () を含む、新しく作成されたベース接続の詳細が返され `id` ます。 この ID は、次の手順に従ってソース接続を作成する必要があります。

```json
{
    "id": "7497ad71-6d32-4973-97ad-716d32797304",
    "etag": "\"23005f80-0000-0200-0000-5e1d00a20000\""
}
```

## 次の手順

このチュートリアルでは、Api を使用して ADLS Gen2 接続を作成し、応答本体の一部として一意の ID を取得しました。 この接続 ID を使用し [ て、フローサービス API を使用してクラウドストレージを探すことができ ](../../explore/cloud-storage.md) ます。
