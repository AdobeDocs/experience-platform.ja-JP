---
keywords: Experience Platform；ホーム；人気のトピック；Phoenix；フェニックス
solution: Experience Platform
title: フローサービス API を使用した Phoenix ベース接続の作成
type: Tutorial
description: フローサービス API を使用して Phoenix データベースをAdobe Experience Platformに接続する方法を説明します。
exl-id: b69d9593-06fe-4fff-88a9-7860e4e45eb7
source-git-commit: 90eb6256179109ef7c445e2a5a8c159fb6cbfe28
workflow-type: tm+mt
source-wordcount: '568'
ht-degree: 49%

---

# [!DNL Flow Service] API を使用した [!DNL Phoenix] ベース接続の作成

>[!NOTE]
>
>この [!DNL Phoenix] コネクタはベータ版です。 ベータ版のコネクタの使用に関して詳しくは、[ソースの概要](../../../../home.md#terms-and-conditions)を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。 このサービスは、ユーザーインターフェイスと RESTful API を提供し、サポートされるすべてのソースから接続できます。

このチュートリアルでは、 [!DNL Flow Service] 接続の手順を説明する API [!DNL Phoenix] データベースへ [!DNL Experience Platform].

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、に正常に接続するために知っておく必要がある追加情報を示します。 [!DNL Phoenix] の使用 [!DNL Flow Service] API

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Phoenix] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | の IP アドレスまたはホスト名 [!DNL Phoenix] サーバー。 |
| `username` | アクセスに使用するユーザー名 [!DNL Phoenix] サーバー。 |
| `password` | ユーザーに対応するパスワード。 |
| `port` | TCP ポート [!DNL Phoenix] サーバーは、を使用してクライアント接続をリッスンします。 次に接続する場合： [!DNL Azure] HDInsights、ポートを 443 に指定します。 |
| `httpPath` | URL の一部 [!DNL Phoenix] サーバー。 使用する場合は/hbasephoenix0 を指定します。 [!DNL Azure] HDInsights クラスター。 |
| `enableSsl` | ブール値。 サーバーへの接続が SSL を使用して暗号化されるかどうかを指定します。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。の接続仕様 ID [!DNL Phoenix] 次に該当： `102706fb-a5cd-42ee-afe0-bc42f017ff43` |

の導入について詳しくは、 [この Phoenix ドキュメント](https://python-phoenixdb.readthedocs.io/en/latest/api.html).

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、[Platform API の概要](../../../../../landing/api-guide.md)を参照してください。

## ベース接続の作成

ベース接続は、ソースと Platform 間の情報（ソースの認証資格情報、現在の接続状態、固有のベース接続 ID など）を保持します。ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Phoenix] 認証資格情報をリクエストパラメーターの一部として使用します。

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
| `auth.params.username` | ユーザー名 [!DNL Phoenix] 接続。 |
| `auth.params.password` | ユーザーに関連付けられたパスワード [!DNL Phoenix] 接続。 |
| `auth.params.port` | TCP ポート [!DNL Phoenix] 接続。 |
| `auth.params.httpPath` | の部分的な HTTP パス [!DNL Phoenix] 接続。 |
| `auth.params.enableSsl` | サーバーへの接続が SSL を使用して暗号化されるかどうかを指定する boolean 値です。 |
| `connectionSpec.id` | この [!DNL Phoenix] 接続仕様 ID: `102706fb-a5cd-42ee-afe0-bc42f017ff43`. |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含む、新しく作成した接続の詳細が返されます。この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "0d982fff-c443-403e-982f-ffc443f03e37",
    "etag": "\"830082dc-0000-0200-0000-5e84ee560000\""
}
```

## 次の手順

このチュートリアルに従って、 [!DNL Phoenix] を使用したベース接続 [!DNL Flow Service] API このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [データフローを作成し、 [!DNL Flow Service] API](../../collect/database-nosql.md)
