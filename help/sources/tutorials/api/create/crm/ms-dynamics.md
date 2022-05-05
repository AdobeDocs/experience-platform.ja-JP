---
keywords: Experience Platform；ホーム；人気の高いトピック；Microsoft Dynamics;microsoft dynamics;dynamics;Dynamics
solution: Experience Platform
title: フローサービス API を使用したMicrosoft Dynamics ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して、Platform をMicrosoft Dynamics アカウントに接続する方法を説明します。
exl-id: 423c6047-f183-4d92-8d2f-cc8cc26647ef
source-git-commit: 17055f76800deadacf435970a691cec79c9f1d17
workflow-type: tm+mt
source-wordcount: '639'
ht-degree: 42%

---

# [!DNL Flow Service] API を使用した [!DNL Microsoft Dynamics] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、のベース接続を作成する手順を説明します。 [!DNL Microsoft Dynamics] （以下「」という。）[!DNL Dynamics]」) [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：Experience Platform を使用すると、様々なソースからデータを取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、を使用して Platform を Dynamics アカウントに正常に接続するために知っておく必要がある追加情報を示します。 [!DNL Flow Service] API

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL Dynamics] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `serviceUri` | のサービス URL [!DNL Dynamics] インスタンス。 |
| `username` | のユーザー名 [!DNL Dynamics] ユーザーアカウント。 |
| `password` | ユーザーのパスワード [!DNL Dynamics] アカウント |
| `servicePrincipalId` | のクライアント ID [!DNL Dynamics] アカウント この ID は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `servicePrincipalKey` | サービスプリンシパル秘密鍵。 この資格情報は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。[!DNL Dynamics] の接続仕様 ID は `38ad80fe-8b06-4938-94f4-d4ee80266b07` です。 |

使い始める方法について詳しくは、 [この [!DNL Dynamics] 文書](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/authenticate-oauth).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Dynamics] 認証資格情報をリクエストパラメーターの一部として使用します。

### 基本認証を使用した [!DNL Dynamics] ベース接続の作成

を作成するには、以下を実行します。 [!DNL Dynamics] 基本認証を使用したベース接続、 [!DNL Flow Service] 接続の `serviceUri`, `username`、および `password`.

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
        "name": "Dynamics connection",
        "description": "Dynamics connection using basic auth",
        "auth": {
            "specName": "Basic Authentication for Dynamics-Online",
            "params": {
                "serviceUri": "{SERVICE_URI}",
                "username": "{USERNAME}",
                "password": "{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "38ad80fe-8b06-4938-94f4-d4ee80266b07",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.serviceUri` | 次に、 [!DNL Dynamics] インスタンス。 |
| `auth.params.username` | ユーザー名 [!DNL Dynamics] アカウント |
| `auth.params.password` | ユーザーに関連付けられたパスワード [!DNL Dynamics] アカウント |
| `connectionSpec.id` | この [!DNL Dynamics] 接続仕様 ID: `38ad80fe-8b06-4938-94f4-d4ee80266b07` |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の識別子 (`id`) をクリックします。 この ID は、次の手順で CRM システムを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"9e0052a2-0000-0200-0000-5e35tb330000\""
}
```

### の作成 [!DNL Dynamics] サービスプリンシパルキーベース認証を使用したベース接続

を作成するには、以下を実行します。 [!DNL Dynamics] サービスプリンシパルキーベースの認証を使用したベース接続で、 [!DNL Flow Service] 接続の `serviceUri`, `servicePrincipalId`、および `servicePrincipalKey`.

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
        "name": "Dynamics connection",
        "description": "Dynamics connection using key-based authentication",
        "auth": {
            "specName": "Service Principal Key Based Authentication",
            "params": {
                "serviceUri": "{SERVICE_URI}",
                "servicePrincipalId": "{SERVICE_PRINCIPAL_ID}",
                "servicePrincipalKey": "{SERVICE_PRINCIPAL_KEY}"
            }
        },
        "connectionSpec": {
            "id": "38ad80fe-8b06-4938-94f4-d4ee80266b07",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.serviceUri` | 次に、 [!DNL Dynamics] インスタンス。 |
| `auth.params.servicePrincipalId` | のクライアント ID [!DNL Dynamics] アカウント この ID は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `auth.params.servicePrincipalKey` | サービスプリンシパル秘密鍵。 この資格情報は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `connectionSpec.id` | この [!DNL Dynamics] 接続仕様 ID: `38ad80fe-8b06-4938-94f4-d4ee80266b07` |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の識別子 (`id`) をクリックします。 この ID は、次の手順で CRM システムを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"9e0052a2-0000-0200-0000-5e35tb330000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Microsoft Dynamics] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [を使用してデータテーブルの構造と内容を調べる [!DNL Flow Service] API](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/crm.md)
