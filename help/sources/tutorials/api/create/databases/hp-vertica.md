---
keywords: Experience Platform；ホーム；人気のあるトピック；Vertica;vertica
solution: Experience Platform
title: フローサービスAPIを使用したHP Verticaソース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してHP VerticaをAdobe Experience Platformに接続する方法を説明します。
exl-id: 37f831c1-7c82-462a-8338-a0bcaaf08cd1
source-git-commit: c3d66e50f647c2203fcdd5ad36ad86ed223733e3
workflow-type: tm+mt
source-wordcount: '593'
ht-degree: 32%

---

# [!DNL Flow Service] APIを使用してHP [!DNL Vertica]ソース接続を作成する

>[!NOTE]
>
>HP [!DNL Vertica]コネクタはベータ版です。 ベータラベルのコネクタの使用について詳しくは、「[ソースの概要](../../../../home.md#terms-and-conditions)」を参照してください。

[!DNL Flow Service] は、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、サポートされているすべてのソースが接続可能なユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、HP [!DNL Vertica]を[!DNL Experience Platform]に接続する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](https://experienceleague.adobe.com/docs/experience-platform/source-connectors/home.html): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、マッピング、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](https://experienceleague.adobe.com/docs/experience-platform/sandbox/home.html)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用してHP [!DNL Vertica]に正しく接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]がHP [!DNL Vertica]と接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `connectionString` | HP [!DNL Vertica]インスタンスへの接続に使用する接続文字列。 HP [!DNL Vertica]の接続文字列パターンは`Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`です |
| `connectionSpec.id` | 接続の作成に必要な識別子。 HP [!DNL Vertica]の固定接続仕様IDは次のとおりです。`a8b6a1a4-5735-42b4-952c-85dce0ac38b5` |

接続文字列の取得に関する詳細は、[このHP Verticaドキュメント](https://www.vertica.com/docs/9.2.x/HTML/Content/Authoring/ConnectingToVertica/ClientJDBC/CreatingAndConfiguringAConnection.htm)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform]内のすべてのリソースは、特定の仮想サンドボックスに分離されます。 [!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続では、ソースを指定し、そのソースの資格情報を含めます。 HP [!DNL Vertica]アカウントごとに1つの接続のみが必要です。異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できます。

**API 形式**

```http
POST /connections
```

**リクエスト**

HP [!DNL Vertica]接続を作成するには、一意の接続仕様IDをPOST要求の一部として指定する必要があります。 HP [!DNL Vertica]の接続仕様IDは`a8b6a1a4-5735-42b4-952c-85dce0ac38b5`です。

```shell
curl -X POST \
    'https://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Connection for HP Vertica",
        "description": "Connection for HP Vertica",
        "auth": {
            "specName": "Connection String Based Authentication",
            "params": {
                "connectionString": "Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "a8b6a1a4-5735-42b4-952c-85dce0ac38b5",
            "version": "1.0"
        }
    }'
```

| パラメーター | 説明 |
| --------- | ----------- |
| `auth.params.connectionString` | HP [!DNL Vertica]アカウントに関連付けられた接続文字列。 HP [!DNL Vertica]の接続文字列パターンは次のとおりです。`Server={SERVER};Port={PORT};Database={DATABASE};UID={USERNAME};PWD={PASSWORD}`. |
| `connectionSpec.id` | HP [!DNL Vertica]接続仕様ID:`a8b6a1a4-5735-42b4-952c-85dce0ac38b5`. |

**応答** 

正常な応答は、新しく作成された接続の詳細(一意の識別子(`id`)を含む)を返します。 このIDは、次のチュートリアルでデータを調べるために必要です。

```json
{
    "id": "6bc13a3b-3546-455f-813a-3b3546a55fb1",
    "etag": "\"3500866c-0000-0200-0000-5e83afa30000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用してHP [!DNL Vertica]接続を作成し、接続の一意のID値を取得しました。 このIDは、次のチュートリアルでフローサービスAPI](../../explore/database-nosql.md)を使用してデータベースを調べる方法を学ぶ際に使用できます。[
