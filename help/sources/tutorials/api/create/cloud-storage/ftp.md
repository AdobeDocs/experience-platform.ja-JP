---
keywords: Experience Platform；ホーム；人気のあるトピック；ファイル転送プロトコル；ファイル転送プロトコル
solution: Experience Platform
title: フローサービス API を使用した FTP ベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してAdobe Experience Platformを FTP（ファイル転送プロトコル）サーバーに接続する方法を説明します。
exl-id: a7bef346-b357-49bc-ac54-ac8b42adac50
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 12%

---

# [!DNL Flow Service] API を使用した FTP ベース接続の作成

>[!NOTE]
>
>FTP コネクタはベータ版です。 機能とドキュメントは変更される場合があります。ベータラベルのコネクタの使用について詳しくは、「[ ソースの概要 ](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して [!DNL FTP]（ファイル転送プロトコル）の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して、受信データの構造化、ラベル付け、強化をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] API を使用して [!DNL FTP] サーバーに正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service] が [!DNL FTP] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `host` | [!DNL FTP] サーバーに関連付けられている名前または IP アドレス。 |
| `username` | [!DNL FTP] サーバーへのアクセス権を持つユーザー名。 |
| `password` | [!DNL FTP] サーバーのパスワード。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL FTP] の接続仕様 ID は次のとおりです。`fb2e94c9-c031-467d-8103-6bd6e0a432f2`. |

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の使用の手引き ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続を作成する

ベース接続は、ソースと Platform の間の情報を保持します。これには、ソースの認証資格情報、接続の現在の状態、一意のベース接続 ID などが含まれます。 ベース接続 ID を使用すると、ソース内からファイルを参照および移動し、取り込む特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTID を作成するには、要求パラメーターの一部として [!DNL FTP] 認証資格情報を指定しながら、`/connections` エンドポイントに接続要求を行います。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL FTP] のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d  '{
        "name": "FTP connector with password",
        "description": "FTP connector password",
        "auth": {
            "specName": "Basic Authentication for FTP",
            "params": {
                "host": "{HOST}",
                "userName": "{USERNAME}",
                "password": "{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "fb2e94c9-c031-467d-8103-6bd6e0a432f2",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.host` | FTP サーバーのホスト名。 |
| `auth.params.username` | FTP サーバーに関連付けられているユーザー名。 |
| `auth.params.password` | FTP サーバーに関連付けられているパスワード。 |
| `connectionSpec.id` | FTP サーバー接続仕様 ID:`fb2e94c9-c031-467d-8103-6bd6e0a432f2` |

**応答**

正常な応答は、新しく作成された接続の一意の識別子 (`id`) を返します。 この ID は、次のチュートリアルで FTP サーバーを調べるために必要です。

```json
{
    "id": "bf367b0d-3d9b-4060-b67b-0d3d9bd06094",
    "etag": "\"1700cc7b-0000-0200-0000-5e3b3fba0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して FTP 接続を作成し、接続の一意の ID 値を取得しました。 この接続 ID を使用して [ フローサービス API](../../explore/cloud-storage.md) または [ フローサービス API](../../cloud-storage-parquet.md) を使用して Parquet データを取り込み、クラウドストレージを調べることができます。
