---
keywords: Experience Platform；ホーム；人気のあるトピック；google adwords;Google AdWords;adwords
solution: Experience Platform
title: フローサービスAPIを使用したGoogle AdWordsベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAdobe Experience PlatformをGoogle AdWordsに接続する方法を説明します。
exl-id: 4658e392-1bd9-4e74-aa05-96109f9b62a0
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '525'
ht-degree: 9%

---

# [!DNL Flow Service] APIを使用して[!DNL Google AdWords]ベース接続を作成する

>[!NOTE]
>
>[!DNL Google AdWords]コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ソースの概要](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Google AdWords]（以下「[!DNL AdWords]」と呼びます）のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL AdWords]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL AdWords]と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `clientCustomerId` | [!DNL AdWords]アカウントのクライアント顧客ID。 |
| `developerToken` | マネージャーアカウントに関連付けられている開発者トークン。 |
| `refreshToken` | [!DNL AdWords]へのアクセスを承認するために[!DNL Google]から取得された更新トークン。 |
| `clientId` | 更新トークンの取得に使用する[!DNL Google]アプリケーションのクライアントID。 |
| `clientSecret` | 更新トークンの取得に使用する[!DNL Google]アプリケーションのクライアント秘密鍵。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL AdWords]の接続仕様IDは次のとおりです。`d771e9c1-4f26-40dc-8617-ce58c4b53702`. |

これらの値について詳しくは、この[Google AdWordsのドキュメント](https://developers.google.com/adwords/api/docs/guides/authentication)を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL AdWords]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL AdWords]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json'
    -d '{
        "name": "google-AdWords connection",
        "description": "Connection for google-AdWords",
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "clientCustomerID": "{CLIENT_CUSTOMER_ID}",
                "developerToken": "{DEVELOPER_TOKEN}",
                "authenticationType": "{AUTHENTICATION_TYPE}"
                "clientId": "{CLIENT_ID}",
                "clientSecret": "{CLIENT_SECRET}",
                "refreshToken": "{REFRESH_TOKEN}"
            }
        },
        "connectionSpec": {
            "id": "d771e9c1-4f26-40dc-8617-ce58c4b53702",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.clientCustomerID` | [!DNL AdWords]アカウントのクライアント顧客ID。 |
| `auth.params.developerToken` | [!DNL AdWords]アカウントの開発者トークン。 |
| `auth.params.refreshToken` | [!DNL AdWords]アカウントの更新トークン。 |
| `auth.params.clientID` | [!DNL AdWords]アカウントのクライアントID。 |
| `auth.params.clientSecret` | [!DNL AdWords]アカウントのクライアント秘密鍵。 |
| `connectionSpec.id` | [!DNL Google AdWords]接続仕様ID:`d771e9c1-4f26-40dc-8617-ce58c4b53702`. |

**応答**

正常な応答は、新しく作成されたベース接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL AdWords]ベース接続を作成し、接続の一意のID値を取得しました。 このIDは、次のチュートリアルでフローサービスAPI](../../explore/advertising.md)を使用して広告システムを[探索する方法を学ぶ際に使用できます。
