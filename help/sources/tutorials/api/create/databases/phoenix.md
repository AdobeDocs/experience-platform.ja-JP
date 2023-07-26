---
title: フローサービス API を使用した Phoenix ベース接続の作成
description: フローサービス API を使用して Phoenix データベースをAdobe Experience Platformに接続する方法を説明します。
exl-id: b69d9593-06fe-4fff-88a9-7860e4e45eb7
source-git-commit: efffd6ce1ed541ce20ee6500e42165465f2fa6a0
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 38%

---

# [!DNL Flow Service] API を使用した [!DNL Phoenix] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、ベース接続を作成し、 [!DNL Phoenix] を使用してAdobe Experience Platformにアカウント [!DNL Flow Service] API.

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Experience Platformサービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md):Experience Platformは、単一のExperience Platformインスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL Phoenix] の使用 [!DNL Flow Service] API.

### 必要な資格情報の収集

次の認証資格情報を入力して、 [!DNL Phoenix] アカウントからExperience Platformへ。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | の IP アドレスまたはホスト名 [!DNL Phoenix] サーバー。 |
| `username` | アクセスに使用するユーザー名 [!DNL Phoenix] サーバー。 |
| `password` | ユーザーに対応するパスワード。 |
| `port` | TCP ポート [!DNL Phoenix] サーバーは、を使用してクライアント接続をリッスンします。 次に接続する場合： [!DNL Azure HDInsights]で、ポートを 443 に指定します。 このパラメーターを指定しない場合、値のデフォルトは 8765 です。 |
| `httpPath` | URL の一部 [!DNL Phoenix] サーバー。 使用する場合は/hbasephoenix0 を指定します。 [!DNL Azure] HDInsights クラスター。 |
| `enableSsl` | ブール値。 サーバーへの接続が SSL を使用して暗号化されるかどうかを指定します。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。の接続仕様 ID [!DNL Phoenix] 次に該当： `102706fb-a5cd-42ee-afe0-bc42f017ff43` |

の導入について詳しくは、 [この Phoenix ドキュメント](https://python-phoenixdb.readthedocs.io/en/latest/api.html).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続を作成するには、 `/connections` エンドポイントを [!DNL Phoenix] 認証資格情報（リクエスト本文）。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Phoenix] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Phoenix test connection",
      "description": "Phoenix test connection",
      "auth": {
          "specName": "Basic Authentication",
      "params": {
          "host":  "{HOST}",
          "username": "{USERNAME}",
          "password":"{PASSWORD}",
          "port": {PORT},
          "httpPath": "{PATH}",
          "enableSsl": {SSL}
          }
      },
      "connectionSpec": {
          "id": "102706fb-a5cd-42ee-afe0-bc42f017ff43",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.host` | のホスト [!DNL Phoenix] サーバー。 |
| `auth.params.username` | に関連付けられたユーザー名 [!DNL Phoenix] 接続。 |
| `auth.params.password` | ユーザーに関連付けられたパスワード [!DNL Phoenix] 接続。 |
| `auth.params.port` | TCP ポート ( [!DNL Phoenix] 接続。 |
| `auth.params.httpPath` | の部分的な HTTP パス [!DNL Phoenix] 接続。 |
| `auth.params.enableSsl` | サーバーへの接続が SSL を使用して暗号化されるかどうかを指定する boolean 値です。 |
| `connectionSpec.id` | The [!DNL Phoenix] 接続仕様 ID: `102706fb-a5cd-42ee-afe0-bc42f017ff43`. |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "0d982fff-c443-403e-982f-ffc443f03e37",
    "etag": "\"830082dc-0000-0200-0000-5e84ee560000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Phoenix] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/database-nosql.md)
