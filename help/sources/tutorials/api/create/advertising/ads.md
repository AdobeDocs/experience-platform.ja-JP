---
title: API を使用したGoogle広告のExperience Platformへの接続
description: Flow Service API を使用してAdobe Experience PlatformをGoogle Ads に接続する方法について説明します。
exl-id: 4658e392-1bd9-4e74-aa05-96109f9b62a0
source-git-commit: a0977e98219797eda14dd8d7ddb6cf3f1410cef0
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 27%

---

# [!DNL Google Ads] API を使用した [!DNL Flow Service] のExperience Platformへの接続

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

[!DNL Google Ads]API[[!DNL Flow Service]  を使用して ](https://developer.adobe.com/experience-platform-apis/references/flow-service/) アカウントをAdobe Experience Platformに接続する方法については、このチュートリアルをお読みください。

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Google Ads] API を使用してに正常に接続するために必要な追加情報を示 [!DNL Flow Service] ています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

### 必要な資格情報の収集

認証について詳しくは、[[!DNL Google Ads]  ソースの概要 ](../../../../connectors/advertising/ads.md) を参照してください。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際にGoogle Ads 認証資格情報をリクエストパラメーターの一部として指定します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、Google Ads のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json'
  -d '{
      "name": "Google Ads base connection",
      "description": "Google Ads base connection",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "clientCustomerID": "{CLIENT_CUSTOMER_ID}",
              "loginCustomerID": "{LOGIN_CUSTOMER_ID}",
              "developerToken": "{DEVELOPER_TOKEN}",
              "refreshToken": "{REFRESH_TOKEN}",
              "clientId": "{CLIENT_ID}",
              "clientSecret": "{CLIENT_SECRET}",
              "googleAdsApiVersion": "v19"

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
| `auth.params.clientCustomerID` | [!DNL Google Ads] アカウントのクライアント顧客 ID。 |
| `auth.params.loginCustomerID` | [!DNL Google Ads] manager アカウントに対応するログイン顧客 ID。 |
| `auth.params.developerToken` | [!DNL Google Ads] アカウントの開発者トークン。 |
| `auth.params.refreshToken` | [!DNL Google Ads] アカウントの更新トークン。 |
| `auth.params.clientID` | [!DNL Google Ads] アカウントのクライアント ID。 |
| `auth.params.clientSecret` | [!DNL Google Ads] アカウントのクライアント秘密鍵。 |
| `auth.params.googleAdsApiVersion` | 使用している [!DNL Google Ads] API のバージョン。 Experience Platformは現在、バージョン `v19` 以降をサポートしています。 互換性を確保するために、これらのサポート対象バージョンのいずれかを使用していることを確認します。 |
| `connectionSpec.id` | [!DNL Google Ads] 接続仕様 ID: `d771e9c1-4f26-40dc-8617-ce58c4b53702`。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 広告データを取り込むデータフローの作成

このチュートリアルでは、[!DNL Google Ads] API を使用して [!DNL Flow Service] ベース接続を作成し、[!DNL Google Ads] アカウントをExperience Platformに接続しました。 このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、広告データをExperience Platformに取り込むデータフローの作成](../../collect/advertising.md)
