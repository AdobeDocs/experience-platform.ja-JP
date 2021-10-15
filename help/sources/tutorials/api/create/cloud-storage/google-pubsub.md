---
keywords: Experience Platform；ホーム；人気のあるトピック；Google PubSub;google pubsub
solution: Experience Platform
title: フローサービス API を使用したGoogle PubSub ソース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience PlatformをGoogle PubSub アカウントに接続する方法を説明します。
exl-id: f5b8f9bf-8a6f-4222-8eb2-928503edb24f
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '740'
ht-degree: 8%

---

# フローサービス API を使用した [!DNL Google PubSub] ソース接続の作成

>[!NOTE]
>
>[!DNL Google PubSub] コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Google PubSub]（以下「[!DNL PubSub]」と呼びます）をExperience Platformに接続する手順について説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL PubSub] を Platform に正しく接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL PubSub] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `projectId` | [!DNL PubSub] の認証に必要なプロジェクト ID です。 |
| `credentials` | [!DNL PubSub] の認証に必要な資格情報またはキー。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソースターゲット接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL PubSub] 接続仕様 ID は次のとおりです。`70116022-a743-464a-bbfe-e226a7f8210c`. |

これらの値の詳細については、[[!DNL PubSub]  認証 ](https://cloud.google.com/pubsub/docs/authentication) のドキュメントを参照してください。 サービスアカウントベースの認証を使用するには、この [[!DNL PubSub]  サービスアカウントの作成に関するガイド ](https://cloud.google.com/docs/authentication/production#create_service_account) を参照して、資格情報の生成手順を確認してください。

>[!TIP]
>
>サービスアカウントベースの認証を使用している場合、資格情報をコピー&amp;ペーストする際に、サービスアカウントへの十分なユーザーアクセス権が付与され、JSON に余分な空白がないことを確認します。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ソース接続を作成する最初の手順は、[!DNL PubSub] ソースを認証し、ベース接続 ID を生成することです。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を識別できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL PubSub] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

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
        "name": "Google PubSub connection",
        "description": "Google PubSub connection",
        "auth": {
            "specName": "Google PubSub authentication credentials",
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
| `auth.params.projectId` | [!DNL PubSub] の認証に必要なプロジェクト ID です。 |
| `auth.params.credentials` | [!DNL PubSub] の認証に必要な資格情報またはキー。 |
| `connectionSpec.id` | [!DNL PubSub] 接続仕様 ID:`70116022-a743-464a-bbfe-e226a7f8210c`. |

**応答**

正常な応答は、新しく作成された接続の詳細 ( 一意の識別子 (`id`) を含む ) を返します。 このベース接続 ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"6507cfd8-0000-0200-0000-5e18fc600000\""
}
```

## ソース接続の作成 {#source}

ソース接続は、データの取り込み元の外部ソースへの接続を作成し、管理します。 ソース接続は、データソース、データ形式、データフローの作成に必要なソース接続 ID などの情報で構成されます。 ソース接続インスタンスは、テナントと IMS 組織に固有です。

ソース接続を作成するには、[!DNL Flow Service] API の `/sourceConnections` エンドポイントにPOSTリクエストを実行します。

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
            "topicId": "{TOPIC_ID}",
            "subscriptionId": "{SUBSCRIPTION_ID}",
            "dataType": "raw"
        }
    }'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | ソース接続の名前。 ソース接続の情報を検索する際に使用できるので、ソース接続の名前がわかりやすいことを確認します。 |
| `description` | ソース接続に関する詳細情報を含めるために指定できるオプションの値です。 |
| `baseConnectionId` | 前の手順で生成した [!DNL PubSub] ソースのベース接続 ID。 |
| `connectionSpec.id` | [!DNL PubSub] の固定接続仕様 ID。 この ID は、です。`70116022-a743-464a-bbfe-e226a7f8210c` |
| `data.format` | 取り込む [!DNL PubSub] データの形式。 現在サポートされているデータ形式は `json` のみです。 |
| `params.topicId` | トピック ID は、パブリッシャーから送信されるメッセージに固有の名前が付いたリソースを定義します |
| `params.subscriptionId` | 購読 ID は、購読アプリケーションに配信される単一の特定のトピックからのメッセージのストリームを表す、特定の名前付きリソースを定義します。 |
| `params.dataType` | このパラメーターは、取り込まれるデータのタイプを定義します。 次のようなデータタイプがサポートされています。`raw` と `xdm`。 |

**応答**

正常な応答は、新しく作成されたソース接続の一意の識別子 (`id`) を返します。 この ID は、次のチュートリアルでデータフローを作成する際に必要になります。

```json
{
    "id": "e96d6135-4b50-446e-922c-6dd66672b6b2",
    "etag": "\"66013508-0000-0200-0000-5f6e2ae70000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL PubSub] ソース接続を作成しました。 次のチュートリアルでこのソース接続 ID を使用して、 [!DNL Flow Service] API](../../collect/streaming.md) を使用してストリーミングデータフローを作成できます。[
