---
keywords: Experience Platform；ホーム；人気のあるトピック；汎用OData；汎用データ
solution: Experience Platform
title: フローサービスAPIを使用した汎用ODataベース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用して汎用ODataをAdobe Experience Platformに接続する方法を説明します。
exl-id: 45b302cb-1a43-4fab-a8a2-cb4e1ee129f9
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '443'
ht-degree: 11%

---

# [!DNL Flow Service] APIを使用して[!DNL Generic OData]ベース接続を作成する

>[!NOTE]
>
>[!DNL Generic OData]コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ソースの概要](../../../../home.md#terms-and-conditions)」を参照してください。

ベース接続は、ソースとAdobe Experience Platform間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して[!DNL Generic OData]の基本接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL Generic OData]に正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL Generic OData]と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `url` | [!DNL Generic OData]サービスのルートURL。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様を含む、ソースのコネクタプロパティを返します。 [!DNL Generic Generic OData]の接続仕様IDは次のとおりです。`8e6b41a8-d998-4545-ad7d-c6a9fff406c3`. |

使い始める方法について詳しくは、[this [!DNL Generic OData] document](https://www.odata.org/getting-started/basic-tutorial/)を参照してください。

### Platform APIの使用

Platform APIを正常に呼び出す方法について詳しくは、[Platform APIの使用の手引き](../../../../../landing/api-guide.md)を参照してください。

## ベース接続を作成する

ベース接続は、ソースとプラットフォームの間の情報（ソースの認証資格情報、接続の現在の状態、一意のベース接続IDなど）を保持します。 ベース接続IDを使用すると、ソース内からファイルを参照およびナビゲートし、取得する特定の項目（データのタイプや形式に関する情報を含む）を特定できます。

ベースPOSTIDを作成するには、リクエストパラメーターの一部として[!DNL Generic OData]認証資格情報を指定しながら、`/connections`エンドポイントに接続リクエストを実行します。

**API 形式**

```http
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Generic OData]のベース接続を作成します。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Protocols",
        "description": "A test connection for a Protocols source",
        "auth": {
            "specName": "Anonymous Authentication",
        "params": {
            "url" :  "{URL}"
            }
        },
        "connectionSpec": {
            "id": "8e6b41a8-d998-4545-ad7d-c6a9fff406c3",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| --------- | ----------- |
| `auth.params.url` | [!DNL Generic OData]サーバのホスト。 |
| `connectionSpec.id` | [!DNL Generic OData]接続仕様ID:`8e6b41a8-d998-4545-ad7d-c6a9fff406c3`. |

**応答**

正常な応答は、新しく作成された接続を返します。この接続には、一意の接続識別子(`id`)が含まれます。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "a5c6b647-e784-4b58-86b6-47e784ab580b",
    "etag": "\"7b01056a-0000-0200-0000-5e8a4f5b0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL OData]接続を作成し、接続の一意のID値を取得しました。 このIDは、次のチュートリアルで、フローサービスAPI](../../explore/protocols.md)を使用してプロトコルアプリケーションを調べる方法を学ぶ際に使用できます。[
