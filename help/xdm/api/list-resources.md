---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リスト資源
topic: developer guide
translation-type: tm+mt
source-git-commit: fe7b6acf86ebf39da728bb091334785a24d86b49

---


# リスト資源

1つのGET要求を実行することで、コンテナ内のすべてのリソース(スキーマ、クラス、ミックスイン、またはデータタイプ)のリストを表示できます。

**API形式**

```http
GET /{CONTAINER_ID}/{RESOURCE_TYPE}
GET /{CONTAINER_ID}/{RESOURCE_TYPE}?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | リソースが配置されるコンテナ（「グローバル」または「テナント」）。 |
| `{RESOURCE_TYPE}` | リソースライブラリから取得するスキーマのタイプ。 有効なタイプは、、、、お `datatypes`よびの `mixins`いず `schemas`れかです `classes`。 |
| `{QUERY_PARAMS`} | 結果をフィルターするクエリパラメーター（オプション）。 詳しくは、 [クエリパラメータ](#query) の節を参照してください。 |

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
| application/vnd.adobe.xed-id+json | 各リソースの短い概要を返します。一般に、リストの際に推奨されるヘッダです。 |
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

## クエリパラメータの使用 {#query}

スキーマレジストリは、リソースのリストを作成する際に、クエリパラメーターを使用して、結果をページに貼り付け、フィルタリングすることをサポートしています。

>[!NOTE] 複数のクエリパラメーターを組み合わせる場合は、アンパサンド(`&`)で区切る必要があります。

### ページ

ページングに最も一般的なクエリパラメータは次のとおりです。

| パラメーター | 説明 |
| --- | --- |
| `start` | リストの結果をどこで差し引くかを指定します。 例：3番目 `start=2` の返された項目の結果が次に返されます。 |
| `limit` | 返されるリソースの数を制限します。 例：5つのリ `limit=5` ソースのリストを返します。 |
| `orderby` | 特定のプロパティで結果を並べ替えます。 例：タイ `orderby=title` トルの昇順(A ～ Z)で結果を並べ替えます。 タイトルの `-` 前に(`orderby=-title`)を追加すると、タイトルの降順(Z-A)で項目が並べ替えられます。 |

### フィルター

パラメーターを使用して結果をフィルターでき `property` ます。このパラメーターは、取得したリソース内の特定のJSONプロパティに対して特定の演算子を適用するために使用されます。 次の演算子がサポートされています。

| 演算子 | 説明 | 例 |
| --- | --- | --- |
| `==` | フィルターを指定します。 | `property=title==test` |
| `!=` | フィルター。指定した値とプロパティが等しくないかどうか。 | `property=title!=test` |
| `<` | フィルターを指定します。 | `property=version<5` |
| `>` | フィルター。指定した値より大きいプロパティを持つかどうか。 | `property=version>5` |
| `<=` | フィルター。指定した値以下か等しいか。 | `property=version<=5` |
| `>=` | フィルター。指定した値以上かどうか。 | `property=version>=5` |
| `~` | フィルター。指定した正規式とプロパティが一致するかどうか。 | `property=title~test$` |
| (None) | プロパティ名のみを指定すると、プロパティが存在するエントリのみが返されます。 | `property=title` |

>[!TIP] このパラメータを使用して、互 `property` 換性のあるクラスでミックスインをフィルタリングできます。 例えば、はXDM Individual `property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile` プロファイルクラスと互換性のあるミックスインのみを返します。
