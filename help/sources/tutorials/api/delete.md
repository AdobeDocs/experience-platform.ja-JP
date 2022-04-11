---
keywords: Experience Platform;ホーム;人気の高いトピック;フローサービス;アカウントの削除;削除;API
solution: Experience Platform
title: Flow Service API を使用したアカウントの削除
topic-legacy: overview
type: Tutorial
description: Flow Service API を使用してアカウントを削除する方法を説明します。
exl-id: 3d07ab7d-c012-472e-8db4-b19e3936dcba
source-git-commit: 95f455bd03b7baefe0133a9818c9d048f36f9d38
workflow-type: ht
source-wordcount: '339'
ht-degree: 100%

---

# Flow Service API を使用したアカウントの削除

エラーを含むソースアカウントや古くなったソースアカウントは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して削除できます。

API を使用してアカウントを削除する手順については、次のチュートリアルを参照してください。

## はじめに

このチュートリアルでは、有効な接続 ID が必要です。有効な接続 ID がない場合は、このチュートリアルを試す前に、[ソースの概要](../../home.md)からコネクタを選択して、説明した手順に従ってください。

このチュートリアルでは、Adobe Experience Platform の次のコンポーネントについて十分に理解していることを前提にしています。

* [ソース](../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../landing/api-guide.md)を参照してください。

## アカウントの削除

>[!TIP]
>
>ソースアカウントを削除する前に、まず、ソースアカウントに関連付けられている既存のデータフローを削除する必要があります。既存のデータフローを削除するには、[ソースデータフローの削除](./delete-dataflows.md)のチュートリアルを参照してください。

アカウントを削除するには、[!DNL Flow Service] API に DELETE リクエストを行い、削除するアカウントに対応するベース接続 ID を指定します。

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

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。

接続先へのルックアップ（GET）リクエストを試みることで、削除を確認できます。

## 次の手順

このチュートリアルでは、既存のアカウントを削除する [!DNL Flow Service] API を正常に使用しました。

ユーザーインターフェイスを使用してこれらの操作を実行する手順については、[UI でのアカウントの削除](../../tutorials/ui/delete-accounts.md)のチュートリアルを参照してください。
