---
keywords: Experience Platform;home;popular topics;flow service;API;api;delete;delete dataflows
solution: Experience Platform
title: Flow Service APIを使用したデータフローの削除
topic: overview
type: Tutorial
description: このチュートリアルでは、Flow Service APIを使用してデータフローを削除する手順を説明します。
translation-type: tm+mt
source-git-commit: ae3e64a5f9a82c0efe3cffeb6d4d425ae2e72bda
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 23%

---


# Flow Service APIを使用したデータフローの削除

このチュートリアルでは、を使用してバッチソースとストリーミングソースの両方で作成したデータフローを削除する手順を説明 [[!DNL Flow Service API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/flow-service.yaml)します。

## はじめに

このチュートリアルでは、有効なフローIDが必要です。 有効なフローIDがない場合は、 [ソースの概要から選択したコネクタを選択し](../../home.md) 、このチュートリアルを試行する前に概要を説明した手順に従ってください。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントについて、十分に理解している必要があります。

* [ソース](../../home.md): [!DNL Experience Platform] 様々なソースからデータを取り込むことができ、 [!DNL Platform] サービスを使用してデータの構造化、ラベル付け、および入力データの拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md): [!DNL Experience Platform] は、1つの [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスを提供します。

The following sections provide additional information that you will need to know in order to successfully delete a dataflow using the [!DNL Flow Service] API.

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください。[!DNL Experience Platform]

### 必須ヘッダーの値の収集

In order to make calls to [!DNL Platform] APIs, you must first complete the [authentication tutorial](../../../tutorials/authentication.md). Completing the authentication tutorial provides the values for each of the required headers in all [!DNL Experience Platform] API calls, as shown below:

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

All resources in [!DNL Experience Platform], including those belonging to [!DNL Flow Service], are isolated to specific virtual sandboxes. All requests to [!DNL Platform] APIs require a header that specifies the name of the sandbox the operation will take place in:

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## データフローの削除

既存のフローIDを使用して、 [!DNL Flow Service] APIに対するDELETE要求を実行することで、データフローを削除できます。

**API 形式**

```http
DELETE /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 削除するデータフローの固有 `id` 値。 |

**リクエスト**

```shell
curl -X DELETE \
    'https://platform-int.adobe.io/data/foundation/flowservice/flows/20c115bc-46e3-40f3-bfe9-fb25abe4ba76' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。データフローに対してルックアップ(GET)リクエストを試行すると、削除を確認できます。 APIは、データフローが削除されたことを示すHTTP 404 （見つかりません）エラーを返します。

## 次の手順

このチュートリアルに従うと、 [!DNL Flow Service] APIを使用して既存のデータフローを削除できます。

ユーザー・インタフェースを使用してこれらの操作を実行する手順については、UIでのデータ・フローの [削除に関するチュートリアルを参照してください](../../tutorials/ui/delete.md)
