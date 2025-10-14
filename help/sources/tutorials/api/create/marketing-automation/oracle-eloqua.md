---
title: Flow Service API を使用したOracle Eloqua ベース接続の作成
description: Flow Service API を使用してAdobe Experience PlatformをOracle Eloqua に接続する方法を説明します。
exl-id: 866e408f-6e0b-4e81-9ad8-9d74c485c89a
source-git-commit: 7ff0709b62590bb80c1ed664368f28cdc4a950ea
workflow-type: tm+mt
source-wordcount: '602'
ht-degree: 55%

---

# [!DNL Flow Service] API を使用した [!DNL Oracle Eloqua] ベース接続の作成

>[!WARNING]
>
>[!DNL Oracle Eloqua] ソースは 2026 年 1 月に非推奨（廃止予定）になります。 新しいソースは、代替手段として今年後半にリリースされる予定です。 新しいソースがリリースされたら、2026 年 1 月末までに、新しいアカウント接続とデータフローを作成して、新しいソースに移行する計画を立てる必要があります。

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Oracle Eloqua] のベース接続を作成する手順を説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [&#x200B; ソース &#x200B;](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [&#x200B; サンドボックス &#x200B;](../../../../../sandboxes/home.md):Experience Platformには、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して [!DNL Oracle Eloqua] に正常に接続するために必要な追加情報を示しています。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Oracle Eloqua] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `endpoint` | [!DNL Oracle Eloqua] のエンドポイント。 |
| `username` | [!DNL Oracle Eloqua] アカウントのユーザー名。 ユーザー名は `siteName + \\ + username` の形式にする必要があります。`siteName` は、[!DNL Oracle Eloqua] へのログインに使用した会社名で、`username` はユーザー名です。 例えば、ログインのユーザー名は `adobe\\emily` のようになります。 |
| `password` | [!DNL Oracle Eloqua] ユーザー名に対応するパスワード。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Oracle Eloqua] ソースの接続仕様 ID の値は、`35d6c4d8-c9a9-11eb-b8bc-0242ac130003` に固定されています。 |

[!DNL Oracle Eloqua] の認証資格情報について詳しくは、[[!DNL Oracle Eloqua] 認証に関するガイド](https://docs.oracle.com/en/cloud/saas/marketing/eloqua-rest-api/Authentication_Basic.html)を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Oracle Eloqua] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Oracle Eloqua] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json'
  -d '{
      "name": "Oracle Eloqua Base Connection",
      "description": "Base Connection for Oracle Eloqua",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "endpoint": "{ENDPOINT}",
              "username": "{USERNAME}",
              "password": "{PASSWORD}"
          }
      },
      "connectionSpec": {
          "id": "35d6c4d8-c9a9-11eb-b8bc-0242ac130003",
          "version": "1.0"
      }
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `name` | [!DNL Oracle Eloqua] ベース接続名。この値を使用してベース接続を検索できるので、わかりやすい名前を指定することをお勧めします。 |
| `description` | （オプション）ベース接続に関する補足情報を提供するために含めることができるプロパティ。 |
| `auth.specName` | 接続に使用する認証タイプ。 |
| `auth.params.endpoint` | [!DNL Oracle Eloqua] サーバーのエンドポイント。 |
| `auth.params.username` | [!DNL Oracle Eloqua] アカウントに対応するサイト名とユーザー名を含む、連結された資格情報。 |
| `auth.params.password` | [!DNL Oracle Eloqua] アカウントに対応するパスワード。 |
| `connectionSpec.id` | [!DNL Oracle Eloqua] ソースの接続仕様 ID の値は、`35d6c4d8-c9a9-11eb-b8bc-0242ac130003` に固定されています。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Oracle Eloqua] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [&#x200B; [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [&#x200B; [!DNL Flow Service] API を使用した、マーケティング自動化データをExperience Platformに取り込むデータフローの作成](../../collect/marketing-automation.md)
