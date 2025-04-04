---
keywords: Experience Platform;ホーム;人気のトピック;oracle;
title: （ベータ版）Flow Service API を使用した Oracle Responsys ベース接続の作成
description: Flow Service API を使用して Adobe Experience Platform を Oracle Responsys に接続する方法について説明します。
hide: true
hidefromtoc: true
exl-id: 76659f5a-c923-488c-88f6-1919bc6a7bb5
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '547'
ht-degree: 76%

---

# （ベータ版）[!DNL Flow Service] API を使用した [!DNL Oracle Responsys] ベース接続の作成

>[!NOTE]
>
>[!DNL Oracle Responsys] ソースはベータ版です。ベータ版のコネクタの使用に関して詳しくは、[ ソースの概要 ](../../../../home.md#terms-and-conditions) を参照してください。

ベース接続は、ソースと Adobe Experience Platform 間の認証済み接続を表します。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、[!DNL Oracle Responsys] のベース接続を作成する手順を説明します。

## はじめに

このガイドは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../../../sandboxes/home.md):Experience Platformには、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して [!DNL Oracle Responsys] に正常に接続するために必要な追加情報を示しています。

### 必要な資格情報の収集

[!DNL Flow Service] を [!DNL Oracle Responsys] に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| `endpoint` | [!DNL Oracle Responsys] インスタンスの REST 認証エンドポイント URL。 |
| `clientId` | [!DNL Oracle Responsys] インスタンスのクライアント ID。 |
| `clientSecret` | [!DNL Oracle Responsys] インスタンスのクライアント秘密鍵。 |
| `connectionSpec.id` | 接続仕様は、ベース接続とソース接続の作成に関連する認証仕様などの、ソースのコネクタプロパティを返します。[!DNL Oracle Responsys] ソースの接続仕様 ID の値は、`ff4274f2-c9a9-11eb-b8bc-0242ac130003` に固定されています。 |

[!DNL Oracle Responsys] の認証資格情報について詳しくは、[[!DNL Oracle Responsys] 認証に関するガイド](https://docs.oracle.com/en/cloud/saas/marketing/responsys-develop/API/GetStarted/authentication.htm)を参照してください。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../../../landing/api-guide.md) を参照してください。

## ベース接続の作成

ベース接続は、ソースとExperience Platform間の情報（ソースの認証資格情報、現在の接続状況、一意のベース接続 ID など）を保持します。 ベース接続 ID により、ソース内からファイルを参照および移動し、データタイプやフォーマットに関する情報を含む、取り込みたい特定の項目を識別することができます。

ベース接続 ID を作成するには、`/connections` エンドポイントに POST リクエストを実行し、[!DNL Oracle Responsys] 認証資格情報をリクエストパラメーターの一部として使用します。

**API 形式**

```https
POST /connections
```

**リクエスト**

次のリクエストは、[!DNL Oracle Responsys] のベース接続を作成します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/connections' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json'
  -d '{
      "name": "Oracle Responsys Base Connection",
      "description": "Base Connection for Oracle Responsys",
      "auth": {
          "specName": "Basic Authentication",
          "params": {
              "endpoint": "{ENDPOINT}",
              "clientId": "{CLIENT_ID}",
              "clientSecret": "{CLIENT_SECRET}"
          }
      },
      "connectionSpec": {
          "id": "ff4274f2-c9a9-11eb-b8bc-0242ac130003",
          "version": "1.0"
      }
  }'
```

| パラメーター | 説明 |
| --- | --- |
| `name` | [!DNL Oracle Responsys] ベース接続名。この値を使用してベース接続を検索できるので、わかりやすい名前を指定することをお勧めします。 |
| `description` | （オプション）ベース接続に関する補足情報を提供するために含めることができるプロパティ。 |
| `auth.specName` | 接続に使用する認証タイプ。 |
| `auth.params.endpoint` | [!DNL Oracle Responsys] サーバーの REST 認証エンドポイント URL。 |
| `auth.params.clientId` | [!DNL Oracle Responsys] インスタンスのクライアント ID。 |
| `auth.params.clientSecret` | [!DNL Oracle Responsys] インスタンスのクライアント秘密鍵。 |
| `connectionSpec.id` | [!DNL Oracle Responsys] ソースの接続仕様 ID の値は、`ff4274f2-c9a9-11eb-b8bc-0242ac130003` に固定されています。 |

**応答**

リクエストが成功した場合は、一意の ID（`id`）を含め、新しく作成されたベース接続の詳細が返されます。この ID は、次の手順でソース接続を作成する際に必要になります。

```json
{
    "id": "2484f2df-c057-4ab5-84f2-dfc0577ab592",
    "etag": "\"10033e77-0000-0200-0000-5e96785b0000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して [!DNL Oracle Responsys] ベース接続を作成しました。このベース接続 ID は、次のチュートリアルで使用できます。

* [ [!DNL Flow Service]  API を使用したデータテーブルの構造と内容の探索](../../explore/tabular.md)
* [ [!DNL Flow Service] API を使用した、マーケティング自動化データをExperience Platformに取り込むデータフローの作成](../../collect/marketing-automation.md)
