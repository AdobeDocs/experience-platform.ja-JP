---
keywords: Experience Platform；ホーム；人気のあるトピック；servicenow;ServiceNow
solution: Experience Platform
title: フローサービスAPIを使用したServiceNowソース接続の作成
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してAdobe Experience PlatformをServiceNowサーバーに接続する方法を説明します。
exl-id: 39d0e628-5c07-4371-a5af-ac06385db891
source-git-commit: e150f05df2107d7b3a2e95a55dc4ad072294279e
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 36%

---

# [!DNL Flow Service] APIを使用して[!DNL ServiceNow]ソース接続を作成する

[!DNL Flow Service] は、Adobe Experience Platform内の様々な異なるソースから顧客データを収集し、一元化するために使用されます。このサービスは、サポートされているすべてのソースが接続可能なユーザーインターフェイスとRESTful APIを提供します。

このチュートリアルでは、[!DNL Flow Service] APIを使用して、[!DNL Experience Platform]を[!DNL ServiceNow]サーバーに接続する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する作業を理解している必要があります。

* [ソース](../../../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用して[!DNL ServiceNow]サーバーに正常に接続するために知っておく必要がある追加情報を示します。

### 必要な資格情報の収集

[!DNL Flow Service]が[!DNL ServiceNow]に接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| ---------- | ----------- |
| `endpoint` | [!DNL ServiceNow]サーバーのエンドポイント。 |
| `username` | [!DNL ServiceNow]サーバーに接続して認証を行う際に使用するユーザー名。 |
| `password` | [!DNL ServiceNow]サーバーに接続して認証を行うためのパスワード。 |

開始方法の詳細については、[このServiceNowのドキュメント](https://developer.servicenow.com/app.do#!/rest_api_doc?v=newyork&amp;id=r_TableAPI-GET)を参照してください。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 接続の作成

接続では、ソースを指定し、そのソースの資格情報を含めます。 異なるデータを取り込むために複数のソースコネクタを作成する場合に使用できるので、[!DNL ServiceNow]アカウントごとに1つの接続のみが必要です。

**API 形式**

```http
POST /connections
```

**リクエスト**

[!DNL ServiceNow]接続を作成するには、一意の接続仕様IDをPOSTリクエストの一部として指定する必要があります。 [!DNL ServiceNow]の接続仕様IDは`eb13cb25-47ab-407f-ba89-c0125281c563`です。

```shell
curl -X POST \
    'http://platform.adobe.io/data/foundation/flowservice/connections' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'Content-Type: application/json' \
    -d '{
        "name": "Connection for service-now",
        "description": "Connection for service-now,
        "auth": {
            "specName": "Basic Authentication",
            "params": {
                "endpoint": "{ENDPOINT}",
                "username": "{USERNAME}",
                "password": "{PASSWORD}"
            }
        },
        "connectionSpec": {
            "id": "eb13cb25-47ab-407f-ba89-c0125281c563",
            "version": "1.0"
        }
    }'
```

| プロパティ | 説明 |
| ------------- | --------------- |
| `auth.params.server` | [!DNL ServiceNow]サーバーのエンドポイント。 |
| `auth.params.username` | [!DNL ServiceNow]サーバーに接続して認証を行う際に使用するユーザー名。 |
| `auth.params.password` | [!DNL ServiceNow]サーバーに接続して認証を行うためのパスワード。 |
| `connectionSpec.id` | [!DNL ServiceNow]に関連付けられている接続仕様ID。 |

**応答** 

正常な応答は、新しく作成された接続を、一意の識別子(`id`)を含めて返します。 このIDは、次の手順でCRMシステムを調べるために必要です。

```json
{
    "id": "8a3ca3dd-6d00-4c95-bca3-dd6d00dc954b",
    "etag": "\"8e0052a2-0000-0200-0000-5e25fb330000\""
}
```

## 次の手順

このチュートリアルでは、[!DNL Flow Service] APIを使用して[!DNL ServiceNow]接続を作成し、接続の一意のID値を取得しました。 次のチュートリアルでは、フローサービスAPI](../../explore/customer-success.md)を使用して顧客の成功システムを調べる方法を学ぶ際に、この接続IDを使用できます。[
