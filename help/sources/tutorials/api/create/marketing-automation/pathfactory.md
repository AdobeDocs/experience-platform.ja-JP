---
title: Flow Service API を使用した PathFactory ベース接続の作成
description: Flow Service API を使用して、Experience Platformに対して PathFactory アカウントを認証する方法を説明します。
exl-id: 2bdfe38b-d3f7-480f-87c6-0b98b9521be2
source-git-commit: 40c3745920204983f5388de6cba1402d87eda71c
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 24%

---

# [!DNL Flow Service] API を使用した [!DNL PathFactory] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このドキュメントでは、[!DNL PathFactory]API[[!DNL Flow Service]  を使用して &#x200B;](<https://www.adobe.io/experience-platform-apis/references/flow-service/>) のベース接続を作成する方法について説明します。

## 基本を学ぶ

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; ソース &#x200B;](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md): Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../../../landing/api-guide.md) を参照してください。

次の節では、[!DNL PathFactory] API を使用してに正常に接続するために必要な追加情報を示し [!DNL Flow Service] す。

### 必要な資格情報の収集 {#gather-credentials}

Experience Platformで PathFactory アカウントにアクセスするには、次の値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| ユーザー名 | [!DNL PathFactory] アカウントのユーザー名。 これは、システム内のアカウントを識別するために不可欠です。 |
| パスワード | [!DNL PathFactory] アカウントに関連付けられたパスワード。 権限のないアクセスを防ぐために、これが安全に保たれていることを確認します。 |
| ドメイン | [!DNL PathFactory] アカウントに関連付けられたドメイン。 これは通常、[!DNL PathFactory] URL 内の一意の ID を参照します。 |
| アクセストークン | システムと [!DNL PathFactory] ーザー間の安全な通信を確保するために、API 認証に使用される一意のトークン。 |
| API エンドポイント | データにアクセスするための特定の API エンドポイント：訪問者、セッションおよびページビュー。 各エンドポイントは、取得可能な様々なデータセットに対応します。 **メモ：** これらは [!DNL PathFactory] で事前定義され、アクセスする予定のデータに固有です。 <ul><li>**訪問者エンドポイント**:`/api/public/v3/data_lake_apis/visitors.json`</li><li>**セッション エンドポイント**: `/api/public/v3/data_lake_apis/sessions.json`</li><li>**ページビューエンドポイント**: `/api/public/v3/data_lake_apis/page_views.json`</li></ul> |

資格情報の保護および使用方法、アクセストークンの取得および更新方法について詳しくは、[[!DNL PathFactory]  サポートセンター &#x200B;](https://support.pathfactory.com/categories/adobe/) を参照してください。 このリソースでは、資格情報の管理と、効果的で安全な API 統合の確保に関する包括的なガイドを提供します。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に [!DNL PathFactory] 認証資格情報をリクエスト本文の一部として指定します。

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
| `auth.params.clientId` | [!DNL PathFactory] アプリケーションに関連付けられたクライアント ID。 |
| `auth.params.clientSecret` | [!DNL PathFactory] アプリケーションに関連付けられたクライアント秘密鍵。 |
| `connectionSpec.id` | [!DNL PathFactory] 接続仕様 ID: `ea1c2a08-b722-11eb-8529-0242ac130003`。 |

**応答**

応答が成功すると、一意の接続識別子（`id`）を含む、新しく作成された接続が返されます。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL PathFactory] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [&#x200B; [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [&#x200B; [!DNL Flow Service] API を使用した、マーケティング自動化データをExperience Platformに取り込むデータフローの作成](../../collect/marketing-automation.md)
