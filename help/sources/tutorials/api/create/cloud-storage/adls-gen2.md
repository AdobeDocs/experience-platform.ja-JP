---
keywords: Experience Platform；ホーム；人気のあるトピック；Azure Data Lake Storage Gen2;azure data lake storage;Azure
solution: Experience Platform
title: フローサービス API を使用した Azure データレイクストレージ Gen2 ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを Azure Data Lake Storage Gen2 に接続する方法を説明します。
exl-id: cad5e2a0-e27c-4130-9ad8-888352c92f04
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '520'
ht-degree: 6%

---

# [!DNL Flow Service] API を使用して [!DNL Azure Data Lake Storage Gen2] ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Azure Data Lake Storage Gen2]（以下「ADLS Gen2」と呼ばれる）の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して ADLS Gen2 ソース接続を正しく作成するために必要な追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が ADLS Gen2 に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `url` | ADLS Gen2 のエンドポイント。 終了点のパターンは次のとおりです。`https://<accountname>.dfs.core.windows.net`. |
| `servicePrincipalId` | アプリケーションのクライアント ID。 |
| `servicePrincipalKey` | アプリケーションのキー。 |
| `tenant` | アプリケーションを含むテナント情報。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 ADLS Gen2 の接続仕様 ID は次のとおりです。`0ed90a81-07f4-4586-8190-b40eccef1c5a`. |

これらの値の詳細は、[ この ADLS Gen2 ドキュメント ](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-data-lake-storage) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、リクエストパラメーターの一部として ADLS Gen2 認証資格情報を指定しながら、`/connections` エンドポイントに接続リクエストを送信します。

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
| `auth.params.servicePrincipalId` | ADLS Gen2 アカウントのサービスプリンシパル ID です。 |
| `auth.params.servicePrincipalKey` | ADLS Gen2 アカウントのサービスプリンシパルキー。 |
| `auth.params.tenant` | ADLS Gen2 アカウントのテナント情報。 |
| `connectionSpec.id` | ADLS Gen2 接続仕様 ID:`0ed90a81-07f4-4586-8190-b40eccef1c5a1`. |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 この ID は、次の手順でソース接続を作成する際に必要です。

```json
{
    "id": "7497ad71-6d32-4973-97ad-716d32797304",
    "etag": "\"23005f80-0000-0200-0000-5e1d00a20000\""
}
```

## 次の手順

このチュートリアルでは、API を使用して ADLS Gen2 接続を作成し、応答本文の一部として一意の ID を取得しました。 この接続 ID を使用して [ フローサービス API](../../explore/cloud-storage.md) または [ フローサービス API](../../cloud-storage-parquet.md) を使用して Parquet データを取り込み、クラウドストレージを調べることができます。
