---
keywords: Experience Platform；ホーム；人気のあるトピック；Oracle;oracle
solution: Experience Platform
title: フローサービスAPIを使用したOracleベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してOracleをExperience Platformに接続する方法を説明します。
exl-id: b1cea714-93ff-425f-8e12-6061da97d094
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 10%

---

# [!DNL Flow Service] APIを使用して[!DNL Oracle]ベース接続を作成する

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Oracle]の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL Oracle]に正常に接続するために知っておく必要がある追加情報を示します。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | [!DNL Oracle]への接続に使用する接続文字列。 [!DNL Oracle]接続文字列パターンは次のとおりです。`Host={HOST};Port={PORT};Sid={SID};User Id={USERNAME};Password={PASSWORD}`. |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Oracle]の接続仕様IDは`d6b52d86-f0f8-475f-89d4-ce54c8527328`です。 |

使い始める方法について詳しくは、この[[!DNL Oracle] ドキュメント](https://docs.oracle.com/database/121/ODPNT/featConnecting.htm#ODPNT199)を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL Oracle]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Oracle]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Oracle connection",
        "description": "A connection for Oracle",
        "auth": {
            "specName": "ConnectionString",
            "params": {
                    "connectionString": "Host={HOST};Port={PORT};Sid={SID};UserId={USERNAME};Password={PASSWORD}"
                }
        },
        "connectionSpec": {
            "id": "d6b52d86-f0f8-475f-89d4-ce54c8527328",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | [!DNL Oracle]データベースへの接続に使用する接続文字列。 [!DNL Oracle]接続文字列パターンは次のとおりです。`Host={HOST};Port={PORT};Sid={SID};User Id={USERNAME};Password={PASSWORD}`. |
| `connectionSpec.id` | [!DNL Oracle]接続仕様ID:`d6b52d86-f0f8-475f-89d4-ce54c8527328`. |

**応答**

正常な応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "f088e4f2-2464-480c-88e4-f22464b80c90",
    "etag": "\"43011faa-0000-0200-0000-5ea740cd0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL Oracle]接続を作成し、接続の一意のID値を取得しました。 このIDは、次のチュートリアルでフローサービスAPI](../../explore/database-nosql.md)を使用してデータベースを調べる方法を学ぶ際に使用できます。[
