---
keywords: Experience Platform；ホーム；人気の高いトピック；イベントハブ；Azure イベントハブ；イベントハブ
solution: Experience Platform
title: フローサービス API を使用した Azure Event Hubs ソース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを Azure Event Hubs アカウントに接続する方法を説明します。
exl-id: a4d0662d-06e3-44f3-8cb7-4a829c44f4d9
source-git-commit: 27e5c64f31b9a68252d262b531660811a0576177
workflow-type: tm+mt
source-wordcount: '737'
ht-degree: 8%

---


# の作成 [!DNL Azure Event Hubs] を使用したソース接続 [!DNL Flow Service] API

このチュートリアルでは、接続の手順について説明します [!DNL Azure Event Hubs] （以下「」という。）[!DNL Event Hubs]&quot;) をExperience Platform、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

- [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、次のコードを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。
- [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

次の節では、接続を成功させるために知っておく必要がある追加情報を示します [!DNL Event Hubs] を使用して Platform に [!DNL Flow Service] API

### 必要な資格情報の収集

次のために [!DNL Flow Service] を [!DNL Event Hubs] アカウントの場合、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `sasKeyName` | 認証規則の名前。SAS キー名とも呼ばれます。 |
| `sasKey` | のプライマリキー [!DNL Event Hubs] 名前空間。 この `sasPolicy` この `sasKey` 必ず～に対応する `manage` 次に対して設定された権限： [!DNL Event Hubs] リストに値を入力します。 |
| `namespace` | の名前空間 [!DNL Event Hubs] にアクセスしています。 An [!DNL Event Hubs] 名前空間は、1 つ以上のスコーピングコンテナを作成できる一意のスコーピングコンテナを提供します [!DNL Event Hubs]. |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 この [!DNL Event Hubs] 接続仕様 ID: `bf9f5905-92b7-48bf-bf20-455bc6b60a4e`. |

これらの値について詳しくは、 [この Event Hubs ドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、 [Platform API の概要](../../../../../landing/api-guide.md).

## ベース接続を作成する

ソース接続を作成する最初の手順は、 [!DNL Event Hubs] ソースおよびベース接続 ID を生成します。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

ベース接続 ID を作成するには、 `/connections` エンドポイントを [!DNL Event Hubs] 認証資格情報をリクエストパラメーターの一部として使用します。

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
        "name": "Azure Event Hubs connection",
        "description": "Connector for Azure Event Hubs",
        "auth": {
            "specName": "Azure EventHub authentication credentials",
            "params": {
                "sasKeyName": "{SAS_KEY_NAME}",
                "sasKey": "{SAS_KEY}",
                "namespace": "{NAMESPACE}"
            }
        },
        "connectionSpec": {
            "id": "bf9f5905-92b7-48bf-bf20-455bc6b60a4e",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.sasKeyName` | 認証規則の名前。SAS キー名とも呼ばれます。 |
| `auth.params.sasKey` | 生成された共有アクセス署名。 |
| `auth.params.namespace` | の名前空間 [!DNL Event Hubs] にアクセスしています。 |
| `connectionSpec.id` | この [!DNL Event Hubs] 接続仕様 ID: `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` |

**応答**

正常な応答は、新しく作成されたベース接続の詳細 ( 一意の識別子 (`id`) をクリックします。 この接続 ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cdbb15c-fb1e-46ee-8049-0f55b53378fe",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## ソース接続の作成

ソース接続は、データの取り込み元となる外部ソースへの接続を作成および管理します。 ソース接続は、データソース、データ形式、データフローの作成に必要なソース接続 ID などの情報で構成されます。 ソース接続インスタンスは、テナントと IMS 組織に固有です。

ソース接続を作成するには、 `/sourceConnections` エンドポイント [!DNL Flow Service] API

**API 形式**

```http
POST /sourceConnections
```

**リクエスト**

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/sourceConnections' \
    -H 'authorization: Bearer {ACCESS_TOKEN}' \
    -H 'content-type: application/json' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_Org}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -d '{
        "name": "Azure Event Hubs source connection",
        "description": "A source connection for Azure Event Hubs",
        "baseConnectionId": "4cdbb15c-fb1e-46ee-8049-0f55b53378fe",
        "connectionSpec": {
            "id": "bf9f5905-92b7-48bf-bf20-455bc6b60a4e",
            "version": "1.0"
        },
        "data": {
            "format": "json"
        },
        "params": {
            "eventHubName": "{EVENTHUB_NAME}",
            "dataType": "raw",
            "reset": "latest",
            "consumerGroup": "{CONSUMER_GROUP}"
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前がわかりやすい名前になっていることを確認します。 |
| `description` | ソース接続に関する詳細情報を含めるために指定できるオプションの値です。 |
| `baseConnectionId` | の接続 ID [!DNL Event Hubs] 前の手順で生成されたソース。 |
| `connectionSpec.id` | の固定接続仕様 ID [!DNL Event Hubs]. この ID は次のとおりです。 `bf9f5905-92b7-48bf-bf20-455bc6b60a4e`. |
| `data.format` | の形式 [!DNL Event Hubs] 取り込むデータ。 現在、サポートされているデータ形式は次のみです。 `json`. |
| `params.eventHubName` | の名前 [!DNL Event Hubs] ソース。 |
| `params.dataType` | このパラメーターは、取り込まれるデータのタイプを定義します。 次のようなデータタイプがサポートされています。 `raw` および `xdm`. |
| `params.reset` | このパラメーターは、データの読み取り方法を定義します。 用途 `latest` 最新のデータから読み込みを開始するには、 `earliest` をクリックして、ストリーム内の最初の使用可能なデータから読み取りを開始します。 このパラメーターはオプションで、デフォルトはです。 `earliest` 指定されていない場合は。 |
| `params.consumerGroup` | 使用する公開または購読のメカニズム [!DNL Event Hubs]. このパラメーターはオプションで、デフォルトはです。 `$Default` 指定されていない場合は。 詳しくは、 [[!DNL Event Hubs] イベント消費者向けガイド](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-features#event-consumers) を参照してください。 |

## 次の手順

このチュートリアルに従って、 [!DNL Event Hubs] を使用したソース接続 [!DNL Flow Service] API 次のチュートリアルでは、このソース接続 ID を使用して、 [を使用してストリーミングデータフローを作成する [!DNL Flow Service] API](../../collect/streaming.md).
