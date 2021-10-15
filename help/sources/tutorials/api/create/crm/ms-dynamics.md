---
keywords: Experience Platform；ホーム；人気のあるトピック；Microsoft Dynamics;microsoft dynamics;dynamics;Dynamics
solution: Experience Platform
title: フローサービス API を使用したMicrosoft Dynamics Base 接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用して、Platform をMicrosoft Dynamics アカウントに接続する方法を説明します。
exl-id: 423c6047-f183-4d92-8d2f-cc8cc26647ef
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '633'
ht-degree: 9%

---

# [!DNL Flow Service] API を使用して [!DNL Microsoft Dynamics] ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Microsoft Dynamics]（以下「[!DNL Dynamics]」と呼ばれます）の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して Platform を Dynamics アカウントに正常に接続するために必要な追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL Dynamics] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `serviceUri` | [!DNL Dynamics] インスタンスのサービス URL。 |
| `username` | [!DNL Dynamics] ユーザーアカウントのユーザー名。 |
| `password` | [!DNL Dynamics] アカウントのパスワード。 |
| `servicePrincipalId` | [!DNL Dynamics] アカウントのクライアント ID。 この ID は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `servicePrincipalKey` | サービスプリンシパル秘密鍵 この資格情報は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Dynamics] の接続仕様 ID は次のとおりです。`38ad80fe-8b06-4938-94f4-d4ee80266b07`. |

開始方法の詳細については、[this [!DNL Dynamics] document](https://docs.microsoft.com/en-us/powerapps/developer/common-data-service/authenticate-oauth) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL Dynamics] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

### 基本認証を使用して [!DNL Dynamics] ベース接続を作成する

基本的なPOSTを使用して [!DNL Dynamics] ベース接続を作成するには、接続の `serviceUri`、`username` および `password` に値を指定しながら、[!DNL Flow Service] API に対して認証リクエストを実行します。

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
| `auth.params.serviceUri` | [!DNL Dynamics] インスタンスに関連付けられたサービス URI。 |
| `auth.params.username` | [!DNL Dynamics] アカウントに関連付けられているユーザー名。 |
| `auth.params.password` | [!DNL Dynamics] アカウントに関連付けられたパスワード。 |
| `connectionSpec.id` | [!DNL Dynamics] 接続仕様 ID:`38ad80fe-8b06-4938-94f4-d4ee80266b07` |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の識別子 (`id`) が含まれます。 この ID は、次の手順で CRM システムを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"9e0052a2-0000-0200-0000-5e35tb330000\""
}
```

### サービスプリンシパルキーベースの認証を使用して [!DNL Dynamics] ベース接続を作成する

サービスプリンシパルのキーベースのPOSTを使用して [!DNL Dynamics] ベース接続を作成するには、接続の `serviceUri`、`servicePrincipalId` および `servicePrincipalKey` に値を指定しながら、[!DNL Flow Service] API に認証リクエストを実行します。

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
| `auth.params.serviceUri` | [!DNL Dynamics] インスタンスに関連付けられたサービス URI。 |
| `auth.params.servicePrincipalId` | [!DNL Dynamics] アカウントのクライアント ID。 この ID は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `auth.params.servicePrincipalKey` | サービスプリンシパル秘密鍵 この資格情報は、サービスプリンシパルとキーベースの認証を使用する場合に必要です。 |
| `connectionSpec.id` | [!DNL Dynamics] 接続仕様 ID:`38ad80fe-8b06-4938-94f4-d4ee80266b07` |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の識別子 (`id`) が含まれます。 この ID は、次の手順で CRM システムを調べるために必要です。

```json
{
    "id": "4cb0c374-d3bb-4557-b139-5712880adc55",
    "etag": "\"9e0052a2-0000-0200-0000-5e35tb330000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Dynamics] 接続を作成し、接続の一意の ID 値を取得しました。 この ID は、次のチュートリアルでフローサービス API](../../explore/crm.md) を使用して CRM システムを調べる方法を学ぶ際に使用できます。[
