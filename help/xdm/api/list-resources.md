---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リスト資源
topic: developer guide
translation-type: tm+mt
source-git-commit: 1541b027a4e572dc5e4e64de1117a269c58bafab

---


# リスト資源

1つのGET要求を実行することで、コンテナ内のすべてのリソース(スキーマ、クラス、ミックスイン、またはデータタイプ)のリストを表示できます。 結果のフィルタリングに役立つように、スキーマレジストリでは、リソースをリストする際にクエリパラメーターの使用をサポートしています。

最も一般的なクエリパラメータは次のとおりです。

* `limit`  — 返されるリソースの数を制限します。 例：5つのリ `limit=5` ソースのリストを返します。
* `orderby`  — 特定のプロパティで結果を並べ替えます。 例：タイ `orderby=title` トルの昇順(A ～ Z)で結果を並べ替えます。 タイトルの `-` 前に(`orderby=-title`)を追加すると、タイトルの降順(Z-A)で項目が並べ替えられます。
* `property`  — 任意の最上位属性に対する結果をフィルタします。 例えば、はXDM Individual `property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile` プロファイルクラスと互換性のあるミックスインのみを返します。

複数のクエリパラメーターを組み合わせる場合は、アンパサンド(`&`)で区切る必要があります。

**API形式**

```http
GET /{CONTAINER_ID}/{RESOURCE_TYPE}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | リソースが配置されるコンテナ（「グローバル」または「テナント」）。 |
| `{RESOURCE_TYPE}` | リソースライブラリから取得するスキーマのタイプ。 有効なタイプは、、、、お `datatypes`よびの `mixins`いず `schemas`れかです `classes`。 |

**リクエスト**

```SHELL
curl -X GET \
  https://platform.adobe.io/data/foundation/schemaregistry/global/classes&limit=2 \
  -H 'Accept: application/vnd.adobe.xed-id+json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

応答の形式は、リクエストで送信されるAcceptヘッダーに依存します。 リソースの一覧表示には、次のAcceptヘッダーを使用できます。

| ヘッダーを受け入れる | 説明 |
| ------- | ------------ |
| application/vnd.adobe.xed-id+json | 各リソースの短い概要を返します。一般に、リストに適したヘッダです。 |
| application/vnd.adobe.xed+json | 各リソースの完全なJSONスキーマを、元のリソースと含めて `$ref` 返しま `allOf` す。 |

**応答**

上記のリクエストでは「受け入れ」 `application/vnd.adobe.xed-id+json` ヘッダーが使用されていたので、応答には各リソースの「 `title`」、「」、「」 `$id`、「」お `meta:altId`よび「」 `version` 属性のみが含まれています。 Acceptヘッダ `full` ーに置き換えると、各リソースのすべての属性が返されます。 応答で必要な情報に応じて、適切な「受け入れ」ヘッダーを選択します。

```JSON
{
  "results": [
    {
        "title": "XDM ExperienceEvent",
        "$id": "https://ns.adobe.com/xdm/context/experienceevent",
        "meta:altId": "_xdm.context.experienceevent",
        "version": "1"
    },
    {
        "title": "XDM Individual Profile",
        "$id": "https://ns.adobe.com/xdm/context/profile",
        "meta:altId": "_xdm.context.profile",
        "version": "1"
    }
  ]
}
```
