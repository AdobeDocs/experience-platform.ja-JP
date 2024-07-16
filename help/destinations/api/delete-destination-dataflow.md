---
keywords: Experience Platform；ホーム；人気のトピック；フローサービス；API;api；削除；宛先データフローの削除
solution: Experience Platform
title: Flow Service API を使用した宛先データフローの削除
type: Tutorial
description: Flow Service API を使用してバッチ宛先とストリーミング宛先のデータフローを削除する方法について説明します。
exl-id: fa40cf97-46c6-4a10-b53c-30bed2dd1b2d
source-git-commit: c35a29d4e9791b566d9633b651aecd2c16f88507
workflow-type: tm+mt
source-wordcount: '567'
ht-degree: 49%

---

# Flow Service API を使用した宛先データフローの削除

エラーが含まれていたり古くなったりしたデータフローは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して削除できます。

このチュートリアルでは、[!DNL Flow Service] を使用してバッチとストリーミングの両方の宛先へのデータフローを削除する手順を説明します。

## はじめに {#get-started}

このチュートリアルは、有効なフロー ID を保有しているユーザーを対象としています。有効なフロー ID がない場合は、このチュートリアルの内容を試す前に、[ 宛先カタログ ](../catalog/overview.md) から宛先を選択し、[ 宛先に接続 ](../ui/connect-destination.md) および [ データをアクティブ化 ](../ui/activation-overview.md) の手順に従ってください。

このチュートリアルでは、Adobe Experience Platform の次のコンポーネントについて十分に理解していることを前提にしています。

* [ 宛先 ](../home.md):[!DNL Destinations] は、Adobe Experience Platformからのデータの円滑なアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。
* [サンドボックス](../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別個の仮想環境に分割してデジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスが用意されています。

次の節では、[!DNL Flow Service] API を使用してデータフローを正常に削除するために必要な追加情報を示しています。

### API 呼び出し例の読み取り {#reading-sample-api-calls}

このチュートリアルでは、API 呼び出しの例を提供し、リクエストの形式を設定する方法を示します。これには、パス、必須ヘッダー、適切な形式のリクエストペイロードが含まれます。また、API レスポンスで返されるサンプル JSON も示されています。ドキュメントで使用される API 呼び出し例の表記について詳しくは、 トラブルシューテングガイドの[API 呼び出し例の読み方](../../landing/troubleshooting.md#how-do-i-format-an-api-request)に関する節を参照してください[!DNL Experience Platform]。

### 必須ヘッダーの値の収集 {#gather-values-for-required-headers}

[!DNL Platform] API を呼び出すには、まず[認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了する必要があります。次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

* `Authorization: Bearer {ACCESS_TOKEN}`
* `x-api-key: {API_KEY}`
* `x-gw-ims-org-id: {ORG_ID}`

[!DNL Flow Service]に属するリソースを含む、[!DNL Experience Platform] のすべてのリソースは、特定の仮想サンドボックスに分離されます。[!DNL Platform] API へのすべてのリクエストには、操作がおこなわれるサンドボックスの名前を指定するヘッダーが必要です。

* `x-sandbox-name: {SANDBOX_NAME}`

>[!NOTE]
>
>`x-sandbox-name` ヘッダーが指定されていない場合、リクエストは `prod` サンドボックスで解決されます。

ペイロード（POST、PUT、PATCH）を含むすべてのリクエストには、メディアのタイプを指定する以下のような追加ヘッダーが必要です。

* `Content-Type: application/json`

## 宛先データフローの削除 {#delete-destination-dataflow}

既存のフロー ID を使用して、[!DNL Flow Service] API に対して宛先リクエストを実行することで、DELETEデータフローを削除できます。

**API 形式**

```http
DELETE /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 削除したい宛先データフローの一意の `id` 値。 |

**リクエスト**

```shell
curl -X DELETE \
    'https://platform.adobe.io/data/foundation/flowservice/flows/455fa81b-f290-4222-94b6-540a73e3fbc2' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 202（コンテンツなし）が空白の本文とともに返されます。データフローに対してルックアップ（GET）リクエストを試みることで、削除を確認することができます。API はデータフローが削除されたことを示す HTTP 404（見つかりません）エラーを返します。

## API エラー処理 {#api-error-handling}

このチュートリアルの API エンドポイントは、一般的なExperience PlatformAPI エラーメッセージの原則に従っています。 エラー応答の解釈について詳しくは、Platform トラブルシューティングガイドの [API ステータスコード ](/help/landing/troubleshooting.md#api-status-codes) および [ リクエストヘッダーエラー ](/help/landing/troubleshooting.md#request-header-errors) を参照してください。

## 次の手順 {#next-steps}

このチュートリアルでは、[!DNL Flow Service] API を使用して宛先に対する既存のデータフローを正常に削除しました。

これらの操作をユーザーインターフェイスを使用して実行する手順については、[UI でのデータフローの削除 ](../ui/delete-destinations.md) に関するチュートリアルを参照してください。

[!DNL Flow Service] API を使用して、続行して [ 宛先アカウントを削除 ](/help/destinations/api/delete-destination-account.md) できるようになりました。
