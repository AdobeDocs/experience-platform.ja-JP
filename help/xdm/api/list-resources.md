---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: リストリソース
topic: developer guide
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '519'
ht-degree: 2%

---


# リストリソース

1つのGET要求を実行することで、コンテナ内の特定の種類(クラス、ミックスイン、スキーマ、データ型、または記述子)のスキーマレジストリリソースのリストを表示できます。

>[!NOTE]
>
>リソースをリストする場合、スキーマレジストリでは結果セットが300項目に制限されます。 この制限を超えるリソースを返すには、 [ページングパラメーターを使用する必要があります](#paging)。 また、結果を [フィルターし](#filtering) 、返されるリソースの数を減らすために、クエリパラメーターを使用することもお勧めします。

**API形式**

```http
GET /{CONTAINER_ID}/{RESOURCE_TYPE}
GET /{CONTAINER_ID}/{RESOURCE_TYPE}?{QUERY_PARAMS}
```

| パラメーター | 説明 |
| --- | --- |
| `{CONTAINER_ID}` | リソースが配置されているコンテナ（「グローバル」または「テナント」）。 |
| `{RESOURCE_TYPE}` | スキーマライブラリから取得するリソースのタイプ。 有効なタイプは、 `classes`、、、、、 `mixins`、 `schemas``datatypes``descriptors`です。 |
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

応答の形式は、要求で送信されるAcceptヘッダーによって異なります。 リソースの一覧表示には、次のAcceptヘッダーを使用できます。

| ヘッダーを受け入れる | 説明 |
| ------- | ------------ |
| application/vnd.adobe.xed-id+json | 各リソースの短い概要を返します。 リソースのリストを表示する際に推奨されるヘッダーです。 (制限： 300) |
| application/vnd.adobe.xed+json | 各リソースの完全なJSONスキーマを返します。オリジナルと含ま `$ref` れ `allOf` ています。 (制限： 300) |
| application/vnd.adobe.xdm-v2+json | エンドポイントを使用する場合、ページング機能を利用するために、このAcceptヘッダーを使用する必要があります。 `/descriptors` |

**応答**

上記のリクエストでは `application/vnd.adobe.xed-id+json` Acceptヘッダーが使用されていたので、応答には各リソースの `title`、 `$id`、 `meta:altId`および `version` 属性のみが含まれます。 Acceptヘッダ `full` に置き換えると、各リソースのすべての属性が返されます。 応答で必要な情報に応じて、適切な「承認」ヘッダーを選択します。

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

スキーマレジストリでは、リソースのリストを作成する際に、クエリパラメーターを使用してページを作成し、結果をフィルターすることができます。

>[!NOTE]
>
>複数のクエリパラメーターを組み合わせる場合は、アンパサンド(`&`)で区切る必要があります。

### ページング {#paging}

ページングに最も一般的なクエリパラメーターは次のとおりです。

| パラメーター | 説明 |
| --- | --- |
| `start` | リストの結果を開始する場所を指定します。 この値はリスト応答の `_page.next` 属性から取得でき、結果の次のページにアクセスするのに使用されます。 この `_page.next` 値がnullの場合、追加のページはありません。 |
| `limit` | 返されるリソースの数を制限する。 例： `limit=5` は、5つのリソースのリストを返します。 |
| `orderby` | 特定のプロパティで結果を並べ替える。 例： `orderby=title` は、タイトルを昇順(A ～ Z)で並べ替えます。 タイトル()の `-``orderby=-title`前にを追加すると、タイトル(Z-A)の降順で項目が並べ替えられます。 |

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
| (None) | プロパティ名のみをステートすると、プロパティが存在するエントリのみが返されます。 | `property=title` |

>[!TIP]
>
>この `property` パラメーターを使用して、互換性のあるクラスでミックスインをフィルタリングできます。 例えば、はXDM Individualプロファイルクラスと互換性のあるミックスインのみを `property=meta:intendedToExtend==https://ns.adobe.com/xdm/context/profile` 返します。
