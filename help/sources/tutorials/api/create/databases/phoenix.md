---
keywords: Experience Platform；ホーム；人気のあるトピック；フェニックス；フェニックス
solution: Experience Platform
title: フローサービスAPIを使用したPhoenixベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してPhoenixデータベースをAdobe Experience Platformに接続する方法を説明します。
exl-id: b69d9593-06fe-4fff-88a9-7860e4e45eb7
source-git-commit: 5fb5f0ce8bd03ba037c6901305ba17f8939eb9ce
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 8%

---

# [!DNL Flow Service] APIを使用して[!DNL Phoenix]ベース接続を作成する

>[!NOTE]
>
>[!DNL Phoenix]コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ソースの概要](../../../../home.md#terms-and-conditions)」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、サポートされているすべてのソースが接続可能なユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、[!DNL Phoenix]データベースを[!DNL Experience Platform]に接続する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL Phoenix]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Phoenix]と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL Phoenix]サーバーのIPアドレスまたはホスト名。 |
| `username` | [!DNL Phoenix]サーバーにアクセスするために使用するユーザー名。 |
| `password` | ユーザーに対応するパスワード。 |
| `port` | [!DNL Phoenix]サーバーがクライアント接続をリッスンするTCPポート。 [!DNL Azure] HDInsightsに接続する場合は、ポートを443に指定します。 |
| `httpPath` | [!DNL Phoenix]サーバーに対応するURLの一部。 [!DNL Azure] HDInsightsクラスターを使用する場合は、/hbasephoenix0を指定します。 |
| `enableSsl` | boolean値。 サーバーへの接続をSSLを使用して暗号化するかどうかを指定します。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Phoenix]の接続仕様IDは次のとおりです。`102706fb-a5cd-42ee-afe0-bc42f017ff43` |

開始方法の詳細は、[このPhoenixのドキュメント](https://python-phoenixdb.readthedocs.io/en/latest/api.html)を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL Phoenix]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Phoenix]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Phoenix test connection",
        "description": "Phoenix test connection",
        "auth": {
            "specName": "Basic Authentication",
        "params": {
            "host" :  "{HOST}",
            "username" : "{USERNAME}",
            "password" :"{PASSWORD}",
            "port" : {PORT},
            "httpPath" : "{PATH}",
            "enableSsl" : {SSL}
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
| `auth.params.host` | [!DNL Phoenix]サーバのホスト。 |
| `auth.params.username` | [!DNL Phoenix]接続に関連付けられたユーザー名。 |
| `auth.params.password` | [!DNL Phoenix]接続に関連付けられたパスワード。 |
| `auth.params.port` | [!DNL Phoenix]接続用のTCPポート。 |
| `auth.params.httpPath` | [!DNL Phoenix]接続のHTTPパスの一部。 |
| `auth.params.enableSsl` | サーバーへの接続がSSLを使用して暗号化されるかどうかを指定するboolean値です。 |
| `connectionSpec.id` | [!DNL Phoenix]接続仕様ID:`102706fb-a5cd-42ee-afe0-bc42f017ff43`. |

**応答**

正常な応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "0d982fff-c443-403e-982f-ffc443f03e37",
    "etag": "\"830082dc-0000-0200-0000-5e84ee560000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Phoenix]接続を作成し、接続の一意のID値を取得しました。 このIDは、次のチュートリアルでフローサービスAPI](../../explore/database-nosql.md)を使用してデータベースを調べる方法を学ぶ際に使用できます。[
