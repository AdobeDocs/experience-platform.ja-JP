---
keywords: Experience Platform、ホーム、人気の高いトピック、フローサービス、API、API、削除、宛先データフローの削除
solution: Experience Platform
title: フローサービス API を使用した宛先のデータフローの削除
type: Tutorial
description: フローサービス API を使用して、バッチ宛先およびストリーミング宛先にデータフローを削除する方法について説明します。
source-git-commit: df89f8ce8050b26068e0ab7aa01f1c964f5d2422
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 44%

---

# フローサービス API を使用した宛先のデータフローの削除

エラーを含むデータフローや古くなったデータフローは、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

このチュートリアルでは、 [!DNL Flow Service].

## はじめに {#get-started}

このチュートリアルでは、有効なフロー ID が必要です。 有効なフロー ID がない場合は、 [宛先カタログ](../catalog/overview.md) そして、以下に示す手順に従います。 [宛先に接続](../ui/connect-destination.md) および [データをアクティブ化](../ui/activation-overview.md) このチュートリアルを試す前に

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [宛先は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。](../home.md)[!DNL Destinations]宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。
* [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

以下の節では、 [!DNL Flow Service] API

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。この中には、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集 {#gather-values-for-required-headers}

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja#platform-apis)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {IMS_ORG}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>この `x-sandbox-name` ヘッダーが指定されていない場合、リクエストは `prod` サンドボックス。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 宛先のデータフローの削除 {#delete-destination-dataflow}

既存のフロー ID を使用すると、 [!DNL Flow Service] API

**API 形式**

```http
DELETE /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 一意の `id` 削除する宛先データフローの値。 |

**リクエスト**

```shell
curl -X DELETE \
    'https://platform.adobe.io/data/foundation/flowservice/flows/455fa81b-f290-4222-94b6-540a73e3fbc2' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答の場合は、空白の本文とともに HTTP ステータス 202 （コンテンツなし）が返されます。データフローに対して検索 (GET) リクエストを試行することで、削除を確認できます。 API は、データフローが削除されたことを示す HTTP 404（見つかりません）エラーを返します。

## API エラー処理 {#api-error-handling}

このチュートリアルの API エンドポイントは、Experience PlatformAPI エラーメッセージの一般的な原則に従います。 参照： [API ステータスコード](../../landing/troubleshooting.md#api-status-codes) および [リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors) （Platform トラブルシューティングガイド）を参照してください。

## 次の手順 {#next-steps}

このチュートリアルでは、 [!DNL Flow Service] 宛先への既存のデータフローを削除する API。

ユーザーインターフェイスを使用してこれらの操作を実行する手順については、 [UI でのデータフローの削除](../ui/delete-destinations.md).
