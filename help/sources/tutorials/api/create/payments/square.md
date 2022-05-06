---
keywords: Experience Platform；ホーム；人気の高いトピック；正方形；四角形
title: フローサービス API を使用した正方形ベース接続の作成
description: フローサービス API を使用して Square をAdobe Experience Platformに接続する方法を説明します。
exl-id: 82c1d513-3b06-4ce9-b637-2c5a268da506
source-git-commit: 47a94b00e141b24203b01dc93834aee13aa6113c
workflow-type: tm+mt
source-wordcount: '542'
ht-degree: 45%

---

# [!DNL Flow Service] API を使用した [!DNL Square] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Square] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md): [!DNL Experience Platform] は、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL Square] の使用 [!DNL Flow Service] API

### 必要な認証情報の収集

[!DNL Flow Service] を [!DNL Square] に接続するには、次の接続プロパティの値を指定する必要があります。

| 認証情報 | 説明 |
| --- | --- |
| `host` | の URL [!DNL Square] インスタンス。 |
| `clientId` | 次に関連付けられたクライアント ID: [!DNL Square] アカウント |
| `clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL Square] アカウント |
| `accessToken` | アクセストークンは、 [!DNL Square] アカウントを OAuth 2.0 認証で使用します。 アクセストークンは、 [!DNL Square]. |
| `refreshToken` | 更新トークンは、現在のアクセストークンの有効期限が切れた後に新しいアクセストークンを生成するために使用されます。 更新トークンは、 [!DNL Square]. |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。の接続仕様 ID [!DNL Square] 次に該当： `2acf109f-9b66-4d5e-bc18-ebb2adcff8d5` |

これらの資格情報とその取得方法について詳しくは、 [[!DNL Square] OAuth に関するドキュメント](https://developer.squareup.com/docs/oauth-api/receive-and-manage-tokens).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Square] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Square] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Square Base Connection",
        "description": "Square Base Connection",
        "auth": {
        "specName": "OAuth2 Refresh Code",
        "params": {
            "host": "{HOST}",
            "clientId": "{CLIENT_ID}",
            "clientSecret": "{CLIENT_SECRET}"
            "accessToken": "{ACCESS_TOKEN}"
            "refreshToken": "{REFRESH_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "2acf109f-9b66-4d5e-bc18-ebb2adcff8d5",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.host` | の URL [!DNL Square] インスタンス。 |
| `auth.params.clientId` | 次に関連付けられたクライアント ID: [!DNL Square] アカウント |
| `auth.params.clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL Square] アカウント |
| `auth.params.accessToken` | アクセストークンは、 [!DNL Square] アカウントを OAuth 2.0 認証で使用します。 アクセストークンは、 [!DNL Square]. |
| `auth.params.refreshToken` | 更新トークンは、現在のアクセストークンの有効期限が切れた後に新しいアクセストークンを生成するために使用されます。 更新トークンは、 [!DNL Square]. |
| `connectionSpec.id` | この [!DNL Square] 接続仕様 ID: `2acf109f-9b66-4d5e-bc18-ebb2adcff8d5`. |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子 (`id`) をクリックします。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "24151d58-ffa7-4960-951d-58ffa7396097",
    "etag": "\"65015e9d-0000-0200-0000-5e89162d0000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Square] を使用した接続 [!DNL Flow Service] API を介して取得され、接続の一意の ID 値を取得している。 この ID は、次のチュートリアルで、 [フローサービス API を使用した支払い申請の調査](../../explore/payments.md).
