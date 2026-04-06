---
title: Flow Service API を使用した Google PubSub ソース接続の作成
description: Flow Service API を使用して Adobe Experience Platform を Google PubSub アカウントに接続する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: f5b8f9bf-8a6f-4222-8eb2-928503edb24f
source-git-commit: 82e41af32468febeda2dce6b471d72ef74359ea9
workflow-type: tm+mt
source-wordcount: '1181'
ht-degree: 45%

---

# Flow Service API を使用した [!DNL Google PubSub] ソース接続の作成

>[!IMPORTANT]
>
>[!DNL Google PubSub] ソースは、Real-Time Customer Data Platform Ultimateを購入したユーザーがソースカタログで利用できます。

このチュートリアルでは、 [[!DNL Flow Service] API](<https://www.adobe.io/experience-platform-apis/references/flow-service/>) を使用して [!DNL Google PubSub]（以下「[!DNL PubSub]」）を Experience Platform に接続する手順を詳しく説明します。

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; ソース &#x200B;](../../../../home.md): Experience Platformを使用すると、様々なソースからデータを取り込むことができますが、Experience Platform サービスを使用して着信データを構造化、ラベル付け、強化することができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md): Experience Platformは、1つのExperience Platform インスタンスを個別のバーチャル環境に分割して、デジタルエクスペリエンスアプリケーションの開発と進化に役立つバーチャルサンドボックスを提供します。

以下の節では、[!DNL PubSub] APIを使用して[!DNL Flow Service]をExperience Platformに正常に接続するために必要な追加情報を示します。

### 必要な資格情報の収集

[!DNL PubSub] アカウントを[!DNL Flow Service]に接続するには、以下に概説する接続プロパティの値を指定する必要があります。 認証と前提条件の設定について詳しくは、[[!DNL PubSub source] 概要](../../../../connectors/cloud-storage/google-pubsub.md#prerequisites)を参照してください。

>[!BEGINTABS]

>[!TAB  プロジェクトベースの認証]

| 資格情報 | 説明 |
| --- | --- |
| `projectId` | [!DNL PubSub] の認証に必要なプロジェクト ID。 |
| `credentials` | [!DNL PubSub]の認証に必要な資格情報。 資格情報から空白を削除した後、完全なJSON ファイルを配置する必要があります。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソースターゲット接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL PubSub] 接続仕様 ID は `70116022-a743-464a-bbfe-e226a7f8210c` です。 |

>[!TAB  トピックおよびサブスクリプションベースの認証]

| 資格情報 | 説明 |
| --- | --- |
| `credentials` | [!DNL PubSub]の認証に必要な資格情報。 資格情報から空白を削除した後、完全なJSON ファイルを配置する必要があります。 |
| `topicName` | メッセージのフィードを表すリソースの名前。 [!DNL PubSub] ソース内の特定のデータ ストリームへのアクセスを提供する場合は、トピック名を指定する必要があります。 トピック名の形式：`projects/{PROJECT_ID}/topics/{TOPIC_ID}`。 |
| `subscriptionName` | [!DNL PubSub] サブスクリプションの名前。 [!DNL PubSub]では、メッセージが公開されたトピックにサブスクライブすることで、メッセージを受信できます。 **注**:1つの[!DNL PubSub] サブスクリプションは、1つのデータフローにのみ使用できます。 複数のデータフローを作成するには、複数のサブスクリプションが必要です。 サブスクリプション名の形式：`projects/{PROJECT_ID}/subscriptions/{SUBSCRIPTION_ID}`。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソースターゲット接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL PubSub] 接続仕様 ID は `70116022-a743-464a-bbfe-e226a7f8210c` です。 |

>[!ENDTABS]

これらの値について詳しくは、この[[!DNL PubSub] 認証](https://cloud.google.com/pubsub/docs/authentication)文書を参照してください。 サービスアカウントベースの認証を使用するには、資格情報を生成する手順については、この[[!DNL PubSub]  サービスアカウントの作成に関するガイド &#x200B;](https://cloud.google.com/docs/authentication/production#create_service_account)を参照してください。

>[!TIP]
>
>サービスアカウントベースの認証を使用している場合は、サービスアカウントに十分なユーザーアクセス権が付与され、資格情報をコピー＆ペーストする際に、JSON 内に余分な空白がないことを確認してください。

### Experience Platform APIの使用

Experience Platform APIの呼び出しを正常に行う方法について詳しくは、[Experience Platform APIの概要](../../../../../landing/api-guide.md)に関するガイドを参照してください。

## ベース接続の作成

>[!TIP]
>
>作成したら、[!DNL Google PubSub] ベース接続の認証タイプを変更することはできません。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

ソース接続を作成する最初の手順は、[!DNL PubSub] ソースを認証し、ベース接続 ID を生成することです。ベース接続 ID を使用すると、ソース内を移動してファイルを探索し、データタイプや形式に関する情報など、取り込みたい特定の項目を識別できます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に [!DNL PubSub] 認証資格情報をリクエストパラメーターの一部として指定します。

[!DNL PubSub] ソースでは、認証中に許可するアクセスの種類を指定できます。 アカウントを設定して、特定の[!DNL PubSub] トピックとサブスクリプションへのルートアクセスを持たせたり、アクセスを制限したりできます。

>[!NOTE]
>
>[!DNL PubSub] プロジェクトに割り当てられたプリンシパル （役割）は、[!DNL PubSub] プロジェクト内で作成されたすべてのトピックとサブスクリプションで継承されます。 プリンシパル（役割）に特定のトピックへのアクセス権を付与する場合は、そのプリンシパル（役割）をトピックの対応するサブスクリプションにも追加する必要があります。 詳しくは、アクセス制御[[!DNL PubSub] に関する](<https://cloud.google.com/pubsub/docs/access-control>) ドキュメントを参照してください。

**API 形式**

```http
POST /connections
```

>[!BEGINTABS]

>[!TAB  プロジェクトベースの認証]

プロジェクトベースの認証でベース接続を作成するには、`/connections` エンドポイントにPOST リクエストを行い、リクエスト本文に`projectId`と`credentials`を指定します。

+++ リクエスト

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Google PubSub connection",
      "description": "Google PubSub connection",
      "auth": {
          "specName": "Project Based Authentication",
          "params": {
              "projectId": "{PROJECT_ID}",
              "credentials": "{CREDENTIALS}"
          }
      },
      "connectionSpec": {
          "id": "70116022-a743-464a-bbfe-e226a7f8210c",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.projectId` | [!DNL PubSub] の認証に必要なプロジェクト ID。 |
| `auth.params.credentials` | [!DNL PubSub] の認証に必要な資格情報またはキー。 |
| `connectionSpec.id` | [!DNL PubSub] 接続仕様 ID：`70116022-a743-464a-bbfe-e226a7f8210c`。 |

+++

+++ 応答

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。このベース接続 ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

+++

>[!TAB  トピックおよびサブスクリプションベースの認証]

トピックおよびサブスクリプションベースの認証を使用してベース接続を作成するには、`/connections` エンドポイントにPOST リクエストを行い、リクエスト本文に`credentials`、`topicName`、および`subscriptionName`を指定します。

+++ リクエスト

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Google PubSub connection",
      "description": "Google PubSub connection",
      "auth": {
          "specName": "Topic & Subscription Based Authentication",
          "params": {
              "credentials": "{CREDENTIALS}",
              "topicName": "projects/{PROJECT_ID}/topics/{TOPIC_ID}",
              "subscriptionName": "projects/{PROJECT_ID}/subscriptions/{SUBSCRIPTION_ID}"
          }
      },
      "connectionSpec": {
          "id": "70116022-a743-464a-bbfe-e226a7f8210c",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.credentials` | [!DNL PubSub] の認証に必要な資格情報またはキー。 |
| `auth.params.topicName` | アクセスを提供する[!DNL PubSub] ソースのプロジェクト IDとトピック IDのペア。 |
| `auth.params.subscriptionName` | アクセスを提供する[!DNL PubSub] ソースのプロジェクト IDとサブスクリプション IDのペア。 |
| `connectionSpec.id` | [!DNL PubSub] 接続仕様 ID：`70116022-a743-464a-bbfe-e226a7f8210c`。 |

+++

+++ 応答

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。このベース接続 ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

+++

>[!ENDTABS]


## ソース接続の作成 {#source}

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
      "name": "Google PubSub source connection",
      "description": "A source connection for Google PubSub",
      "baseConnectionId": "4cb0c374-d3bb-4557-b139-5712880adc55",
      "connectionSpec": {
          "id": "70116022-a743-464a-bbfe-e226a7f8210c",
          "version": "1.0"
      },
      "data": {
          "format": "json"
      },
      "params": {
          "topicName": "projects/{PROJECT_ID}/topics/{TOPIC_ID}",
          "subscriptionName": "projects/{PROJECT_ID}/subscriptions/{SUBSCRIPTION_ID}",
          "dataType": "raw"
      }
  }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前はわかりやすいものにしてください。 |
| `description` | 指定するとソース接続に関する詳細情報を含めることができるオプションの値。 |
| `baseConnectionId` | 前の手順で生成された [!DNL PubSub] ソースのベース接続 ID。 |
| `connectionSpec.id` | [!DNL PubSub] の固定接続仕様 ID。この ID は `70116022-a743-464a-bbfe-e226a7f8210c` です。 |
| `data.format` | 取り込む [!DNL PubSub] データの形式。現在、サポートされているデータ形式は `json` のみです。 |
| `params.topicName` | [!DNL PubSub] トピックの名前。 [!DNL PubSub]では、トピックはメッセージのフィードを表す名前付きリソースです。 |
| `params.subscriptionName` | 特定のトピックに対応するサブスクリプション名。 [!DNL PubSub]では、サブスクリプションを使用してトピックからメッセージを読み取ることができます。 1つまたは複数のサブスクリプションを1つのトピックに割り当てることができます。 |
| `params.dataType` | このパラメーターは、取り込まれるデータのタイプを定義します。`raw` および `xdm` を含むデータタイプがサポートされています。 |

**応答**

リクエストが成功した場合は、新しく作成されたソース接続の一意の ID（`id`）が返されます。この ID は、次のチュートリアルでデータフローを作成する際に必要です。

```json
{
    "id": "e96d6135-4b50-446e-922c-6dd66672b6b2",
    "etag": "\"66013508-0000-0200-0000-5f6e2ae70000\""
}
```

>[!NOTE]
>
>ストリーミングデータフローを作成または更新した後、データの損失やデータの削除の可能性のあるインスタンスを防ぐには、データの取り込みを5分間だけ一時停止する必要があります。

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL PubSub] ソース接続を作成しました。次のチュートリアルでは、このソース接続 ID を使用して、[&#x200B; [!DNL Flow Service] API を使用したストリーミングデータフローの作成](../../collect/streaming.md)を行います。
