---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 複数のオブジェクトの検索
topic: developer guide
translation-type: tm+mt
source-git-commit: f3e9da9ab3d02006c07c59b17751c971a95d49bc

---


# 複数のオブジェクトの検索

1つのオブジェクトにつき1つのリクエストを作成する代わりに、複数の特定のオブジェクトを表示する場合は、同じ種類の複数のオブジェクトをリクエストするためのシンプルなショートカットが提供されます。 1つのGETリクエストを使用して、IDのコンマ区切りのリストを含めることで、複数の特定のオブジェクトを返すことができます。

>[!NOTE] 特定のCatalogオブジェクトをリクエストする場合でも、必要なプロパティのみを返すように `properties` クエリパラメータを設定することをお勧めします。

**API形式**

```http
GET /{OBJECT_TYPE}/{ID_1},{ID_2},{ID_3},{ID_4}
GET /{OBJECT_TYPE}/{ID_1},{ID_2},{ID_3},{ID_4}?properties={PROPERTY_1},{PROPERTY_2},{PROPERTY_3}
```

| `{OBJECT_TYPE}` |取得するCatalogオブジェクトの種類。 有効なオブジェクトは次のとおりです。 <ul><li>`accounts`</li><li>`batches`</li><li>`connections`</li><li>`connectors`</li><li>`dataSets`</li><li>`dataSetFiles`</li><li>`dataSetViews`</li></ul> || `{ID}` |取得する特定のオブジェクトの識別子。 |

**リクエスト**

次のリクエストには、データセットIDのコンマ区切りリストと、各データセットに対して返されるプロパティのコンマ区切りリストが含まれています。

```shell
curl -X GET \
  'https://platform.adobe.io/data/foundation/catalog/dataSets/5bde21511dd27b0000d24e95,5bda3a4228babc0000126377,5bceaa4c26c115000039b24b,5bb276b03a14440000971552,5ba9452f7de80400007fc52a?properties=name,description,files' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -H 'x-api-key: {API_KEY}' \
  -H 'x-gw-ims-org-id: {IMS_ORG}' \
  -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、指定したデータセットのリストを返します。このデータセットには、それぞれに対して要求され`name`たプロパティ(、 `description`、および `files`)のみが含まれます。

>[!NOTE] 返されたオブジェクトに、クエリが示す要求されたプロパティが1つ以上含まれていない場合、応答は、以下の「サンプルデータセット3」および「サンプルデータセット4」に示すように、要求されたプロパティのみを返します。 `properties`

```json
{
    "5ba9452f7de80400007fc52a": {
        "name": "Sample Dataset 1",
        "description": "Description of dataset.",
        "files": "@/dataSets/5ba9452f7de80400007fc52a/views/5ba9452f7de80400007fc52b/files"
    },
    "5bb276b03a14440000971552": {
        "name": "Sample Dataset 2",
        "description": "Description of dataset.",
        "files": "@/dataSets/5bb276b03a14440000971552/views/5bb276b01250b012f9acc75b/files"
    },
    "5bceaa4c26c115000039b24b": {
        "name": "Sample Dataset 3"
    },
    "5bda3a4228babc0000126377": {
        "name": "Sample Dataset 4",
        "files": "@/dataSets/5bda3a4228babc0000126377/views/5bda3a4228babc0000126378/files"
    },
    "5bde21511dd27b0000d24e95": {
        "name": "Sample Dataset 5",
        "description": "Description of dataset.",
        "files": "@/dataSets/5bde21511dd27b0000d24e95/views/5bde21511dd27b0000d24e96/files"
    }
}
```
