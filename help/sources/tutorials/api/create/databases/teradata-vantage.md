---
keywords: Experience Platform；ホーム；人気のトピック；Teradata Vantage
title: Flow Service API を使用したTeradata Vantage ベース接続の作成
description: Flow Service API を使用してAdobe Experience PlatformをTeradata Vantage に接続する方法を説明します。
exl-id: 88707dca-3c7a-43c7-9d71-473ad9715fc6
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '455'
ht-degree: 46%

---

# [!DNL Flow Service] API を使用した [!DNL Teradata Vantage] ベース接続の作成

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Teradata Vantage] のベース接続を作成する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 &#x200B;](../../../../../landing/api-guide.md) を参照してください。

次の節では、[!DNL Flow Service] API を使用してに正常に接続するために必要な追加情報を示し [!DNL Teradata Vantage] す。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Teradata Vantage] に接続するには、次の接続プロパティを指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `connectionString` | 接続文字列は、データ ソースとその接続方法に関する情報を提供する文字列です。 [!DNL Teradata Vantage] の接続文字列のパターンは `DBCName={SERVER};Uid={USERNAME};Pwd={PASSWORD}` です。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Teradata Vantage] の接続仕様 ID は `2fa8af9c-2d1a-43ea-a253-f00a00c74412` です。 |

基本について詳しくは、この [[!DNL Teradata Vantage]  ドキュメント &#x200B;](https://docs.teradata.com/r/Teradata-VantageTM-Advanced-SQL-Engine-Security-Administration/July-2021/Setting-Up-the-Administrative-Infrastructure/Controlling-Access-to-the-Operating-System/Working-with-OS-Level-Security-Options) を参照してください。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに対して POST リクエストを実行し、その際に [!DNL Teradata Vantage] 認証資格情報をリクエスト本文の一部として指定します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Teradata Vantage] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json' \
  -d '{
      "name": "Teradata Vantage base connection",
      "description": "Teradata Vantage base connection",
      "auth": {
          "specName": "ConnectionString,
          "params": {
              "connectionString": "DBCName={SERVER};Uid={USERNAME};Pwd={PASSWORD}"
          }
      },
      "connectionSpec": {
          "id": "2fa8af9c-2d1a-43ea-a253-f00a00c74412",
          "version": "1.0"
      }
  }'
```

| プロパティ | 説明 |
| -------- | ----------- |
| `auth.params.connectionString` | [!DNL Teradata Vantage] インスタンスへの接続に使用する接続文字列。 [!DNL Teradata Vantage] の接続文字列のパターンは `DBCName={SERVER};Uid={USERNAME};Pwd={PASSWORD}` です。 |
| `connectionSpec.id` | [!DNL Teradata Vantage] 接続仕様 ID: `2fa8af9c-2d1a-43ea-a253-f00a00c74412`。 |

**応答**

応答が成功すると、一意の接続識別子（`id`）を含む、新しく作成された接続が返されます。 この ID は、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "2fce94c1-9a93-4971-8e94-c19a93097129",
    "etag": "\"d403848a-0000-0200-0000-5e978f7b0000\""
}
```

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Teradata Vantage] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [&#x200B; [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [&#x200B; [!DNL Flow Service] API を使用した、データベースデータをExperience Platformに取り込むデータフローの作成](../../collect/database-nosql.md)
