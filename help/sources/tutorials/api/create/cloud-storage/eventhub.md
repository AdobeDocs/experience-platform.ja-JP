---
title: フローサービス API を使用した Azure Event Hubs ソース接続の作成
description: フローサービス API を使用してAdobe Experience Platformを Azure Event Hubs アカウントに接続する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: a4d0662d-06e3-44f3-8cb7-4a829c44f4d9
source-git-commit: d22c71fb77655c401f4a336e339aaf8b3125d1b6
workflow-type: tm+mt
source-wordcount: '1005'
ht-degree: 45%

---

# の作成 [!DNL Azure Event Hubs] を使用したソース接続 [!DNL Flow Service] API

>[!IMPORTANT]
>
>The [!DNL Azure Event Hubs] ソースは、Real-time Customer Data Platform Ultimate を購入したユーザーがソースカタログで利用できます。

このチュートリアルでは、 [!DNL Azure Event Hubs] （以下「」という。）[!DNL Event Hubs]&quot;) をExperience Platformに、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

- [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
- [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、[!DNL Flow Service] API を使用して [!DNL Event Hubs] を Platform に正しく接続するために必要な追加情報を示します。

### 必要な資格情報の収集

次の条件を満たすため [!DNL Flow Service] を [!DNL Event Hubs] アカウントの場合、次の接続プロパティの値を指定する必要があります。

>[!BEGINTABS]

>[!TAB 標準認証]

| 資格情報 | 説明 |
| --- | --- |
| `sasKeyName` | 認証規則の名前。SAS キー名とも呼ばれます。 |
| `sasKey` | のプライマリキー [!DNL Event Hubs] 名前空間。 The `sasPolicy` この `sasKey` 必ず～に対応する `manage` 次に対して設定された権限： [!DNL Event Hubs] リストに値を入力します。 |
| `namespace` | の名前空間 [!DNL Event Hubs] にアクセスしています。 An [!DNL Event Hubs] 名前空間は、1 つ以上のスコーピングコンテナを作成できる一意のスコーピングコンテナを提供します。 [!DNL Event Hubs]. |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Event Hubs] 接続仕様 ID は `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` です。 |

>[!TAB SAS 認証]

| 資格情報 | 説明 |
| --- | --- |
| `sasKeyName` | 認証規則の名前。SAS キー名とも呼ばれます。 |
| `sasKey` | のプライマリキー [!DNL Event Hubs] 名前空間。 The `sasPolicy` この `sasKey` 必ず～に対応する `manage` 次に対して設定された権限： [!DNL Event Hubs] リストに値を入力します。 |
| `namespace` | の名前空間 [!DNL Event Hubs] にアクセスしています。 An [!DNL Event Hubs] 名前空間は、1 つ以上のスコーピングコンテナを作成できる一意のスコーピングコンテナを提供します。 [!DNL Event Hubs]. |
| `eventHubName` | の名前 [!DNL Event Hubs] ソース。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Event Hubs] 接続仕様 ID は `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` です。 |

>[!ENDTABS]

これらの値について詳しくは、 [この Event Hubs ドキュメント](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

>[!TIP]
>
>作成後は、 [!DNL Event Hubs] ベース接続。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

ソース接続を作成する最初の手順は、[!DNL Event Hubs] ソースを認証し、ベース接続 ID を生成することです。ベース接続 ID を使用すると、ソース内を移動してファイルを探索し、データのタイプや形式に関する情報など、取り込みたい特定の項目を識別できます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に [!DNL Event Hubs] 認証資格情報をリクエストパラメーターの一部として指定します。

**API 形式**

```http
POST /connections
```

>[!BEGINTABS]

>[!TAB 標準認証]

標準の認証を使用してアカウントを作成するには、 `/connections` エンドポイントを使用して `sasKeyName`, `sasKey`、および `namespace`.

+++リクエスト

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `connectionSpec.id` | The [!DNL Event Hubs] 接続仕様 ID: `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` |

+++

+++応答

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この接続 ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cdbb15c-fb1e-46ee-8049-0f55b53378fe",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

+++

>[!TAB SAS 認証]

SAS 認証を使用してアカウントを作成するには、 `/connections` エンドポイントを使用して `sasKeyName`, `sasKey`,`namespace`、および `eventHubName`.

+++リクエスト

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
              "namespace": "{NAMESPACE}",
              "eventHubName": "{EVENT_HUB_NAME} 
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
| `params.eventHubName` | の名前 [!DNL Event Hubs] ソース。 |
| `connectionSpec.id` | The [!DNL Event Hubs] 接続仕様 ID: `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` |

+++

+++応答

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この接続 ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cdbb15c-fb1e-46ee-8049-0f55b53378fe",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

+++

>[!ENDTABS]

## ソース接続の作成

>[!TIP]
>
>An [!DNL Event Hubs] 消費者グループは、特定の時間に 1 つのフローに対してのみ使用できます。

ソース接続は、データの取り込み元となる外部ソースへの接続を作成および管理します。ソース接続は、データソース、データ形式、データフローの作成に必要なソース接続 ID などの情報で構成されます。ソース接続インスタンスは、テナントと組織に固有です。

ソース接続を作成するには、[!DNL Flow Service] API の `/sourceConnections` エンドポイントに POST リクエストを実行します。

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
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
          "eventHubName": "{EVENT_HUB_NAME}",
          "dataType": "raw",
          "reset": "latest",
          "consumerGroup": "{CONSUMER_GROUP}"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前はわかりやすいものにしてください。 |
| `description` | 指定するとソース接続に関する詳細情報を含めることができるオプションの値。 |
| `baseConnectionId` | の接続 ID [!DNL Event Hubs] 前の手順で生成されたソース。 |
| `connectionSpec.id` | [!DNL Event Hubs] の固定接続仕様 ID。この ID は `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` です。。 |
| `data.format` | 取り込む [!DNL Event Hubs] データの形式。現在、サポートされているデータ形式は `json` のみです。 |
| `params.eventHubName` | の名前 [!DNL Event Hubs] ソース。 |
| `params.dataType` | このパラメーターは、取り込まれるデータのタイプを定義します。`raw` および `xdm` を含むデータタイプがサポートされています。 |
| `params.reset` | このパラメーターは、データの読み取り方法を定義します。 用途 `latest` 最新のデータから読み込みを開始するには、 `earliest` をクリックして、ストリーム内の最初の使用可能なデータから読み取りを開始します。 このパラメーターはオプションで、デフォルトはです。 `earliest` 指定されていない場合は。 |
| `params.consumerGroup` | 使用する公開または購読のメカニズム [!DNL Event Hubs]. このパラメーターはオプションで、デフォルトはです。 `$Default` 指定されていない場合は。 これを参照してください。 [[!DNL Event Hubs] イベント消費者向けガイド](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-features#event-consumers) を参照してください。 **注意**: [!DNL Event Hubs] 消費者グループは、特定の時間に 1 つのフローに対してのみ使用できます。 |

## 次の手順

このチュートリアルに従って、 [!DNL Event Hubs] を使用したソース接続 [!DNL Flow Service] API. 次のチュートリアルでは、このソース接続 ID を使用して、[ [!DNL Flow Service] API を使用したストリーミングデータフローの作成](../../collect/streaming.md)を行います。
