---
title: Flow Service API を使用した Google PubSub ソース接続の作成
description: Flow Service API を使用して Adobe Experience Platform を Google PubSub アカウントに接続する方法を説明します。
exl-id: f5b8f9bf-8a6f-4222-8eb2-928503edb24f
source-git-commit: f56cdc2dc67f2d4820d80d8e5bdec8306d852891
workflow-type: tm+mt
source-wordcount: '864'
ht-degree: 74%

---

# Flow Service API を使用した [!DNL Google PubSub] ソース接続の作成

このチュートリアルでは、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Google PubSub]（以下「[!DNL PubSub]」）を Experience Platform に接続する手順を詳しく説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：Experience Platform を使用すると、様々なソースからデータを取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、[!DNL Flow Service] API を使用して [!DNL PubSub] を Platform に正しく接続するために必要な追加情報を示します。

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL PubSub] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `projectId` | [!DNL PubSub] の認証に必要なプロジェクト ID。 |
| `credentials` | [!DNL PubSub] の認証に必要な資格情報またはキー。 |
| `topicId` | の ID [!DNL PubSub] メッセージのフィードを表すリソース。 トピック ID を指定する必要があるのは、 [!DNL Google PubSub] ソース。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソースターゲット接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL PubSub] 接続仕様 ID は `70116022-a743-464a-bbfe-e226a7f8210c` です。 |

これらの値について詳しくは、こちらの [[!DNL PubSub] 認証](https://cloud.google.com/pubsub/docs/authentication)に関するドキュメントを参照してください。サービスアカウントベースの認証を使用するには、こちらの[[!DNL PubSub] サービスアカウントの作成に関するガイド](https://cloud.google.com/docs/authentication/production#create_service_account)で、資格情報の生成手順を確認してください。

>[!TIP]
>
>サービスアカウントベースの認証を使用している場合は、サービスアカウントに十分なユーザーアクセス権が付与され、資格情報をコピー＆ペーストする際に、JSON 内に余分な空白がないことを確認してください。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ソース接続を作成する最初の手順は、[!DNL PubSub] ソースを認証し、ベース接続 ID を生成することです。ベース接続 ID を使用すると、ソース内を移動してファイルを探索し、データのタイプや形式に関する情報など、取り込みたい特定の項目を識別できます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に [!DNL PubSub] 認証資格情報をリクエストパラメーターの一部として指定します。

この手順では、トピック ID を指定して、アカウントがアクセスできるデータを定義できます。 そのトピック ID に関連付けられている購読のみにアクセスできます。

>[!NOTE]
>
>パブサブプロジェクトに割り当てられたプリンシパル（ロール）は、 [!DNL PubSub] プロジェクト。 特定のトピックに対するアクセス権を持つプリンシパル（役割）を追加する場合は、そのプリンシパル（役割）もトピックの対応するサブスクリプションに追加する必要があります。 詳しくは、 [[!DNL PubSub] アクセス制御に関するドキュメント](https://cloud.google.com/pubsub/docs/access-control).

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
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Google PubSub connection",
      "description": "Google PubSub connection",
      "auth": {
          "specName": "Google PubSub authentication credentials",
          "params": {
              "projectId": "acme-project",
              "credentials": "{CREDENTIALS}",
              "topicID": "acmeProjectAPI"
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
| `auth.params.topicID` | のトピック ID [!DNL PubSub] アクセスを提供するソース。 |
| `connectionSpec.id` | [!DNL PubSub] 接続仕様 ID：`70116022-a743-464a-bbfe-e226a7f8210c`。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。このベース接続 ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

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
          "topicId": "acme-project",
          "subscriptionId": "{SUBSCRIPTION_ID}",
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
| `params.topicId` | の名前または ID [!DNL PubSub] トピック。 In [!DNL PubSub]の場合、トピックは、メッセージのフィードを表す名前付きリソースです。 |
| `params.subscriptionId` | 特定のトピックに対応する購読 ID。 In [!DNL PubSub]を使用すると、購読を使用してトピックからメッセージを読み取ることができます。 1 つ以上の購読を 1 つのトピックに割り当てることができます。 |
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
