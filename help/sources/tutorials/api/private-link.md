---
title: API でのソースのプライベートリンクのサポート
description: Adobe Experience Platform ソースのプライベートリンクを作成および使用する方法について説明します
exl-id: 9b7fc1be-5f42-4e29-b552-0b0423a40aa1
source-git-commit: 4d82b0a7f5ae9e0a7607fe7cb75261e4d3489eff
workflow-type: tm+mt
source-wordcount: '1515'
ht-degree: 7%

---

# API でのソースのプライベートリンクのサポート

>[!AVAILABILITY]
>
>この機能は、次のソースでサポートされています。
>
>* [[!DNL Azure Blob Storage]](../../connectors/cloud-storage/blob.md)
>* [[!DNL ADLS Gen2]](../../connectors/cloud-storage/adls-gen2.md)
>* [[!DNL Azure File Storage]](../../connectors/cloud-storage/azure-file-storage.md)
>
>プライベートリンクサポートは、現在、Adobe Healthcare Shield またはAdobe Privacy &amp; Security Shield を購入した組織でのみ利用できます。

プライベートリンク機能を使用して、Adobe Experience Platform ソースの接続先となるプライベートエンドポイントを作成できます。 プライベート IP アドレスを使用してソースを仮想ネットワークに安全に接続し、パブリック IP を不要にし、攻撃対象領域を削減します。 複雑なファイアウォールやネットワークアドレス翻訳設定の必要性を排除し、データトラフィックが承認済みのサービスにのみ到達するようにすることで、ネットワーク設定を簡素化します。

このガイドでは、API を使用してプライベートエンドポイントを作成および使用する方法について説明します。

>[!BEGINSHADEBOX]

## プライベートリンクサポートのライセンス使用権限

ソースでのプライベートリンクサポートに関するライセンス使用権限の指標は次のとおりです。

* お客様は、すべてのサンドボックスと組織にわたって、サポートされているソース（[!DNL Azure Blob Storage]、[!DNL ADLS Gen2]、[!DNL Azure File Storage]）を通じたデータ転送を年間で最大 2 TB まで利用できます。
* 各組織には、すべての実稼動サンドボックスに対して最大 10 個のエンドポイントを設定できます。
* 各組織には、すべての開発用サンドボックスに対して最大 1 つのエンドポイントを設定できます。

>[!ENDSHADEBOX]

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; ソース &#x200B;](../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../landing/api-guide.md)のガイドを参照してください。

## プライベートエンドポイントの作成 {#create-private-endpoint}

プライベートエンドポイントを作成するには、`/privateEndpoints` に対して POST リクエストを実行します。

**API 形式**

```http
POST /privateEndpoints
```

**リクエスト**

次のリクエストは、プライベートエンドポイントを作成します。

+++選択してリクエストの例を表示

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/privateEndpoints/' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "ACME Private Endpoint",
      "subscriptionId": "4281a16a-696f-4993-a7d3-a3da32b846f3",
      "resourceGroupName": "acme-sources-experience-platform",
      "resourceName": "acmeexperienceplatform",
      "connectionSpec": {
          "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
          "version": "1.0"
    }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | プライベートエンドポイントの名前。 |
| `subscriptionId` | [!DNL Azure] サブスクリプションに関連付けられた ID。 詳しくは、[!DNL Azure] のガイド [&#x200B; からサブスクリプションとテナント ID を取得する  [!DNL Azure Portal]](https://learn.microsoft.com/en-us/azure/azure-portal/get-subscription-tenant-id) を参照してください。 |
| `resourceGroupName` | [!DNL Azure] のリソースグループの名前。 リソースグループには、[!DNL Azure] ソリューションに関連するリソースが含まれています。 詳しくは、[!DNL Azure] リソースグループの管理 [&#x200B; に関する &#x200B;](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal) ガイドを参照してください。 |
| `resourceName` | リソースの名前。 [!DNL Azure] えば、リソースとは、仮想マシン、Web アプリ、データベースなどのインスタンスを指します。 詳しくは、[!DNL Azure] リソースマネージャーについて [&#x200B; に関する  [!DNL Azure]  ガイド &#x200B;](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview) 参照してください。 |
| `connectionSpec.id` | 使用しているソースの接続仕様 ID。 |
| `connectionSpec.version` | 使用している接続仕様 ID のバージョン。 |

+++

**応答**

応答が成功すると、次の値が返されます。

+++選択すると応答の例が表示されます

```json
{
  "id": "2c7f6574-299a-4832-aec5-886e875872e2",
  "name": "ACME Private Endpoint",
  "subscriptionId": "4281a16a-696f-4993-a7d3-a3da32b846f3",
  "resourceGroupName": "acme-sources-experience-platform",
  "resourceName": "acmeexperienceplatform",
  "connectionSpec": {
      "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
      "version": "1.0"
  },
  "state": "Pending"
}
```

| プロパティ | 説明 |
| --- | --- |
| `id` | 新しく作成したプライベートエンドポイントの ID。 |
| `name` | プライベートエンドポイントの名前。 |
| `subscriptionId` | [!DNL Azure] サブスクリプションに関連付けられた ID。 詳しくは、[!DNL Azure] のガイド [&#x200B; からサブスクリプションとテナント ID を取得する  [!DNL Azure Portal]](https://learn.microsoft.com/en-us/azure/azure-portal/get-subscription-tenant-id) を参照してください。 |
| `resourceGroupName` | [!DNL Azure] のリソースグループの名前。 リソースグループには、[!DNL Azure] ソリューションに関連するリソースが含まれています。 詳しくは、[!DNL Azure] リソースグループの管理 [&#x200B; に関する &#x200B;](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal) ガイドを参照してください。 |
| `resourceName` | リソースの名前。 [!DNL Azure] えば、リソースとは、仮想マシン、Web アプリ、データベースなどのインスタンスを指します。 詳しくは、[!DNL Azure] リソースマネージャーについて [&#x200B; に関する  [!DNL Azure]  ガイド &#x200B;](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/overview) 参照してください。 |
| `connectionSpec.id` | 使用しているソースの接続仕様 ID。 |
| `connectionSpec.version` | 使用している接続仕様 ID のバージョン。 |
| `state` | プライベートエンドポイントの現在の状態。 有効な状態は次のとおりです。 <ul><li>`Pending`</li><li>`Failed`</li><li>`Approved`</li><li>`Rejected`</li></ul> |

+++

## プライベートエンドポイントのリストの取得 {#retrieve-private-endpoints}

組織内の特定のサンドボックスからプライベートエンドポイントのリストを取得するには、`/privateEndpoints` にGET リクエストを実行します。

**API 形式**

```http
GET /privateEndpoints
```

**リクエスト**

次のリクエストでは、組織内に存在するすべてのプライベートエンドポイントのリストを取得します。

+++選択してリクエストの例を表示

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/privateEndpoints' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

応答が成功すると、組織内のプライベートエンドポイントのリストが返されます。

+++選択すると応答の例が表示されます

```json
{
  "items": [
       {
      "id": "ac9eb695-0d1a-42d4-bc45-0842aeaa1eff",
      "name": "TEST_E2E_29_Jan",
      "subscriptionId": "4281a16a-696f-4993-a7d3-a3da32b846f3",
      "resourceGroupName": "acme-noid-experience-platform",
      "resourceName": "acmeprivatelinking",
      "fqdns": [
         
      ],
      "state": "Approved",
      "connectionSpec": {
        "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
        "version": "1.0"
      }
    },
          {
      "id": "4c9eb695-0d1a-42d4-bc45-0842aeaa1efr",
      "name": "TEST_E2E_29_Jan",
      "subscriptionId": "5a0ff2f3-53d6-47e4-abb5-10a18bd3fff0",
      "resourceGroupName": "acme-sources-experience-platform",
      "resourceName": "acmeexperienceplatform",
      "fqdns": [
         
      ],
      "state": "Pending",
      "connectionSpec": {
        "id": "b3ba5556-48be-44b7-8b85-ff2b69b46dc4",
        "version": "1.0"
      }
    } 
  ]
}
```

+++

## 特定のソースのプライベートエンドポイントのリストを取得 {#retrieve-private-endpoints-by-source}

特定のソースに対応するプライベートエンドポイントのリストを取得するには、`/privateEndpoints` エンドポイントに対してGET リクエストを実行し、ソースの `connectionSpec.id` を指定します。

**API 形式**

```http
GET /privateEndpoints?property=connectionSpec.id=={CONNECTION_SPEC_ID}
```

| クエリーパラメーター | 説明 |
| --- | --- |
| `{CONNECTION_SPEC_ID}` | プライベートエンドポイントを検索するソースの接続仕様 ID。 |

**リクエスト**

次のリクエストは、接続仕様 ID `4c10e202-c428-4796-9208-5f1f5732b1cf` のソースに対応するすべてのプライベートエンドポイントのリストを取得します。

+++選択してリクエストの例を表示

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/privateEndpoints?property=connectionSpec.id==4c10e202-c428-4796-9208-5f1f5732b1cf' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

応答が成功すると、接続仕様 ID `4c10e202-c428-4796-9208-5f1f5732b1cf` を持つソースに対応するすべてのプライベートエンドポイントのリストが返されます。

+++選択すると応答の例が表示されます

```json
{
  "items": [
       {
      "id": "ac9eb695-0d1a-42d4-bc45-0842aeaa1eff",
      "name": "TEST_E2E_29_Jan",
      "subscriptionId": "4281a16a-696f-4993-a7d3-a3da32b846f3",
      "resourceGroupName": "acme-noid-experience-platform",
      "resourceName": "acmeprivatelinkhg",
      "fqdns": [
         
      ],
      "state": "Approved",
      "connectionSpec": {
        "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
        "version": "1.0"
      }
    },
    {
      "id": "4c9eb695-0d1a-42d4-bc45-0842aeaa1efr",
      "name": "TEST_E2E_29_Jan",
      "subscriptionId": "5a0ff2f3-53d6-47e4-abb5-10a18bd3fff0",
      "resourceGroupName": "acme-sources-experience-platform",
      "resourceName": "acmeexperienceplatform",
      "fqdns": [
         
      ],
      "state": "Pending",
      "connectionSpec": {
        "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
        "version": "1.0"
      }
    } 
  ]
}
```

+++

## プライベートエンドポイントの取得 {#retrieve-specific-private-endpoint}

特定のプライベートエンドポイントを取得するには、`/privateEndpoints` にGET リクエストを実行し、取得するプライベートエンドポイントの ID を指定します。

**API 形式**

```http
GET /privateEndpoints/{PRIVATE_ENDPOINT_ID}
```

| クエリーパラメーター | 説明 |
| --- | --- |
| `{PRIVATE_ENDPOINT_ID}` | 取得するプライベートエンドポイントの ID。 |

**リクエスト**

次のリクエストは、ID `2c5699b0-b9b6-486f-8877-ee5e21fe9a9d` のプライベートエンドポイントを取得します。

+++選択してリクエストの例を表示

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/privateEndpoints/2c5699b0-b9b6-486f-8877-ee5e21fe9a9d' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

応答が成功すると、次の ID を持つプライベートエンドポイントが返されます。`2c5699b0-b9b6-486f-8877-ee5e21fe9a9d`

+++選択すると応答の例が表示されます

```json
{
  "items": [
       {
      "id": "2c5699b0-b9b6-486f-8877-ee5e21fe9a9d",
      "name": "TEST_E2E_29_Jan",
      "subscriptionId": "5a0ff2f3-53d6-47e4-abb5-10a18bd3fff0",
      "resourceGroupName": "acme-noid-experience-platform",
      "resourceName": "acmeprivatelinkhg",
      "fqdns": [
         
      ],
      "state": "Approved",
      "connectionSpec": {
        "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
        "version": "1.0"
      }
    }
  ]
}
```

+++

## プライベートエンドポイントの解決 {#resolve-private-endpoint}

**API 形式**

```http
GET /privateEndpoints?op=autoResolve
```

**リクエスト**

+++選択してリクエストの例を表示

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/privateEndpoints?op=autoResolve' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "auth": {
          "specName": "ConnectionString",
          "params": {
              "usePrivateLink": true,
              "connectionString": "{CONNECTION_STRING}"
          }
      },
      "connectionSpec": {
          "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
          "version": "1.0"
      }
  }'
```

+++

**応答**

+++選択すると応答の例が表示されます

```json
{
  "items": [
        {
      "id": "4c9eb695-0d1a-42d4-bc45-0842aeaa1efr",
      "name": "TEST_E2E_29_Jan",
      "subscriptionId": "5a0ff2f3-53d6-47e4-abb5-10a18bd3fff0",
      "resourceGroupName": "acme-sources-experience-platform",
      "resourceName": "acmeexperienceplatform",
      "fqdns": [
         
      ],
      "state": "Pending",
      "connectionSpec": {
        "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
        "version": "1.0"
      } 
    }
  ]
}
```

+++

## Enable [!DNL Interactive Authoring] {#enable-interactive-authoring}

>[!IMPORTANT]
>
>フローを作成または更新する前、および接続を作成、更新または探索する前に、[!DNL Interactive Authoring] を有効にする必要があります。

[!DNL Interactive Authoring] は、接続やアカウントの調査やデータのプレビューなどの機能に使用されます。 [!DNL Interactive Authoring] を有効にするには、`/privateEndpoints/interactiveAuthoring` に対して POST リクエストを実行し、クエリパラメーターの演算子として `enable` を指定します。

**API 形式**

```http
POST /privateEndpoints/interactiveAuthoring?op=enable
```

| クエリーパラメーター | 説明 |
| --- | --- |
| `op` | 実行する操作。 [!DNL Interactive Authoring] を有効にするには、`op` の値を `enable` に設定します。 |

**リクエスト**

次のリクエストは、プライベートエンドポイントの [!DNL Interactive Authoring] を有効にし、TTL を 60 分に設定します。

+++選択してリクエストの例を表示

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/privateEndpoints/interactiveAuthoring?op=enable' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "autoTerminationMinutes": 60
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `autoTerminationMinutes` | [!DNL Interactive Authoring] の TTL （有効期間）は分単位です。 [!DNL Interactive Authoring] は、接続やアカウントの調査やデータのプレビューなどの機能に使用されます。 最大 TTL は 120 分に設定できます。 デフォルト TTL は 60 分です。 |

+++

**応答**

リクエストが成功した場合は、HTTP ステータス 202 （許可済み）が返されます。

## ステータス [!DNL Interactive Authoring] 取得 {#retrieve-interactive-authoring-status}

プライベートエンドポイントに関する [!DNL Interactive Authoring] の現在のステータスを表示するには、`/privateEndpoints/interactiveAuthoring` に対してGET リクエストを実行します。

**API 形式**

```http
GET /privateEndpoints/interactiveAuthoring
```

**リクエスト**

次のリクエストは、[!DNL Interactive Authoring] のステータスを取得します。

+++選択してリクエストの例を表示

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/connectors/privateEndpoints/interactiveAuthoring' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

+++選択すると応答の例が表示されます

```json
{
    "status": "Disabled"
}
```

| プロパティ | 説明 |
| --- | --- |
| `status` | [!DNL Interactive Authoring] のステータス。 有効な値は `disabled`、`enabling`、`enabled` です。 |

+++

## プライベートエンドポイントを削除 {#delete-private-endpoint}

プライベートエンドポイントを削除するには、`/privateEndpoints` にDELETE リクエストを実行し、削除するエンドポイントの ID を指定します。

**API 形式**

```http
DELETE /privateEndpoints/{PRIVATE_ENDPOINT_ID}
```

| クエリーパラメーター | 説明 |
| --- | --- |
| `{PRIVATE_ENDPOINT_ID}` | 削除するプライベートエンドポイントの ID。 |

**リクエスト**

次のリクエストは、ID `02a74b31-a566-4a86-9cea-309b101a7f24` のプライベートエンドポイントを削除します。

+++選択してリクエストの例を表示

```shell
curl -X DELETE \
  'https://platform.adobe.io/data/foundation/connectors/privateEndpoints/02a74b31-a566-4a86-9cea-309b101a7f24' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

リクエストが成功した場合は、HTTP ステータス 200 （成功）が返されます。 削除を確認するには、GET リクエストを実行し、削除された ID をクエリパラメーターとして `/privateEndpoints` に指定します。

## フローサービス {#flow-service}

[[!DNL Flow Service] API](https://developer.adobe.com/experience-platform-apis/references/flow-service/) と組み合わせてプライベートエンドポイントを使用する方法については、次の節を参照してください。

### プライベートエンドポイントとの接続の作成 {#create-base-connection}

Experience Platformのプライベートエンドポイントとの接続を作成するには、`/connections` API の [!DNL Flow Service] エンドポイントに対して POST リクエストを実行します。

**API 形式**

```http
POST /connections/
```

**リクエスト**

次のリクエストは、プライベートエンドポイントも使用して、[!DNL Azure Blob Storage] 用の認証済みベース接続を作成します。

+++選択してリクエストの例を表示

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections/' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Azure Blob Storage base connection",
      "description": "A base connection for a Azure Blob Storage source that uses a private link.",
      "auth": {
          "specName": "ConnectionString",
          "params": {
              "connectionString": "{CONNECTION_STRING}",
              "usePrivateLink" : true
          }
      },
      "connectionSpec": {
          "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ベース接続の名前。 |
| `description` | （オプション）接続に関する追加情報を提供する説明。 |
| `auth.specName` | ソースをExperience Platformに接続するために使用される認証。 |
| `auth.params.connectionString` | [!DNL Azure Blob Storage] 接続文字列。 詳しくは、[[!DNL Azure Blob Storage] API 認証ガイド &#x200B;](../api/create/cloud-storage/blob.md) を参照してください。 |
| `auth.params.usePrivateLink` | プライベートエンドポイントを使用しているかどうかを判断するブール値。 プライベートエンドポイントを使用している場合、この値を `true` に設定します。 |
| `connectionSpec.id` | [!DNL Azure Blob Storage] の接続仕様 ID |
| `connectionSpec.version` | [!DNL Azure Blob Storage] 接続仕様 ID のバージョン。 |

+++

**応答**

リクエストが成功した場合は、新しく生成されたベース接続 ID と etag が返されます。

+++選択すると応答の例が表示されます

```json
{
  "id": "a59d368a-1152-4673-a46e-bd52e8cdb9a9",
  "etag": "\"f50185ed-0000-0200-0000-637e8fad0000\""
}
```

+++

### 特定のプライベートエンドポイントに関連付けられている接続を取得します {#retrieve-connections-by-endpoint}

特定のプライベートエンドポイントに関連付けられている接続を取得するには、`/connections` エンドポイントに対してGET リクエストを実行し、プライベートエンドポイントの ID をクエリパラメーターとして指定します。

**API 形式**

```http
GET /connections?property=auth.params.privateEndpointId=={PRIVATE_ENDPOINT_ID}
```

| クエリーパラメーター | 説明 |
| --- | --- |
| {PRIVATE_ENDPOINT_ID} | 取得する接続に結び付けられたプライベートエンドポイントの ID。 |

**リクエスト**

次のリクエストは、ID `02a74b31-a566-4a86-9cea-309b101a7f24` のプライベートエンドポイントに関連付けられている既存の接続を取得します。

+++選択してリクエストの例を表示

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections?property=auth.params.privateEndpointId==02a74b31-a566-4a86-9cea-309b101a7f24' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

応答が成功すると、クエリされたプライベートエンドポイントに関連付けられた接続のリストが返されます。

+++選択すると応答の例が表示されます

```json
{
  "items": [
    {
      "id": "42a27b1f-8e3f-48ce-8c29-7e474b29a015",
      "createdAt": 1729154379292,
      "updatedAt": 1729154382031,
      "createdBy": "{CREATED_BY}",
      "updatedBy": "{UPDATED_BY}",
      "createdClient": "{CREATED_CLIENT}",
      "updatedClient": "{UPDATED_CLIENT}",
      "sandboxId": "{SANDBOX_ID}",
      "sandboxName": "{SANDBOX_NAME}",
      "imsOrgId": "{ORG_NAME}",
      "name": "acme-e2e",
      "connectionSpec": {
        "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
        "version": "1.0"
      },
      "state": "enabled",
      "auth": {
        "specName": "ConnectionString",
        "params": {
          "connectionString": "{CONNECTION_STRING}",
          "usePrivateLink": true,
          "privateEndpointId": "02a74b31-a566-4a86-9cea-309b101a7f24"
        }
      },
      "version": "\"2f01454b-0000-0200-0000-6766749a0000\"",
      "etag": "\"2f01454b-0000-0200-0000-6766749a0000\"",
      "lastOperation": {
        "started": 0,
        "updated": 0,
        "operation": "create"
      }
    },
    {
      "id": "6350311a-664c-4b08-aad4-4065781aac81",
      "createdAt": 1718199941102,
      "updatedAt": 1718199945147,
      "createdBy": "{CREATED_BY}",
      "updatedBy": "{UPDATED_BY}",
      "createdClient": "{CREATED_CLIENT}",
      "updatedClient": "{UPDATED_CLIENT}",
      "sandboxId": "{SANDBOX_ID}",
      "sandboxName": "{SANDBOX_NAME}",
      "imsOrgId": "{ORG_NAME}",
      "name": "acme demo",
      "connectionSpec": {
        "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
        "version": "1.0"
      },
      "state": "enabled",
      "auth": {
        "specName": "ConnectionString",
        "params": {
          "connectionString": "{CONNECTION_STRING}",
          "usePrivateLink": true,
          "privateEndpointId": "02a74b31-a566-4a86-9cea-309b101a7f24"
        }
      },
      "version": "\"3001307e-0000-0200-0000-6766cf710000\"",
      "etag": "\"3001307e-0000-0200-0000-6766cf710000\"",
      "lastOperation": {
        "started": 0,
        "updated": 0,
        "operation": "create"
      }
    }
  ],
  "_links": {
     
  }
}
```

+++

### 任意のプライベートエンドポイントに関連付けられている接続の取得 {#retrieve-connections}

任意のプライベートエンドポイントに関連付けられている接続を取得するには、`/connections` エンドポイントに対してGET リクエストを実行し、クエリパラメーターとして `property=auth.params.usePrivateLink==true` を指定します。

**API 形式**

```http
GET /connections?property=auth.params.usePrivateLink==true
```

**リクエスト**

次のリクエストでは、プライベートエンドポイントを使用する、組織内のすべての接続を取得します。

+++選択してリクエストの例を表示

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/flowservice/connections?property=auth.params.usePrivateLink==true' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
```

+++

**応答**

応答が成功すると、プライベートエンドポイントに関連付けられているすべての接続が返されます。

+++選択すると応答の例が表示されます

```json
{
  "items": [
    {
      "id": "42a27b1f-8e3f-48ce-8c29-7e474b29a015",
      "createdAt": 1729154379292,
      "updatedAt": 1729154382031,
      "createdBy": "{CREATED_BY}",
      "updatedBy": "{UPDATED_BY}",
      "createdClient": "{CREATED_CLIENT}",
      "updatedClient": "{UPDATED_CLIENT}",
      "sandboxId": "{SANDBOX_ID}",
      "sandboxName": "{SANDBOX_NAME}",
      "imsOrgId": "{ORG_NAME}",
      "name": "acme-e2e",
      "connectionSpec": {
        "id": "4c10e202-c428-4796-9208-5f1f5732b1cf",
        "version": "1.0"
      },
      "state": "enabled",
      "auth": {
        "specName": "ConnectionString",
        "params": {
          "connectionString": "{CONNECTION_STRING}",
          "usePrivateLink": true,
          "privateEndpointId": "02a74b31-a566-4a86-9cea-309b101a7f24"
        }
      },
      "version": "\"2f01454b-0000-0200-0000-6766749a0000\"",
      "etag": "\"2f01454b-0000-0200-0000-6766749a0000\"",
      "lastOperation": {
        "started": 0,
        "updated": 0,
        "operation": "create"
      }
    },
    {
      "id": "6350311a-664c-4b08-aad4-4065781aac81",
      "createdAt": 1718199941102,
      "updatedAt": 1718199945147,
      "createdBy": "{CREATED_BY}",
      "updatedBy": "{UPDATED_BY}",
      "createdClient": "{CREATED_CLIENT}",
      "updatedClient": "{UPDATED_CLIENT}",
      "sandboxId": "{SANDBOX_ID}",
      "sandboxName": "{SANDBOX_NAME}",
      "imsOrgId": "{ORG_NAME}",
      "name": "acme demo",
      "connectionSpec": {
        "id": "b2e08744-4f1a-40ce-af30-7abac3e23cf3",
        "version": "1.0"
      },
      "state": "enabled",
      "auth": {
        "specName": "ConnectionString",
        "params": {
          "connectionString": "{CONNECTION_STRING}",
          "usePrivateLink": true
        }
      },
      "version": "\"3001307e-0000-0200-0000-6766cf710000\"",
      "etag": "\"3001307e-0000-0200-0000-6766cf710000\"",
      "lastOperation": {
        "started": 0,
        "updated": 0,
        "operation": "create"
      }
    }
  ],
  "_links": {
     
  }
}
```

+++

## 付録

API のプライベートリンクを使用する方法について [!DNL Azure]、この節を参照してください。

### [!DNL Azure Blob] および [!DNL Azure Data Lake Gen2] のプライベートエンドポイントを承認

[!DNL Azure Blob] および [!DNL Azure Data Lake Gen2] ソースに対するプライベートエンドポイントリクエストを承認するには、[!DNL Azure Portal] にログインします。 左側のナビゲーションで「**[!DNL Data storage]**」を選択し、「**[!DNL Security + networking]**」タブに移動して「**[!DNL Networking]**」を選択します。 次に、「**[!DNL Private endpoints]**」を選択して、お使いのアカウントに関連付けられているプライベートエンドポイントとその現在の接続状態のリストを表示します。 保留中のリクエストを承認するには、目的のエンドポイントを選択し、「**[!DNL Approve]**」をクリックします。

![&#x200B; 保留中のプライベートエンドポイントのリストを含む Azure Portal。](../../images/tutorials/private-links/azure.png)

<!--

### Configure your [!DNL Snowflake] account to connect to private links

You must complete the following prerequisite steps in order to use the [!DNL Snowflake] source with private links.

First, you must raise a support ticket in [!DNL Snowflake] and request for the **endpoint service resource ID** of the [!DNL Azure] region of your [!DNL Snowflake] account. Follow the steps below to raise a [!DNL Snowflake] ticket:

1. Navigate to the [[!DNL Snowflake] UI](https://app.snowflake.com) and sign in with your email account. During this step, you must ensure that your email is verified in profile settings.
2. Select your **user menu** and then select **support** to access [!DNL Snowflake] support.
3. To create a support case, select **[!DNL + Support Case]**. Then, fill out the form with relevant details and attach any necessary files.
4. When finished, submit the case.

The endpoint resource ID is formatted as follows:

```shell
subscriptions/{SUBSCRIPTION_ID}/resourceGroups/az{REGION}-privatelink/providers/microsoft.network/privatelinkservices/sf-pvlinksvc-az{REGION}
```

+++Select to view example

```shell
/subscriptions/4575fb04-6859-4781-8948-7f3a92dc06a3/resourceGroups/azwestus2-privatelink/providers/microsoft.network/privatelinkservices/sf-pvlinksvc-azwestus2
```

+++

| Parameter | Description | Example |
| --- | --- | --- |
| `{SUBSCRIPTION_ID}` | The unique ID that identifies your [!DNL Azure] subscription. | `a1b2c3d4-5678-90ab-cdef-1234567890ab` |
| `{REGION}` | The [!DNL Azure] region of your [!DNL Snowflake] account. | `azwestus2` |

### Retrieve your private link configuration details

To retrieve your private link configuration details, you must run the following command in [!DNL Snowflake]:

```sql
USE ROLE accountadmin;
SELECT key, value::varchar
FROM TABLE(FLATTEN(input => PARSE_JSON(SYSTEM$GET_PRIVATELINK_CONFIG())));
```

Next, retrieve values for the following properties:

* `privatelink-account-url`
* `regionless-privatelink-account-url`
* `privatelink_ocsp-url`

Once you have retrieved the values, you can make the following call to create a private link for [!DNL Snowflake].

**Request**

The following request creates a private endpoint for [!DNL Snowflake]:

>[!BEGINTABS]

>[!TAB Template]

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/privateEndpoints/' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "{ENDPOINT_NAME}",
    "subscriptionId": "{AZURE_SUBSCRIPTION_ID}",
    "resourceGroupName": "{RESOURCE_GROUP_NAME}",
    "resourceName": "{SNOWFLAKE_ENDPOINT_SERVICE_NAME}",
    "fqdns": [
      "{PRIVATELINK_ACCOUNT_URL}",
      "{REGIONLESS_PRIVATELINK_ACCOUNT_URL}",
      "{PRIVATELINK_OCSP_URL}"
    ],
    "connectionSpec": {
      "id": "b2e08744-4f1a-40ce-af30-7abac3e23cf3",
      "version": "1.0"
    }
  }'
```

>[!TAB Example]

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/connectors/privateEndpoints/' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
    "name": "TEST_Snowflake_PE",
    "subscriptionId": "4575fb04-6859-4781-8948-7f3a92dc06a3",
    "resourceGroupName": "azwestus2-privatelink",
    "resourceName": "sf-pvlinksvc-azwestus2",
    "fqdns": [
      "hf06619.west-us-2.privatelink.snowflakecomputing.com",
      "adobe-segmentationdbint.privatelink.snowflakecomputing.com",
      "ocsp.hf06619.west-us-2.privatelink.snowflakecomputing.com"
    ],
    "connectionSpec": {
      "id": "b2e08744-4f1a-40ce-af30-7abac3e23cf3",
      "version": "1.0"
    }
  }'
```

>[!ENDTABS]

-->