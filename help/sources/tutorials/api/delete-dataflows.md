---
keywords: Experience Platform、ホーム、人気の高いトピック、フローサービス、API、API、削除、データフローの削除
solution: Experience Platform
title: フローサービス API を使用したデータフローの削除
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してバッチおよびストリーミングのデータフローを削除する方法について説明します。
exl-id: ea9040b1-3a40-493d-86f0-27deef09df07
source-git-commit: 95f455bd03b7baefe0133a9818c9d048f36f9d38
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 14%

---

# フローサービス API を使用したデータフローの削除

エラーを含む、または古くなったバッチおよびストリーミングのデータフローは、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

このチュートリアルでは、を使用して、バッチソースとストリーミングソースの両方でおこなったデータフローを削除する手順を説明します。 [!DNL Flow Service].

## はじめに

このチュートリアルでは、有効なフロー ID が必要です。 有効なフロー ID がない場合は、選択したコネクタを [ソースの概要](../../home.md) このチュートリアルを試す前に、概要を説明した手順に従ってください。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、次のコードを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。
* [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

### Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、 [Platform API の概要](../../../landing/api-guide.md).

## データフローの削除

既存のフロー ID を使用すると、 [!DNL Flow Service] API

**API 形式**

```http
DELETE /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 一意の `id` の値を指定します。 |

**リクエスト**

```shell
curl -X DELETE \
    'https://platform.adobe.io/data/foundation/flowservice/flows/20c115bc-46e3-40f3-bfe9-fb25abe4ba76' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。データフローに対して検索 (GET) リクエストを試行することで、削除を確認できます。 API は、データフローが削除されたことを示す HTTP 404（見つかりません）エラーを返します。

## 次の手順

このチュートリアルでは、 [!DNL Flow Service] 既存のデータフローを削除する API。

ユーザーインターフェイスを使用してこれらの操作を実行する手順については、 [UI でのデータフローの削除](../../tutorials/ui/delete.md)
