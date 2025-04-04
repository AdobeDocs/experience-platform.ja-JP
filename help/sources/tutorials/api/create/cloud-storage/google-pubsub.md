---
title: Flow Service API を使用した Google PubSub ソース接続の作成
description: Flow Service API を使用して Adobe Experience Platform を Google PubSub アカウントに接続する方法を説明します。
badgeUltimate: label="Ultimate" type="Positive"
exl-id: f5b8f9bf-8a6f-4222-8eb2-928503edb24f
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1153'
ht-degree: 46%

---

# Flow Service API を使用した [!DNL Google PubSub] ソース接続の作成

>[!IMPORTANT]
>
>Real-Time Customer Data Platform Ultimateを購入したユーザーは、ソースカタログで [!DNL Google PubSub] ソースを利用できます。

このチュートリアルでは、 [[!DNL Flow Service] API](<https://www.adobe.io/experience-platform-apis/references/flow-service/>) を使用して [!DNL Google PubSub]（以下「[!DNL PubSub]」）を Experience Platform に接続する手順を詳しく説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してExperience Platformに正しく接続するために必要 [!DNL PubSub] 追加情報を示します。

### 必要な資格情報の収集

[!DNL PubSub] アカウントを [!DNL Flow Service] に接続するには、以下に説明する接続プロパティの値を指定する必要があります。 認証と前提条件の設定について詳しくは、[[!DNL PubSub source]  概要 ](../../../../connectors/cloud-storage/google-pubsub.md#prerequisites) を参照してください。

>[!BEGINTABS]

>[!TAB  プロジェクトベースの認証 ]

| 資格情報 | 説明 |
| --- | --- |
| `projectId` | [!DNL PubSub] の認証に必要なプロジェクト ID。 |
| `credentials` | [!DNL PubSub] の認証に必要な資格情報。 資格情報から空白を削除した後、必ず完全な JSON ファイルを配置してください。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソースターゲット接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL PubSub] 接続仕様 ID は `70116022-a743-464a-bbfe-e226a7f8210c` です。 |

>[!TAB  トピックおよび購読ベースの認証 ]

| 資格情報 | 説明 |
| --- | --- |
| `credentials` | [!DNL PubSub] の認証に必要な資格情報。 資格情報から空白を削除した後、必ず完全な JSON ファイルを配置してください。 |
| `topicName` | メッセージのフィードを表すリソースの名前。 [!DNL PubSub] ソース内の特定のデータストリームへのアクセスを提供する場合は、トピック名を指定する必要があります。 トピック名の形式は `projects/{PROJECT_ID}/topics/{TOPIC_ID}` です。 |
| `subscriptionName` | [!DNL PubSub] サブスクリプションの名前。 ま [!DNL PubSub]、購読を使用すると、メッセージの公開先のトピックを購読することで、メッセージを受信できます。 **メモ**:1 つの [!DNL PubSub] 購読は、1 つのデータフローに対してのみ使用できます。 複数のデータフローを作成するには、複数の購読が必要です。 配信登録名の形式は `projects/{PROJECT_ID}/subscriptions/{SUBSCRIPTION_ID}` です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソースターゲット接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL PubSub] 接続仕様 ID は `70116022-a743-464a-bbfe-e226a7f8210c` です。 |

>[!ENDTABS]

これらの値について詳しくは、この [[!DNL PubSub]  認証 ](https://cloud.google.com/pubsub/docs/authentication) ドキュメントを参照してください。 サービスアカウントベースの認証を使用するには、こちらの [[!DNL PubSub]  サービスアカウントの作成に関するガイド ](https://cloud.google.com/docs/authentication/production#create_service_account) で、資格情報の生成手順を確認してください。

>[!TIP]
>
>サービスアカウントベースの認証を使用している場合は、サービスアカウントに十分なユーザーアクセス権が付与され、資格情報をコピー＆ペーストする際に、JSON 内に余分な空白がないことを確認してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

>[!TIP]
>
>作成した後は、[!DNL Google PubSub] ベース接続の認証タイプを変更できません。 認証タイプを変更するには、新しいベース接続を作成する必要があります。

ソース接続を作成する最初の手順は、[!DNL PubSub] ソースを認証し、ベース接続 ID を生成することです。ベース接続 ID を使用すると、ソース内を移動してファイルを探索し、データのタイプや形式に関する情報など、取り込みたい特定の項目を識別できます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に [!DNL PubSub] 認証資格情報をリクエストパラメーターの一部として指定します。

[!DNL PubSub] ソースでは、認証時に許可するアクセスの種類を指定できます。 ルートアクセス権を持つように、または特定の [!DNL PubSub] トピックおよび購読へのアクセスを制限するために、アカウントを設定できます。

>[!NOTE]
>
>[!DNL PubSub] プロジェクトに割り当てられたプリンシパル（役割）は、[!DNL PubSub] プロジェクト内で作成されたすべてのトピックと購読に継承されます。 プリンシパル（役割）に特定のトピックへのアクセス権を付与する場合は、そのプリンシパル（役割）もトピックの対応するサブスクリプションに追加する必要があります。 詳しくは、[[!DNL PubSub]  アクセス制御に関するドキュメント ](<https://cloud.google.com/pubsub/docs/access-control>) を参照してください。

**API 形式**

```http
POST /connections
```

>[!BEGINTABS]

>[!TAB  プロジェクトベースの認証 ]

プロジェクトベースの認証を使用したベース接続を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、リクエスト本文で `projectId` と `credentials` を指定します。

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

++++

+++応答

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。このベース接続 ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

++++

>[!TAB  トピックおよび購読ベースの認証 ]

トピックおよび購読ベースの認証を使用したベース接続を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、リクエスト本文で `credentials`、`topicName`、`subscriptionName` を指定します。

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
| `auth.params.topicName` | アクセス権を付与する [!DNL PubSub] ソースのプロジェクト ID とトピック ID のペア。 |
| `auth.params.subscriptionName` | アクセス権を付与する [!DNL PubSub] ソースのプロジェクト ID と購読 ID のペア。 |
| `connectionSpec.id` | [!DNL PubSub] 接続仕様 ID：`70116022-a743-464a-bbfe-e226a7f8210c`。 |

+++

+++応答

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。このベース接続 ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

++++

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
| `params.topicName` | [!DNL PubSub] トピックの名前。 ま [!DNL PubSub]、トピックは、メッセージのフィードを表す名前付きリソースです。 |
| `params.subscriptionName` | 特定のトピックに対応するサブスクリプション名。 [!DNL PubSub] では、購読を使用すると、トピックからメッセージを読み取ることができます。 1 つまたは複数の購読を 1 つのトピックに割り当てることができます。 |
| `params.dataType` | このパラメーターは、取り込まれるデータのタイプを定義します。`raw` および `xdm` を含むデータタイプがサポートされています。 |

**応答**

リクエストが成功した場合は、新しく作成されたソース接続の一意の ID（`id`）が返されます。この ID は、次のチュートリアルでデータフローを作成する際に必要です。

```json
{
    "id": "e96d6135-4b50-446e-922c-6dd66672b6b2",
    "etag": "\"66013508-0000-0200-0000-5f6e2ae70000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL PubSub] ソース接続を作成しました。次のチュートリアルでは、このソース接続 ID を使用して、[ [!DNL Flow Service] API を使用したストリーミングデータフローの作成](../../collect/streaming.md)を行います。
