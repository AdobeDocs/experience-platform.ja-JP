---
keywords: Experience Platform；ホーム；人気のあるトピック；bigquery;Google;google;Google BigQuery
solution: Experience Platform
title: フローサービスAPIを使用したGoogle BigQueryベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAdobe Experience PlatformをGoogle BigQueryに接続する方法を説明します。
exl-id: 51f90366-7a0e-49f1-bd57-b540fa1d15af
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 9%

---

# [!DNL Flow Service] APIを使用して[!DNL Google BigQuery]ベース接続を作成する

>[!NOTE]
>
>[!DNL Google BigQuery]コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ソースの概要](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Google BigQuery]（以下「[!DNL BigQuery]」と呼びます）のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL BigQuery]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL BigQuery]をPlatformに接続するには、次のOAuth 2.0認証値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `project` | クエリを実行するデフォルトの[!DNL BigQuery]プロジェクトのプロジェクトID。 |
| `clientID` | 更新トークンの生成に使用するID値。 |
| `clientSecret` | 更新トークンの生成に使用するシークレット値。 |
| `refreshToken` | [!DNL BigQuery]へのアクセスを承認するために使用される[!DNL Google]から取得された更新トークン。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL BigQuery]の接続仕様IDは次のとおりです。`3c9b37f8-13a6-43d8-bad3-b863b941fedd`. |

これらの値の詳細については、この[[!DNL BigQuery] ドキュメント](https://cloud.google.com/storage/docs/json_api/v1/how-tos/authorizing)を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL BigQuery]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL BigQuery]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Google BigQuery connection",
        "description": "Google BigQuery connection",
        "auth": {
            "specName": "Basic Authentication",
            "type": "OAuth2.0",
            "params": {
                    "project": "{PROJECT}",
                    "clientId": "{CLIENT_ID},
                    "clientSecret": "{CLIENT_SECRET}",
                    "refreshToken": "{REFRESH_TOKEN}"
                }
        },
        "connectionSpec": {
            "id": "3c9b37f8-13a6-43d8-bad3-b863b941fedd",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.project` | クエリを実行するデフォルトの[!DNL BigQuery]プロジェクトのプロジェクトID。 対して |
| `auth.params.clientId` | 更新トークンの生成に使用するID値。 |
| `auth.params.clientSecret` | 更新トークンの生成に使用するクライアント値。 |
| `auth.params.refreshToken` | [!DNL BigQuery]へのアクセスを承認するために使用される[!DNL Google]から取得された更新トークン。 |
| `connectionSpec.id` | [!DNL Google BigQuery]接続仕様ID:`3c9b37f8-13a6-43d8-bad3-b863b941fedd`. |

**応答**

正常な応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6990abad-977d-41b9-a85d-17ea8cf1c0e4",
    "etag": "\"ca00acbf-0000-0200-0000-60149e1e0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL BigQuery]接続を作成し、接続の一意のID値を取得しました。 次のチュートリアルでは、フローサービスAPI](../../explore/database-nosql.md)を使用してデータベースやNoSQLシステムを調べる方法を学ぶ際に、この接続IDを使用できます。[
