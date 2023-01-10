---
keywords: Experience Platform;ホーム;人気のトピック;フローサービス;API;api;削除;データフローの削除
solution: Experience Platform
title: Flow Service API を使用したデータフローの削除
type: Tutorial
description: Flow Service API を使用してバッチおよびストリーミングのデータフローを削除する方法について説明します。
exl-id: ea9040b1-3a40-493d-86f0-27deef09df07
source-git-commit: 59dfa862388394a68630a7136dee8e8988d0368c
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 100%

---

# Flow Service API を使用したデータフローの削除

エラーが含まれていたり古くなったりしたバッチやストリーミングのデータフローは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して削除できます。

このチュートリアルでは、バッチソースとストリーミングソースの両方で作成されたデータフローを、[!DNL Flow Service] を使用して削除する手順を説明します。

## はじめに

このチュートリアルは、有効なフロー ID を保有しているユーザーを対象としています。有効なフロー ID がない場合は、このチュートリアルを試す前に、[ソースの概要](../../home.md)からコネクタを選択して、説明した手順に従ってください。

このチュートリアルでは、Adobe Experience Platform の次のコンポーネントについて十分に理解していることを前提にしています。

* [ソース](../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)：[!DNL Experience Platform] には、単一の [!DNL Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法については詳しくは、[Platform API の概要](../../../landing/api-guide.md)のガイドを参照してください。

## データフローの削除

既存のフロー ID を使用して、[!DNL Flow Service] API に DELETE リクエストを実行することでデータフローを削除できます。

**API 形式**

```http
DELETE /flows/{FLOW_ID}
```

| パラメーター | 説明 |
| --------- | ----------- |
| `{FLOW_ID}` | 削除するデータフローの一意の `id` の値。 |

**リクエスト**

```shell
curl -X DELETE \
    'https://platform.adobe.io/data/foundation/flowservice/flows/20c115bc-46e3-40f3-bfe9-fb25abe4ba76' \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

リクエストが成功した場合は、HTTP ステータス 204（コンテンツなし）が空白の本文とともに返されます。データフローに対してルックアップ（GET）リクエストを試みることで、削除を確認することができます。API はデータフローが削除されたことを示す HTTP 404（見つかりません）エラーを返します。

## 次の手順

このチュートリアルでは、[!DNL Flow Service] API を使用して既存のデータフローを正常に削除しました。

これらの操作をユーザーインターフェイスを使用して実行する手順については、[UI でのデータフローの削除](../../tutorials/ui/delete.md)に関するチュートリアルを参照してください。
