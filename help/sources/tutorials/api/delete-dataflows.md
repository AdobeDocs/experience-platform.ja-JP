---
keywords: Experience Platform、ホーム、人気のあるトピック、フローサービス、API、API、削除、データフローの削除
solution: Experience Platform
title: フローサービスAPIを使用したデータフローの削除
topic-legacy: overview
type: Tutorial
description: フローサービスAPIを使用してバッチおよびストリーミングデータフローを削除する方法について説明します。
exl-id: ea9040b1-3a40-493d-86f0-27deef09df07
source-git-commit: b4291b4f13918a1f85d73e0320c67dd2b71913fc
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 41%

---

# フローサービスAPIを使用したデータフローの削除

エラーを含むバッチおよびストリーミングデータフローを削除したり、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/)を使用して古くなったデータフローを削除したりできます。

このチュートリアルでは、[!DNL Flow Service]を使用して、バッチソースとストリーミングソースの両方で作成したデータフローを削除する手順を説明します。

## はじめに

このチュートリアルでは、有効なフローIDが必要です。 有効なフローIDがない場合は、[ソースの概要](../../home.md)から目的のコネクタを選択し、このチュートリアルを試す前に説明した手順に従います。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、サービスを使用して受信データの構造化、ラベル付け、拡張をおこなうことがで [!DNL Platform] きます。
* [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、[!DNL Flow Service] APIを使用してデータフローを正しく削除するために知っておく必要がある追加情報を示します。

### API 呼び出し例の読み取り

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## データフローの削除

既存のフローIDを使用して、[!DNL Flow Service] APIに対してDELETEリクエストを実行することで、データフローを削除できます。

**API 形式**

```http
DELETE /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 削除するデータフローの一意の`id`値。 |

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

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。データフローに対してルックアップ(GET)リクエストを試行して、削除を確認できます。 APIは、データフローが削除されたことを示すHTTP 404（未検出）エラーを返します。

## 次の手順

このチュートリアルでは、 [!DNL Flow Service] APIを使用して既存のデータフローを削除しました。

ユーザーインターフェイスを使用してこれらの操作を実行する手順については、UIでのデータフローの削除[に関するチュートリアルを参照してください。](../../tutorials/ui/delete.md)
