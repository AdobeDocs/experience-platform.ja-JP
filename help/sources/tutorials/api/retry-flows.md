---
title: 失敗したデータフロー実行の再試行
description: Flow Service API を使用して、失敗したデータフロー実行を再試行する方法を説明します。
exl-id: b9abc737-9a57-47e6-98ab-6d6c44f38d17
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 11%

---

# 失敗したデータフロー実行の再試行

>[!IMPORTANT]
>
>失敗したデータフロー実行の再試行のサポートは、バッチソースで使用できます。 再試行できるのは、失敗したデータフロー実行のみです。

このチュートリアルでは、[[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/) を使用して、失敗したデータフロー実行を再試行する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ ソース ](../../home.md):Experience Platformを使用すると、データを様々なソースから取得しながら、[!DNL Experience Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [ サンドボックス ](../../../sandboxes/home.md):Experience Platformには、単一の [!DNL Experience Platform] インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Experience Platform API の使用

Experience Platform API を正常に呼び出す方法について詳しくは、[Experience Platform API の概要 ](../../../landing/api-guide.md) を参照してください。

## 失敗したデータフロー実行の再試行

失敗したデータフロー実行を再試行するには、`/runs` エンドポイントに POST リクエストを実行し、データフローの実行 ID と `re-trigger` 操作をクエリパラメーターの一部として指定します。

**API 形式**

```http
POST /runs/{RUN_ID}/action?op=re-trigger
```

| パラメーター | 説明 |
| --- | --- |
| `{RUN_ID}` | 再試行するデータフロー実行に対応する実行 ID。 |
| `op` | 実行するアクションを決定する操作。 失敗したデータフロー実行を再試行するには、操作として `re-trigger` を指定する必要があります。 |

**リクエスト**

>[!NOTE]
>
>`re-trigger` 操作を使用して、成功したデータフロー実行に取り込まれたレコードがゼロである場合に、成功したデータフロー実行を再試行することもできます。

次のリクエストは、実行 ID `4fb0418e-1804-45d6-8d56-dd51f05c0baf` のデータフロー実行を再試行します。

```shell
curl -X POST \
  'https://platform.adobe.io/data/foundation/flowservice/runs/4fb0418e-1804-45d6-8d56-dd51f05c0baf/action?op=re-trigger' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {ORG_ID}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}' \
  -H 'Content-Type: application/json'
```

**応答**

正常な応答は、新しく作成されたフロー実行 ID と、対応する etag バージョンを返します。

```json
{
    "id": "3fb0418e-1804-45d6-8d56-dd51f05c0baf",
    "etag": "\"1100c53e-0000-0200-0000-627138980000\""
}
```
