---
keywords: Experience Platform；ホーム；人気のあるトピック；servicenow;ServiceNow
solution: Experience Platform
title: フローサービス API を使用した ServiceNow ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを ServiceNow サーバーに接続する方法を説明します。
exl-id: 39d0e628-5c07-4371-a5af-ac06385db891
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 11%

---

# [!DNL Flow Service] API を使用して [!DNL ServiceNow] ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL Google ServiceNow] の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL ServiceNow] サーバーに正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL ServiceNow] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `endpoint` | [!DNL ServiceNow] サーバのエンドポイント。 |
| `username` | [!DNL ServiceNow] サーバーに接続して認証を行う際に使用するユーザー名。 |
| `password` | [!DNL ServiceNow] サーバーに接続して認証を行うためのパスワード。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL ServiceNow] の接続仕様 ID は次のとおりです。`eb13cb25-47ab-407f-ba89-c0125281c563`. |

使い始める方法について詳しくは、[ この ServiceNow のドキュメント ](https://developer.servicenow.com/app.do#!/rest_api_doc?v=newyork&amp;id=r_TableAPI-GET) を参照してください。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL ServiceNow] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL ServiceNow] のベース接続を作成します。

```shell
curl -X POST \
    'http://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Connection for service-now",
        "description": "Connection for service-now,
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "endpoint": "{ENDPOINT}",
                "username": "{USERNAME}",
                "password": "{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "eb13cb25-47ab-407f-ba89-c0125281c563",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.server` | [!DNL ServiceNow] サーバーのエンドポイント。 |
| `auth.params.username` | [!DNL ServiceNow] サーバーに接続して認証を行う際に使用するユーザー名。 |
| `auth.params.password` | [!DNL ServiceNow] サーバーに接続して認証を行うためのパスワード。 |
| `connectionSpec.id` | [!DNL ServiceNow] 接続仕様 ID:`eb13cb25-47ab-407f-ba89-c0125281c563` |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の識別子 (`id`) が含まれます。 この ID は、次の手順で CRM システムを調べるために必要です。

```json
{
    "id": "8a3ca3dd-6d00-4c95-bca3-dd6d00dc954b",
    "etag": "\"8e0052a2-0000-0200-0000-5e25fb330000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL ServiceNow] 接続を作成し、接続の一意の ID 値を取得しました。 この接続 ID は、次のチュートリアルでフローサービス API](../../explore/customer-success.md) を使用して顧客の成功システムを調べる方法を学ぶ際に使用できます。[
