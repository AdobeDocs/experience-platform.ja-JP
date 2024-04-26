---
title: Flow Service API を使用した PathFactory ベース接続の作成
description: Flow Service API を使用して、Experience Platformに対して PathFactory アカウントを認証する方法を説明します。
last-substantial-update: 2024-04-30T00:00:00Z
hide: true
hidefromtoc: true
badge: ベータ版
source-git-commit: 18f6c253aec6815cf84272cbce340a9aa7ed8ab9
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 46%

---

# [!DNL Flow Service] API を使用した [!DNL PathFactory] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

のベース接続を作成する方法については、このドキュメントを参照してください。 [!DNL PathFactory] の使用 [[!DNL Flow Service] API](<https://www.adobe.io/experience-platform-apis/references/flow-service/>).

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)のガイドを参照してください。

次の節では、に正常に接続するために必要な追加情報を示します [!DNL PathFactory] の使用 [!DNL Flow Service] API です。

### 必要な資格情報の収集 {#gather-credentials}

Platform で PathFactory アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| ユーザー名 | あなたの [!DNL PathFactory] アカウントのユーザー名。 これは、システム内のアカウントを識別するために不可欠です。 |
| パスワード | に関連付けられたパスワード [!DNL PathFactory] アカウント。 権限のないアクセスを防ぐために、これが安全に保たれていることを確認します。 |
| ドメイン | に関連付けられたドメイン [!DNL PathFactory] アカウント。 これは通常、内の一意の ID を指します [!DNL PathFactory] URL。 |
| アクセストークン | システムとシステムの間の安全な通信を確保するために、API 認証に使用される一意のトークン [!DNL PathFactory]. |
| API エンドポイント | データにアクセスするための特定の API エンドポイント：訪問者、セッションおよびページビュー。 各エンドポイントは、取得可能な様々なデータセットに対応します。 **注意：** これらは、次の方法で事前定義されています [!DNL PathFactory] およびは、アクセスしようとしているデータに固有です。 <ul><li>**訪問者エンドポイント**: `/api/public/v3/data_lake_apis/visitors.json`</li><li>**セッションエンドポイント**: `/api/public/v3/data_lake_apis/sessions.json`</li><li>**ページビューエンドポイント**: `/api/public/v3/data_lake_apis/page_views.json`</li></ul> |

資格情報の保護および使用方法、アクセストークンの取得および更新方法について詳しくは、を参照してください。 [[!DNL PathFactory] サポートセンター](https://support.pathfactory.com/categories/adobe/). このリソースでは、資格情報の管理と、効果的で安全な API 統合の確保に関する包括的なガイドを提供します。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、次に対してPOSTリクエストを実行します。 `/connections` を指定する際のエンドポイント [!DNL PathFactory] リクエスト本文の一部としての認証資格情報。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL PathFactory] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "PathFactory base connection",
      "description": "PathFactory base connection",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "host": "acme-ab12c3d4e5fg6hijk7lmnop8qrst"
              "clientId": "pathfactory",
              "clientSecret": "xxxx"
          }
      },
      "connectionSpec": {
          "id": "ea1c2a08-b722-11eb-8529-0242ac130003",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.clientId` | に関連付けられたクライアント ID [!DNL PathFactory] アプリケーション。 |
| `auth.params.clientSecret` | に関連付けられたクライアント秘密鍵 [!DNL PathFactory] アプリケーション。 |
| `connectionSpec.id` | この [!DNL PathFactory] 接続仕様 ID: `ea1c2a08-b722-11eb-8529-0242ac130003`. |

**応答**

リクエストが成功した場合は、一意の接続識別子（`id`）に設定します。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL PathFactory] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service]  API を使用した、マーケティング自動化データを Platform に取り込むデータフローの作成](../../collect/marketing-automation.md)
