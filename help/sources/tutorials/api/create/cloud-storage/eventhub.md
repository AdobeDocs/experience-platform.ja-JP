---
title: Flow Service API を使用した Azure Event Hubs Source接続の作成
description: Flow Service API を使用してAdobe Experience Platformを Azure Event Hubs アカウントに接続する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: a4d0662d-06e3-44f3-8cb7-4a829c44f4d9
source-git-commit: bad1e0a9d86dcce68f1a591060989560435070c5
workflow-type: tm+mt
source-wordcount: '1524'
ht-degree: 29%

---

# [!DNL Flow Service] API を使用した [!DNL Azure Event Hubs] ソース接続の作成

>[!IMPORTANT]
>
>Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL Azure Event Hubs] ソースを利用できます。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Azure Event Hubs] （以下「[!DNL Event Hubs]」）をExperience Platformに接続する方法について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

- [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
- [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してExperience Platformに正しく接続するために必要 [!DNL Event Hubs] 追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Event Hubs] アカウントに接続するには、次の接続プロパティの値を指定する必要があります。

>[!BEGINTABS]

>[!TAB  標準認証 ]

| 資格情報 | 説明 |
| --- | --- |
| `sasKeyName` | 承認ルールの名前。SAS キー名とも呼ばれます。 |
| `sasKey` | [!DNL Event Hubs] 名前空間のプライマリキー。 [!DNL Event Hubs] リストを入力するには、`sasKey` が対応する `sasPolicy` に `manage` 権限が設定されている必要があります。 |
| `namespace` | アクセスする [!DNL Event Hub] の名前空間。 [!DNL Event Hub] 名前空間は、一意のスコーピングコンテナを提供し、このコンテナでは 1 つ以上の [!DNL Event Hubs] を作成できます。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Event Hubs] 接続仕様 ID は `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` です。 |

>[!TAB SAS 認証 ]

| 資格情報 | 説明 |
| --- | --- |
| `sasKeyName` | 承認ルールの名前。SAS キー名とも呼ばれます。 |
| `sasKey` | [!DNL Event Hubs] 名前空間のプライマリキー。 [!DNL Event Hubs] リストを入力するには、`sasKey` が対応する `sasPolicy` に `manage` 権限が設定されている必要があります。 |
| `namespace` | アクセスする [!DNL Event Hub] の名前空間。 [!DNL Event Hub] 名前空間は、一意のスコーピングコンテナを提供し、このコンテナでは 1 つ以上の [!DNL Event Hubs] を作成できます。 |
| `eventHubName` | [!DNL Azure Event Hub] 名を入力します。 [!DNL Event Hub] の名前について詳しくは、[Microsoft ドキュメント &#x200B;](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hub) を参照してください。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Event Hubs] 接続仕様 ID は `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` です。 |

[!DNL Event Hubs] の共有アクセス署名（SAS）認証について詳しくは、[[!DNL Azure] SAS の使用に関するガイド &#x200B;](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature) を参照してください。

>[!TAB Event Hub Azure Active Directory Auth]

| 資格情報 | 説明 |
| --- | --- |
| `tenantId` | 権限をリクエストするテナント ID。 テナント ID は、GUID またはわかりやすい名前として書式設定できます。 **注意**：テナント ID は、[!DNL Microsoft Azure] インターフェイスでは「ディレクトリ ID」と呼ばれます。 |
| `clientId` | アプリに割り当てられたアプリケーション ID。 この ID は、[!DNL Azure Active Directory] ーザーを登録した [!DNL Microsoft Entra ID] ポータルから取得できます。 |
| `clientSecretValue` | アプリを認証するためにクライアント ID と共に使用されるクライアント秘密鍵。 クライアントの秘密鍵は、クライアントを登録した [!DNL Microsoft Entra ID] ポータルから取得でき [!DNL Azure Active Directory] す。 |
| `namespace` | アクセスする [!DNL Event Hub] の名前空間。 [!DNL Event Hub] 名前空間は、一意のスコーピングコンテナを提供し、このコンテナでは 1 つ以上の [!DNL Event Hubs] を作成できます。 |

[!DNL Azure Active Directory] について詳しくは、[Microsoft Entra ID の使用に関する Azure ガイド &#x200B;](https://learn.microsoft.com/en-us/azure/healthcare-apis/register-application) を参照してください。

>[!TAB Azure Active Directory 認証をスコープとするイベント ハブ ]

| 資格情報 | 説明 |
| --- | --- |
| `tenantId` | 権限をリクエストするテナント ID。 テナント ID は、GUID またはわかりやすい名前として書式設定できます。 **注意**：テナント ID は、[!DNL Microsoft Azure] インターフェイスでは「ディレクトリ ID」と呼ばれます。 |
| `clientId` | アプリに割り当てられたアプリケーション ID。 この ID は、[!DNL Azure Active Directory] ーザーを登録した [!DNL Microsoft Entra ID] ポータルから取得できます。 |
| `clientSecretValue` | アプリを認証するためにクライアント ID と共に使用されるクライアント秘密鍵。 クライアントの秘密鍵は、クライアントを登録した [!DNL Microsoft Entra ID] ポータルから取得でき [!DNL Azure Active Directory] す。 |
| `namespace` | アクセスする [!DNL Event Hub] の名前空間。 [!DNL Event Hub] 名前空間は、一意のスコーピングコンテナを提供し、このコンテナでは 1 つ以上の [!DNL Event Hubs] を作成できます。 |
| `eventHubName` | [!DNL Azure Event Hub] 名を入力します。 [!DNL Event Hub] の名前について詳しくは、[Microsoft ドキュメント &#x200B;](https://learn.microsoft.com/en-us/azure/event-hubs/event-hubs-create#create-an-event-hub) を参照してください。 |

>[!ENDTABS]

これらの値について詳しくは、[&#x200B; この Event Hubs ドキュメント &#x200B;](https://docs.microsoft.com/en-us/azure/event-hubs/authenticate-shared-access-signature) を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

>[!TIP]
>
>作成した後は、[!DNL Event Hubs] ベース接続の認証タイプを変更できません。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

ソース接続を作成する最初の手順は、[!DNL Event Hubs] ソースを認証し、ベース接続 ID を生成することです。ベース接続 ID を使用すると、ソース内を移動してファイルを探索し、データのタイプや形式に関する情報など、取り込みたい特定の項目を識別できます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に [!DNL Event Hubs] 認証資格情報をリクエストパラメーターの一部として指定します。

**API 形式**

```http
POST /connections
```

>[!BEGINTABS]

>[!TAB  標準認証 ]

標準認証を使用してアカウントを作成するには、`/connections` エンドポイントに POST リクエストを実行し、その際、`sasKeyName`、`sasKey`、`namespace` の値を指定します。

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
| `auth.params.sasKeyName` | 承認ルールの名前。SAS キー名とも呼ばれます。 |
| `auth.params.sasKey` | 生成された共有アクセス署名。 |
| `auth.params.namespace` | アクセスする [!DNL Event Hubs] の名前空間。 |
| `connectionSpec.id` | [!DNL Event Hubs] 接続仕様 ID は `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` です。 |

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

>[!TAB SAS 認証 ]

SAS 認証を使用してアカウントを作成するには、`sasKeyName`、`sasKey`、`namespace`、`eventHubName` の値を指定すると同時に、`/connections` エンドポイントに対して POST リクエストを行います。

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
| `auth.params.sasKeyName` | 承認ルールの名前。SAS キー名とも呼ばれます。 |
| `auth.params.sasKey` | 生成された共有アクセス署名。 |
| `auth.params.namespace` | アクセスする [!DNL Event Hubs] の名前空間。 |
| `params.eventHubName` | [!DNL Event Hubs] ソースの名前。 |
| `connectionSpec.id` | [!DNL Event Hubs] 接続仕様 ID は `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` です。 |

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

>[!TAB Event Hub Azure Active Directory Auth]

Azure Active Directory Auth を使用してアカウントを作成するには、`tenantId`、`clientId`、`clientSecretValue`、`namespace` の値を指定したうえで、`/connections` エンドポイントに対して POST リクエストを行います。

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
          "specName": "Event Hub Azure Active Directory Auth",
          "params": {
              "tenantId": "{TENANT_ID}",
              "clientId": "{CLIENT_ID}",
              "clientSecretValue": "{CLIENT_SECRET_VALUE}",
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
| `auth.params.tenantId` | アプリケーションのテナント ID。 **注意**：テナント ID は、[!DNL Microsoft Azure] インターフェイスでは「ディレクトリ ID」と呼ばれます。 |
| `auth.params.clientId` | 組織のクライアント ID。 |
| `auth.params.clientSecretValue` | 組織のクライアントシークレット値。 |
| `auth.params.namespace` | アクセスする [!DNL Event Hubs] の名前空間。 |
| `connectionSpec.id` | [!DNL Event Hubs] 接続仕様 ID は `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` です。 |

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

>[!TAB Azure Active Directory 認証をスコープとするイベント ハブ ]

Azure Active Directory Auth を使用してアカウントを作成するには、`tenantId`、`clientId`、`clientSecretValue`、`namespace`、`eventHubName` の値を指定したうえで、`/connections` エンドポイントに対して POST リクエストを行います。

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
          "specName": "Event Hub Scoped Azure Active Directory Auth",
          "params": {
              "tenantId": "{TENANT_ID}",
              "clientId": "{CLIENT_ID}",
              "clientSecretValue": "{CLIENT_SECRET_VALUE}",
              "namespace": "{NAMESPACE}",
              "eventHubName": "{EVENT_HUB_NAME}" 
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
| `auth.params.tenantId` | アプリケーションのテナント ID。 **注意**：テナント ID は、[!DNL Microsoft Azure] インターフェイスでは「ディレクトリ ID」と呼ばれます。 |
| `auth.params.clientId` | 組織のクライアント ID。 |
| `auth.params.clientSecretValue` | 組織のクライアントシークレット値。 |
| `auth.params.namespace` | アクセスする [!DNL Event Hubs] の名前空間。 |
| `auth.params.eventHubName` | [!DNL Event Hubs] ソースの名前。 |
| `connectionSpec.id` | [!DNL Event Hubs] 接続仕様 ID は `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` です。 |

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
>[!DNL Event Hubs] コンシューマーグループは、特定の時間に 1 つのフローに対してのみ使用できます。

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
| `baseConnectionId` | 前の手順で生成された [!DNL Event Hubs] ソースの接続 ID。 |
| `connectionSpec.id` | [!DNL Event Hubs] の固定接続仕様 ID。この ID は `bf9f5905-92b7-48bf-bf20-455bc6b60a4e` です。 |
| `data.format` | 取り込む [!DNL Event Hubs] データの形式。現在、サポートされているデータ形式は `json` のみです。 |
| `params.eventHubName` | [!DNL Event Hubs] ソースの名前。 |
| `params.dataType` | このパラメーターは、取り込まれるデータのタイプを定義します。`raw` および `xdm` を含むデータタイプがサポートされています。 |
| `params.reset` | このパラメーターは、データの読み取り方法を定義します。 `latest` を使用すると、最新のデータから読み取りを開始でき、`earliest` を使用すると、ストリーム内の最初の使用可能なデータから読み取りを開始できます。 このパラメーターはオプションです。指定しない場合、デフォルトは `earliest` になります。 |
| `params.consumerGroup` | [!DNL Event Hubs] に使用するパブリッシュまたは購読のメカニズム。 このパラメーターはオプションです。指定しない場合、デフォルトは `$Default` になります。 詳しくは、この [[!DNL Event Hubs]  イベントコンシューマーに関するガイド &#x200B;](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-features#event-consumers) を参照してください。 **メモ**:[!DNL Event Hubs] コンシューマーグループは、特定の時間に 1 つのフローに対してのみ使用できます。 |

>[!NOTE]
>
>ストリーミングデータフローを作成または更新した後、データ損失やデータ削除が発生する可能性を防ぐために、データ取り込みを 5 分間ほど一時停止する必要があります。

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Event Hubs] ソース接続を作成しました。 次のチュートリアルでは、このソース接続 ID を使用して、[&#x200B; [!DNL Flow Service] API を使用したストリーミングデータフローの作成](../../collect/streaming.md)を行います。
