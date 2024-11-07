---
title: Flow Service API を使用した Phoenix ベース接続の作成
description: Flow Service API を使用して Phoenix データベースをAdobe Experience Platformに接続する方法を説明します。
exl-id: b69d9593-06fe-4fff-88a9-7860e4e45eb7
source-git-commit: a32d0d7ed7d18454099d2b55b3f6809cfbcd9b62
workflow-type: tm+mt
source-wordcount: '558'
ht-degree: 38%

---

# [!DNL Flow Service] API を使用した [!DNL Phoenix] ベース接続の作成

>[!IMPORTANT]
>
>[!DNL Phoenix] ソースは 2025 年 5 月末に非推奨（廃止予定）になります。

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[!DNL Flow Service] API を使用してベース接続を作成し、[!DNL Phoenix] アカウントをAdobe Experience Platformに接続する手順について説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、Experience Platformサービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md):Experience Platformには、1 つのExperience Platformインスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示 [!DNL Phoenix] ています。

### 必要な資格情報の収集

[!DNL Phoenix] アカウントをExperience Platformに接続するには、次の認証資格情報を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Phoenix] サーバーの IP アドレスまたはホスト名。 |
| `username` | [!DNL Phoenix] Server へのアクセスに使用するユーザー名。 |
| `password` | ユーザーに対応するパスワード。 |
| `port` | クライアント接続をリッスンするために [!DNL Phoenix] サーバーが使用する TCP ポート。 [!DNL Azure HDInsights] に接続する場合は、ポートを 443 に指定します。 このパラメーターを指定しない場合、値はデフォルトで 8765 になります。 |
| `httpPath` | [!DNL Phoenix] サーバーに対応する部分的な URL。 HDInsights クラスターを使用する場合は、/hbasephoenix0[!DNL Azure] 指定します。 |
| `enableSsl` | ブール値。 サーバーへの接続を SSL を使用して暗号化するかどうかを指定します。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Phoenix] の接続仕様 ID は `102706fb-a5cd-42ee-afe0-bc42f017ff43` です。 |

基本について詳しくは、[ この Phoenix のドキュメント ](https://python-phoenixdb.readthedocs.io/en/latest/api.html) を参照してください。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベースPOSTを作成するには、`/connections` エンドポイントに接続リクエストを実行し、その際にリクエスト本文で [!DNL Phoenix] 認証資格情報を指定します。

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
| `auth.params.host` | [!DNL Phoenix] サーバーのホスト。 |
| `auth.params.username` | [!DNL Phoenix] 接続に関連付けられたユーザー名。 |
| `auth.params.password` | [!DNL Phoenix] 接続に関連付けられたパスワード。 |
| `auth.params.port` | [!DNL Phoenix] 接続の TCP ポート。 |
| `auth.params.httpPath` | [!DNL Phoenix] 接続の部分的な http パス。 |
| `auth.params.enableSsl` | サーバーへの接続を SSL を使用して暗号化するかどうかを指定するブール値。 |
| `connectionSpec.id` | [!DNL Phoenix] 接続仕様 ID: `102706fb-a5cd-42ee-afe0-bc42f017ff43`。 |

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
* [ [!DNL Flow Service] API を使用した、データベースデータを Platform に取り込むデータフローの作成](../../collect/database-nosql.md)
