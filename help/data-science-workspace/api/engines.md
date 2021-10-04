---
keywords: Experience Platform；開発者ガイド；エンドポイント；Data Science Workspace；よく読まれるトピック；エンジン；sensei 機械学習 api
solution: Experience Platform
title: エンジン API エンドポイント
topic-legacy: Developer guide
description: エンジンは、Data Science Workspace　での機械学習モデルの基礎です。特定の問題を解決する機械学習アルゴリズム、特徴エンジニアリングを実行する特徴パイプライン、またはその両方が含まれます。
exl-id: 7c670abd-636c-47d8-bd8c-5ce0965ce82f
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1165'
ht-degree: 69%

---

# エンジンエンドポイント

エンジンは、Data Science Workspace　での機械学習モデルの基礎です。特定の問題を解決する機械学習アルゴリズム、特徴エンジニアリングを実行する特徴パイプライン、またはその両方が含まれます。

## Docker レジストリの検索

>[!TIP]
>
>Docker URL がない場合は、『[ ソースファイルをレシピにパッケージ化 ](../models-recipes/package-source-files-recipe.md)』チュートリアルを参照して、Docker ホスト URL の作成手順を確認してください。

パッケージ化されたレシピファイル（Docker ホストの URL、ユーザー名、パスワードなど）をアップロードするには、Docker レジストリ資格情報が必要です。この情報は、次の GET リクエストを実行することで調べることができます。

**API 形式**

```https
GET /engines/dockerRegistry
```

**リクエスト**

```shell
curl -X GET https://platform.adobe.io/data/sensei/engines/dockerRegistry \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、Docker URL（`host`）、ユーザー名（`username`）、パスワード（`password`）を含む Docker レジストリの詳細を含むペイロードを返します。

>[!NOTE]
>
> Docker のパスワードは、`{ACCESS_TOKEN}` が更新するたびに変更されます。

```json
{
    "host": "docker_host.azurecr.io",
    "username": "00000000-0000-0000-0000-000000000000",
    "password": "password"
}
```

## Docker URL を使用したエンジンの作成 {#docker-image}

エンジンを作成するには、メタデータと、マルチパートフォームの Docker 画像を参照する Docker URL を提供しながら、POST リクエストを実行します。

**API 形式**

```https
POST /engines
```

**Python/R をリクエスト**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/engines \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: multipart/form-data' \
    -F 'engine={
        "name": "A name for this Engine",
        "description": "A description for this Engine",
        "type": "Python",
        "algorithm": "Classification",
        "artifacts": {
            "default": {
                "image": {
                    "location": "v1rsvj32smc4wbs.azurecr.io/ml-featurepipeline-pyspark:1.0",
                    "name": "An additional name for the Docker image",
                    "executionType": "Python"
                }
            }
        }
    }' 
```

| プロパティ | 説明 |
| --- | --- |
| `name` | エンジンの名前。このエンジンに対応するレシピは、UI に表示されるこの値をレシピ名として継承します。 |
| `description` | エンジンのオプションの説明。このエンジンに対応するレシピは、UI に表示されるこの値をレシピの説明として継承します。このプロパティが必要です。説明を指定しない場合は、値を空の文字列に設定します。 |
| `type` | エンジンの実行タイプ。この値は、Docker イメージの構築元の言語に対応し、「Python」、「R」、「Tensorflow」のいずれかを指定できます。 |
| `algorithm` | 機械学習アルゴリズムのタイプを指定する文字列。サポートされるアルゴリズムのタイプには、「分類」、「回帰」、「カスタム」があります。 |
| `artifacts.default.image.location` | Docker URL によってリンクされた Docker イメージの場所。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。この値は、Docker イメージの構築元の言語に対応し、「Python」、「R」、「Tensorflow」のいずれかを指定できます。 |

**PySpark/Scala をリクエスト**

PySpark レシピをリクエストする場合、 `executionType` と `type` は「PySpark」になります。 Scala レシピをリクエストする場合、`executionType` と `type` は「Spark」になります。 次の Scala レシピの例では、Spark を使用しています。

```shell
curl -X POST \
  https://platform.adobe.io/data/sensei/engines \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: multipart/form-data' \
    -F 'engine={
    "name": "Spark retail sales recipe",
    "description": "A description for this Engine",
    "type": "Spark",
    "mlLibrary":"databricks-spark",
    "artifacts": {
        "default": {
            "image": {
                "name": "modelspark",
                "executionType": "Spark",
                "packagingType": "docker",
                "location": "v1d2cs4mimnlttw.azurecr.io/sarunbatchtest:0.0.1"
            }
        }
    }
}'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | エンジンの名前。このエンジンに対応するレシピは、UI に表示されるこの値をレシピ名として継承します。 |
| `description` | エンジンのオプションの説明。このエンジンに対応するレシピは、UI に表示されるこの値をレシピの説明として継承します。このプロパティが必要です。説明を指定しない場合は、値を空の文字列に設定します。 |
| `type` | エンジンの実行タイプ。この値は、Docker イメージの構築元の言語に対応します。 値は Spark または PySpark に設定できます。 |
| `mlLibrary` | PySpark と Scala のレシピ用のエンジンを作成する際に必要なフィールドです。 このフィールドは `databricks-spark` に設定する必要があります。 |
| `artifacts.default.image.location` | Docker イメージの場所。 Azure ACRまたはパブリック（未認証）Dockerhub のみがサポートされています。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。この値は、Docker イメージの構築元の言語に対応します。 「Spark」または「PySpark」を指定できます。 |

**応答**

正常な応答は、新たに作成されたエンジンの一意の ID（`id`）を含む詳細を含むペイロードを返します。次のレスポンスの例は、Python エンジン用です。 すべてのエンジン応答は、次の形式に従います。

```json
{
    "id": "22f4166f-85ba-4130-a995-a2b8e1edde32",
    "name": "A name for this Engine",
    "description": "A description for this Engine",
    "type": "Python",
    "algorithm": "Classification",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z",
    "artifacts": {
        "default": {
            "image": {
                "location": "v1rsvj32smc4wbs.azurecr.io/ml-featurepipeline-pyspark:1.0",
                "name": "An additional name for the Docker image",
                "executionType": "Python",
                "packagingType": "docker"
            }
        }
    }
}
```

## Docker URL を使用した機能パイプラインエンジンの作成 {#feature-pipeline-docker}

機能パイプラインエンジンを作成するには、メタデータと Docker 画像を参照する Docker URL を提供しながらPOSTリクエストを実行します。

**API 形式**

```https
POST /engines
```

**リクエスト**

```shell
curl -X POST \
 https://platform.adobe.io/data/sensei/engines \
    -H 'Authorization: Bearer ' \
    -H 'x-gw-ims-org-id: 20655D0F5B9875B20A495E23@AdobeOrg' \
    -H 'Content-Type: application/vnd.adobe.platform.sensei+json;profile=engine.v1.json' \
    -H 'x-api-key: acp_foundation_machineLearning' \
    -H 'Content-Type: text/plain' \
    -F '{
    "type": "PySpark",
    "algorithm":"fp",
    "name": "Feature_Pipeline_Engine",
    "description": "Feature_Pipeline_Engine",
    "mlLibrary": "databricks-spark",
    "artifacts": {
       "default": {
           "image": {
                "location": "v7d1cs2mimnlttw.azurecr.io/ml-featurepipeline-pyspark:0.2.1",
                "name": "datatransformation",
                "executionType": "PySpark",
                "packagingType": "docker"
            },
           "defaultMLInstanceConfigs": [ ...
           ]
       }
   }
}'
```

| プロパティ | 説明 |
| --- | --- |
| `type` | エンジンの実行タイプ。この値は、Docker イメージの構築元の言語に対応します。 値は Spark または PySpark に設定できます。 |
| `algorithm` | 使用するアルゴリズムで、この値を `fp`（機能パイプライン）に設定します。 |
| `name` | 機能パイプラインエンジンの名前。 このエンジンに対応するレシピは、UI に表示されるこの値をレシピ名として継承します。 |
| `description` | エンジンのオプションの説明。このエンジンに対応するレシピは、UI に表示されるこの値をレシピの説明として継承します。このプロパティが必要です。説明を指定しない場合は、値を空の文字列に設定します。 |
| `mlLibrary` | PySpark と Scala のレシピ用のエンジンを作成する際に必要なフィールドです。 このフィールドは `databricks-spark` に設定する必要があります。 |
| `artifacts.default.image.location` | Docker イメージの場所。 Azure ACRまたはパブリック（未認証）Dockerhub のみがサポートされています。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。この値は、Docker イメージの構築元の言語に対応します。 「Spark」または「PySpark」を指定できます。 |
| `artifacts.default.image.packagingType` | エンジンのパッケージタイプ。 この値は `docker` に設定する必要があります。 |
| `artifacts.default.defaultMLInstanceConfigs` | `pipeline.json` 構成ファイルのパラメータ。 |

**応答**

正常な応答は、新しく作成された機能パイプラインエンジンの一意の識別子 (`id`) を含む詳細を含むペイロードを返します。 次のレスポンスは、PySpark 機能パイプラインエンジンの場合のものです。

```json
{
    "id": "88236891-4309-4fd9-acd0-3de7827cecd1",
    "name": "Feature_Pipeline_Engine",
    "description": "Feature_Pipeline_Engine",
    "type": "PySpark",
    "algorithm": "fp",
    "mlLibrary": "databricks-spark",
    "created": "2020-04-24T20:46:58.382Z",
    "updated": "2020-04-24T20:46:58.382Z",
    "deprecated": false,
    "artifacts": {
        "default": {
            "image": {
                "location": "v7d1cs3mimnlttw.azurecr.io/ml-featurepipeline-pyspark:0.2.1",
                "name": "datatransformation",
                "executionType": "PySpark",
                "packagingType": "docker"
            },
        "defaultMLInstanceConfigs": [ ... ]
        }
    }
}
```

## エンジンのリストの取得

エンジンのリストは、1 回の GET リクエストを実行することで取得できます。結果のフィルタリングに役立つように、リクエストパスでクエリパラメーターを指定できます。使用可能なクエリのリストについては、「[アセット取得のためのクエリーパラメーター](./appendix.md#query)」の付録の節を参照してください。

**API 形式**

```https
GET /engines
GET /engines?parameter_1=value_1
GET /engines?parameter_1=value_1&parameter_2=value_2
```

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/engines \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、エンジンのリストとその詳細を返します。

```json
{
    "children": [
        {
            "id": "22f4166f-85ba-4130-a995-a2b8e1edde31",
            "name": "A name for this Engine",
            "description": "A description for this Engine",
            "type": "PySpark",
            "algorithm": "Classification",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-01T00:00:00.000Z"
        },
        {
            "id": "22f4166f-85ba-4130-a995-a2b8e1edde32",
            "name": "A name for this Engine",
            "description": "A description for this Engine",
            "type": "Python",
            "algorithm": "Classification",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-01T00:00:00.000Z"
        },
        {
            "id": "22f4166f-85ba-4130-a995-a2b8e1edde33",
            "name": "Feature Pipeline Engine",
            "description": "A feature pipeline Engine",
            "type": "PySpark",
            "algorithm":"fp",
            "created": "2019-01-01T00:00:00.000Z",
            "createdBy": {
                "userId": "Jane_Doe@AdobeID"
            },
            "updated": "2019-01-01T00:00:00.000Z"
        }
    ],
    "_page": {
        "property": "deleted==false",
        "totalCount": 100,
        "count": 3
    }
}
```

### 特定のエンジンの取得 {#retrieve-specific}

特定のエンジンの詳細を取得するには、目的のエンジンの ID をリクエストパスに含む GET リクエストを実行します。

**API 形式**

```https
GET /engines/{ENGINE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{ENGINE_ID}` | 既存のエンジンの ID。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/engines/22f4166f-85ba-4130-a995-a2b8e1edde32 \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

正常な応答は、目的のエンジンの詳細を含むペイロードを返します。

```json
{
    "id": "22f4166f-85ba-4130-a995-a2b8e1edde32",
    "name": "A name for this Engine",
    "description": "A description for this Engine",
    "type": "PySpark",
    "algorithm": "Classification",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z",
    "artifacts": {
        "default": {
            "image": {
                "location": "v7d1cs2mimnlttw.azurecr.io/ml-featurepipeline-pyspark:0.2.1",
                "name": "file.egg",
                "executionType": "PySpark",
                "packagingType": "docker"
            }
        }
    }
}
```

## エンジンの更新

既存のエンジンを変更して更新するには、リクエストパスにターゲットエンジンの ID を含む PUT リクエストを使用してプロパティを上書きし、更新されたプロパティを含む JSON ペイロードを提供します。

>[!NOTE]
>
>この PUT リクエストを確実に成功させるために、まず GET リクエストを実行して、[ID でエンジンを取得](#retrieve-specific)することをお勧めします。次に、返された JSON オブジェクトを変更および更新し、変更された JSON オブジェクト全体を PUT リクエストのペイロードとして適用します。

以下の API 呼び出し例を使用すると、これらのプロパティを最初に持つ間に、エンジンの名前と説明が更新されます。

```json
{
    "name": "A name for this Engine",
    "description": "A description for this Engine",
    "type": "Python",
    "algorithm": "Classification",
    "artifacts": {
        "default": {
            "image": {
                "executionType": "Python",
                "packagingType": "docker"
            }
        }
    }
}
```

**API 形式**

```https
PUT /engines/{ENGINE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{ENGINE_ID}` | 既存のエンジンの ID。 |

**リクエスト**

```shell
curl -X PUT \
    https://platform.adobe.io/data/sensei/engines/22f4166f-85ba-4130-a995-a2b8e1edde32 \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: application/vnd.adobe.platform.sensei+json;profile=engine.v1.json' \
    -d '{
        "name": "An updated name for this Engine",
        "description": "An updated description",
        "type": "Python",
        "algorithm": "Classification",
        "artifacts": {
            "default": {
                "image": {
                    "executionType": "Python",
                    "packagingType": "docker"
                }
            }
        }
    }'
```

**応答** 

正常な応答は、エンジンの更新された詳細を含むペイロードを返します。

```json
{
    "id": "22f4166f-85ba-4130-a995-a2b8e1edde32",
    "name": "An updated name for this Engine",
    "description": "An updated description",
    "type": "Python",
    "algorithm": "Classification",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "displayName": "Jane Doe",
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-02T00:00:00.000Z",
    "artifacts": {
        "default": {
            "image": {
                "executionType": "Python",
                "packagingType": "docker"
            }
        }
    }
}
```

## エンジンの削除

エンジンを削除するには、リクエストパスにターゲットエンジンの ID を含む DELETE リクエストを実行します。エンジンを削除すると、そのエンジンを参照するすべての MLInstances が、それらの MLInstance に属する実験と実験の実行も含めて削除されます。

**API 形式**

```https
DELETE /engines/{ENGINE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{ENGINE_ID}` | 既存のエンジンの ID。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/engines/22f4166f-85ba-4130-a995-a2b8e1edde32 \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答** 

```json
{
    "title": "Success",
    "status": 200,
    "detail": "Engine deletion was successful"
}
```
