---
keywords: Experience Platform；ホーム；人気のトピック；フローサービス；宛先アカウントの削除；削除；api
solution: Experience Platform
title: Flow Service API を使用した宛先アカウントの削除
type: Tutorial
description: Flow Service API を使用して宛先アカウントを削除する方法を説明します。
exl-id: a963073c-ecba-486b-a5c2-b85bdd426e72
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 38%

---

# Flow Service API を使用した宛先アカウントの削除

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

データをアクティブ化する前に、まず宛先アカウントを設定して宛先に接続する必要があります。 このチュートリアルでは、不要になった宛先アカウントを [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して削除する手順を説明します。

>[!NOTE]
>
>宛先アカウントの削除は、現在、Flow Service API でのみサポートされています。 宛先アカウントは、Experience Platform UI を使用して削除することはできません。

## はじめに {#get-started}

このチュートリアルでは、有効な接続 ID が必要です。接続 ID は、宛先へのアカウント接続を表します。 有効な接続 ID がない場合は、このチュートリアルを試す前に、[ 宛先カタログ ](../catalog/overview.md) から宛先を選択し、[ 宛先に接続 ](../ui/connect-destination.md) の説明に従ってください。

このチュートリアルでは、Adobe Experience Platform の次のコンポーネントについて十分に理解していることを前提にしています。

* [ 宛先 ](../home.md):[!DNL Destinations] は、Adobe Experience Platformからのデータの円滑なアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。
* [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Experience Platform] インスタンスを別個の仮想環境に分割してデジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用して宛先アカウントを正常に削除するために必要な追加情報を示しています。

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集 {#gather-values-for-required-headers}

[!DNL Experience Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Experience Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>`x-sandbox-name` ヘッダーが指定されていない場合、リクエストは `prod` サンドボックスで解決されます。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 削除する宛先アカウントの接続 ID を見つけます {#find-connection-id}

>[!NOTE]
>このチュートリアルでは [Airship の宛先 ](../catalog/mobile-engagement/airship-attributes.md) を例として使用しますが、説明する手順は [ 使用可能な宛先 ](../catalog/overview.md) のいずれにも該当します。

宛先アカウントを削除する最初の手順は、削除する宛先アカウントに対応する接続 ID を見つけることです。

Experience Platform UI で、**[!UICONTROL 宛先]**/**[!UICONTROL アカウント]** を参照し、削除するアカウントを **[!UICONTROL 宛先]** 列の数値を選択して選択します。

![ 削除する宛先アカウントを選択 ](/help/destinations/assets/api/delete-destination-account/select-destination-account.png)

次に、ブラウザーの URL から宛先アカウントの接続 ID を取得できます。

![URL から接続 ID を取得 ](/help/destinations/assets/api/delete-destination-account/find-connection-id.png)

<!--

## Look up connection ID {#look-up-connection-id}

The first step in updating your connection information is to retrieve connection details using your connection ID.

**API format**

```http
GET /connections/{CONNECTION_ID}
```

| Parameter | Description |
| --------- | ----------- |
| `{CONNECTION_ID}` | The unique `id` value for the connection you want to retrieve. |

**Request**

The following request retrieves information regarding your connection ID.

```shell
curl -X GET \
    'https://platform.adobe.io/data/foundation/flowservice/connections/c8622ec7-7d94-44a5-a35a-ffcc6bdcc384' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**Response**

A successful response returns the current details of your connection including its credentials, unique identifier (`id`), and version.

```json
{
    "items": [
        {
            "id": "c8622ec7-7d94-44a5-a35a-ffcc6bdcc384",
            "createdAt": 1640103419202,
            "updatedAt": 1640104751063,
            "createdBy": "{CREATED_BY}",
            "updatedBy": "{UPDATED_BY}",
            "createdClient": "{CREATED_CLIENT}",
            "updatedClient": "{UPDATED_CLIENT}",
            "sandboxId": "{SANDBOX_ID}",
            "sandboxName": "{SANDBOX_NAME}",
            "imsOrgId": "{ORG_ID}",
            "name": "Airship Attributes",
            "description": "test account connection to Airship Attributes destination",
            "connectionSpec": {
                "id": "34cd3131-b208-474b-b779-b487b5a2bd01",
                "version": "1.0"
            },
            "state": "enabled",
            "auth": {
                "specName": "Bearer Token",
                "params": {
                    "authorizedDate": "2021-12-21",
                    "token": "xxxx"
                }
            },
            "version": "\"8c01091c-0000-0200-0000-61c2032f0000\"",
            "etag": "\"8c01091c-0000-0200-0000-61c2032f0000\""
        }
    ]
}
```

-->

## 接続を削除 {#delete-connection}

>[!IMPORTANT]
>
>宛先アカウントを削除する前に、宛先アカウントへの既存のデータフローを削除する必要があります。
>既存のデータフローを削除するには、次のページを参照してください。
>* [Experience Platform UI を使用 ](../ui/delete-destinations.md) して、既存のデータフローを削除します。
>* [Flow Service API を使用 ](delete-destination-dataflow.md) して、既存のデータフローを削除します。

接続 ID を取得し、宛先アカウントへのデータフローが存在しないことを確認したら、[!DNL Flow Service] API に対してDELETE リクエストを実行します。

**API 形式**

```http
DELETE /connections/{CONNECTION_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{CONNECTION_ID}` | 削除したい接続の一意の `id` 値。 |

**リクエスト**

```shell
curl -X DELETE \
    'https://platform.adobe.io/data/foundation/flowservice/connections/c8622ec7-7d94-44a5-a35a-ffcc6bdcc384' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。接続先へのルックアップ（GET）リクエストを試みることで、削除を確認できます。 API は HTTP 404 （見つかりません）エラーを返し、宛先アカウントが削除されたことを示します。

## API エラー処理 {#api-error-handling}

このチュートリアルの API エンドポイントは、Experience Platform API の一般的なエラーメッセージの原則に従っています。 Experience Platform トラブルシューティングガイドの [API ステータスコード ](../../landing/troubleshooting.md#api-status-codes) および [ リクエストヘッダーエラー ](../../landing/troubleshooting.md#request-header-errors) を参照してください。

## 次の手順

このチュートリアルでは、既存の宛先アカウントを削除する [!DNL Flow Service] API を正常に使用しました。 宛先の使用について詳しくは、[ 宛先の概要 ](/help/destinations/home.md) を参照してください。