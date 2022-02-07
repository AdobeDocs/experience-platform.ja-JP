---
keywords: Experience Platform；ホーム；人気の高いトピック；フローサービス；アカウントの削除；削除；API
solution: Experience Platform
title: フローサービス API を使用したアカウントの削除
topic-legacy: overview
type: Tutorial
description: フローサービス API を使用してアカウントを削除する方法を説明します。
exl-id: 3d07ab7d-c012-472e-8db4-b19e3936dcba
source-git-commit: a51c878bbfd3004cb597ce9244a9ed2f2318604b
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 14%

---

# フローサービス API を使用したアカウントの削除

エラーを含むまたは古くなったソースアカウントは、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

API を使用してアカウントを削除する手順については、次のチュートリアルを参照してください。

## はじめに

このチュートリアルでは、有効な接続 ID が必要です。 有効な接続 ID がない場合は、選択したコネクタを [ソースの概要](../../home.md) このチュートリアルを試す前に、概要を説明した手順に従ってください。

また、このチュートリアルでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../home.md): [!DNL Experience Platform] を使用すると、様々なソースからデータを取り込みながら、次のコードを使用して受信データの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。
* [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] は、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

## Platform API の使用

Platform API への呼び出しを正常に実行する方法について詳しくは、 [Platform API の概要](../../../landing/api-guide.md).

## アカウントを削除

>[!TIP]
>
>ソースアカウントを削除する前に、まず、ソースアカウントに関連付けられている既存のデータフローを削除する必要があります。 既存のデータフローを削除するには、 [ソースデータフローの削除](./delete-dataflows.md).

アカウントを削除するには、 [!DNL Flow Service] 削除するアカウントに対応するベース接続 ID を提供する際の API。

**API 形式**

```http
DELETE /connections/{BASE_CONNECTION_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{BASE_CONNECTION_ID}` | 削除するソースアカウントのベース接続 ID。 |

**リクエスト**

```shell
curl -X DELETE \
  'https://platform.adobe.io/data/foundation/flowservice/connections/dd3631cd-d0ea-4fea-b631-cdd0ea6fea21' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

正常な応答は、空白の本文とともに HTTP ステータス 204（コンテンツなし）を返します。

接続に対して検索 (GET) リクエストを試みて、削除を確認できます。

## 次の手順

このチュートリアルでは、 [!DNL Flow Service] 既存のアカウントを削除する API。

ユーザーインターフェイスを使用してこれらの操作を実行する手順については、 [UI でのアカウントの削除](../../tutorials/ui/delete-accounts.md).
