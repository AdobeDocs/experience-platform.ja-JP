---
title: 失敗したデータフロー実行を再試行
description: フローサービス API を使用して、失敗したデータフローの実行を再試行する方法を説明します。
exl-id: b9abc737-9a57-47e6-98ab-6d6c44f38d17
source-git-commit: d4dba26a151619a555a69287e182ff8398cca7b4
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 38%

---

# 失敗したデータフロー実行の再試行

>[!IMPORTANT]
>
>失敗したデータフローの実行の再試行は、バッチソースでサポートされます。 再試行できるのは、失敗したデータフローの実行のみです。

このチュートリアルでは、 [[!DNL Flow Service] API](https://www.adobe.io/experience-platform-apis/references/flow-service/).

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../home.md)[!DNL Platform]：Experience を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../sandboxes/home.md)[!DNL Platform]：Experience には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

### Platform API の使用

Platform API を正常に呼び出す方法について詳しくは、[Platform API の概要](../../../landing/api-guide.md)のガイドを参照してください。

## 失敗したデータフローの実行を再試行

失敗したデータフローの実行を再試行するには、 `/runs` エンドポイントを使用して、データフローの実行 ID と `re-trigger` 操作をクエリパラメーターの一部として追加する必要があります。

**API 形式**

```http
POST /runs/{RUN_ID}/action?op=re-trigger
```

| パラメーター | 説明 |
| --- | --- |
| `{RUN_ID}` | 再試行するデータフロー実行に対応する実行 ID。 |
| `op` | 実行するアクションを決定する操作です。 失敗したデータフローの実行を再試行するには、次を指定する必要があります： `re-trigger` を設定します。 |

**リクエスト**

>[!NOTE]
>
>以下を使用すると、 `re-trigger` 正常なデータフロー実行に取り込まれたレコードがゼロである場合に、正常なデータフロー実行も再試行する操作。

次のリクエストは、実行 ID のデータフローの実行を再試行します。 `4fb0418e-1804-45d6-8d56-dd51f05c0baf`.

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
