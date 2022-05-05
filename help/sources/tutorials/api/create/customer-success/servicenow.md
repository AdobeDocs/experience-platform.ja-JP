---
keywords: Experience Platform；ホーム；人気の高いトピック；servicenow;ServiceNow
solution: Experience Platform
title: フローサービス API を使用した ServiceNow ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを ServiceNow サーバーに接続する方法を説明します。
exl-id: 39d0e628-5c07-4371-a5af-ac06385db891
source-git-commit: 17055f76800deadacf435970a691cec79c9f1d17
workflow-type: tm+mt
source-wordcount: '479'
ht-degree: 57%

---

# [!DNL Flow Service] API を使用した [!DNL ServiceNow] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Google ServiceNow] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

以下の節では、 [!DNL ServiceNow] サーバー [!DNL Flow Service] API

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL ServiceNow] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `endpoint` | のエンドポイント [!DNL ServiceNow] サーバー。 |
| `username` | に接続するために使用するユーザー名 [!DNL ServiceNow] 認証用のサーバ。 |
| `password` | に接続するパスワード [!DNL ServiceNow] 認証用のサーバ。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。[!DNL ServiceNow] の接続仕様 ID は `eb13cb25-47ab-407f-ba89-c0125281c563` です。 |

の導入について詳しくは、 [この ServiceNow ドキュメント](https://developer.servicenow.com/app.do#!/rest_api_doc?v=newyork&amp;id=r_TableAPI-GET).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL ServiceNow] 認証資格情報をリクエストパラメーターの一部として使用します。

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
| `auth.params.server` | のエンドポイント [!DNL ServiceNow] サーバー。 |
| `auth.params.username` | に接続するために使用するユーザー名 [!DNL ServiceNow] 認証用のサーバ。 |
| `auth.params.password` | に接続するパスワード [!DNL ServiceNow] 認証用のサーバ。 |
| `connectionSpec.id` | この [!DNL ServiceNow] 接続仕様 ID: `eb13cb25-47ab-407f-ba89-c0125281c563` |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の識別子 (`id`) をクリックします。 この ID は、次の手順で CRM システムを調べるために必要です。

```json
{
    "id": "8a3ca3dd-6d00-4c95-bca3-dd6d00dc954b",
    "etag": "\"8e0052a2-0000-0200-0000-5e25fb330000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL ServiceNow] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [を使用してデータテーブルの構造と内容を調べる [!DNL Flow Service] API](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/customer-success.md)
