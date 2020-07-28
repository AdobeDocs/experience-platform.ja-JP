---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リソースのリスト
topic: developer guide
translation-type: tm+mt
source-git-commit: d04bf35e49488ab7d5e07de91eb77d0d9921b6fa
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 30%

---


# リソースのリスト

単一のGET要求を実行することで、コンテナ内の特定のタイプ(クラス、ミックスイン、スキーマ、データ型、または記述子)のすべての [!DNL Schema Registry] リソースのリストを表示できます。

>[!NOTE]
>
>リソースをリストする場合、結果セットは300項目に [!DNL Schema Registry] 制限されます。 この制限を超えるリソースを返すには、 [ページングパラメーターを使用する必要があります](#paging)。 また、結果を [フィルターし](#filtering) 、返されるリソースの数を減らすために、クエリパラメーターを使用することもお勧めします。

**API 形式**

```http
GET /{CONTAINER_ID}/{RESOURCE_TYPE}
GET /{CONTAINER_ID}/{RESOURCE_TYPE}?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | リソースが配置されるコンテナ（「グローバル」または「テナント」）。 |
| `{RESOURCE_TYPE}` | The type of resource to retrieve from the [!DNL Schema Library]. Valid types are `classes`, `mixins`, `schemas`, `datatypes`, and `descriptors`. |
| `{QUERY_PARAMS`} | 結果をフィルターするオプションのクエリパラメーター。 詳しくは、 [クエリパラメーターの節を参照してください](#query) 。 |

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

応答の形式は、リクエストで送信される Accept ヘッダーによって異なります。次の Accept ヘッダーを使用して、リソースをリストできます。

| Accept ヘッダー | 説明 |
| ------- | ------------ |
| application/vnd.adobe.xed-id+json | 各リソースの短い概要を返します。 リソースのリストを表示する際に推奨されるヘッダーです。 (制限： 300) |
| application/vnd.adobe.xed+json | 各リソースの完全な JSON スキーマを、元の `$ref` と `allOf` を含めて返します。。(制限： 300) |
| application/vnd.adobe.xdm-v2+json | エンドポイントを使用する場合、ページング機能を利用するために、このAcceptヘッダーを使用する必要があります。 `/descriptors` |

**応答**

上記のリクエストでは、`application/vnd.adobe.xed-id+json` Accept ヘッダーが使用されていたため、応答には各リソースの `title`、`$id`、`meta:altId` および `version` 属性のみが含まれています。Accept ヘッダーに `full` を代入すると、各リソースのすべての属性が返されます。応答で必要な情報に応じて、適切な Accept ヘッダーを選択してください。

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

## クエリパラメーターの使用 {#query}

は、リソースのリスト表示時に、ページを作成し結果をフィルタリングするクエリパラメーターの使用をサポートしています。 [!DNL Schema Registry]

>[!NOTE]
>
>複数のクエリパラメーターを組み合わせる場合は、アンパサンド（`&`）で区切る必要があります。

### ページング {#paging}

ページングに最も一般的なクエリパラメーターは次のとおりです。

| パラメーター | 説明 |
| --- | --- |
| `start` | リストの結果を開始する場所を指定します。 この値はリスト応答の `_page.next` 属性から取得でき、結果の次のページにアクセスするのに使用されます。 この `_page.next` 値がnullの場合、追加のページはありません。 |
| `limit` | 返されるリソースの数を制限する。 例：`limit=5` は 5 つのリソースのリストを返します。 |
| `orderby` | 特定のプロパティで結果を並べ替える。 例：`orderby=title` は昇順（A ～ Z）のタイトルで結果を並べ替えます。タイトルの前に `-` を追加すると（`orderby=-title`）、降順（Z ～ A）のタイトルで項目が並べ替えられます。 |

### フィルター {#filtering}

パラメーターを使用して結果をフィルターできます。この `property` パラメーターは、取得したリソース内の特定のJSONプロパティに対して特定の演算子を適用するのに使用されます。 次の演算子がサポートされています。

| 演算子 | 説明 | 例 |
| --- | --- | --- |
| `==` | プロパティが指定された値と等しいかどうかを示すフィルター。 | `property=title==test` |
| `!=` | プロパティが指定された値と等しくないかどうかによるフィルター。 | `property=title!=test` |
| `<` | プロパティが指定された値より小さいかどうかによるフィルター。 | `property=version<5` |
| `>` | プロパティが指定された値より大きいかどうかによるフィルター。 | `property=version>5` |
| `<=` | プロパティが指定した値以下か、または指定した値と等しいかによるフィルター。 | `property=version<=5` |
| `>=` | プロパティが指定された値以上かどうかによるフィルター。 | `property=version>=5` |
| `~` | 指定された正規式とプロパティが一致するかどうかを示すフィルター。 | `property=title~test$` |
| なし | プロパティ名のみをステートすると、プロパティが存在するエントリのみが返されます。 | `property=title` |

>[!TIP]
>
>この `property` パラメーターを使用して、互換性のあるクラスでミックスインをフィルタリングできます。 For example, `property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile` returns only mixins that are compatible with the [!DNL XDM Individual Profile] class.
