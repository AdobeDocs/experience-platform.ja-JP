---
keywords: Experience Platform;developer guide;endpoint;Data Science Workspace;popular topics
solution: Experience Platform
title: エンジン
topic: Developer guide
translation-type: tm+mt
source-git-commit: 19823c7cf0459e045366f0baae2bd8a98416154c

---


# エンジン

エンジンは、Data Science Workspaceの機械学習モデルの基礎です。 特定の問題を解決する機械学習アルゴリズム、フィーチャエンジニアリングを実行するフィーチャパイプライン、またはその両方が含まれます。

## Dockerレジストリの検索

>[!TIP]
>Docker URLがない場合は、 [Package source files into a recipe](../models-recipes/package-source-files-recipe.md) tutorialsを参照して、DockerホストURLの作成手順を確認してください。

パッケージ化されたレシピファイル（DockerホストのURL、ユーザー名、パスワードなど）をアップロードするには、Dockerレジストリ資格情報が必要です。 この情報は、次のGETリクエストを実行することで調べることができます。

**API形式**

```http
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

成功した応答は、Docker URL(`host`)、ユーザー名()、パスワード(`username`)を含むDockerレジストリの詳細を含むペイロードを返`password`します。

>[!NOTE]
>Dockerのパスワードは、更新するたびに `{ACCESS_TOKEN}` 変更されます。

```json
{
    "host": "docker_host.azurecr.io",
    "username": "00000000-0000-0000-0000-000000000000",
    "password": "password"
}
```

## Docker URLを使用したエンジンの作成 {#docker-image}

エンジンを作成するには、メタデータと、マルチパートフォームのDocker画像を参照するDocker URLを提供しながら、POSTリクエストを実行します。

**API形式**

```http
POST /engines
```

**リクエストPython/R**

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
                    "location": "{DOCKER_URL}",
                    "name": "An additional name for the Docker image",
                    "executionType": "Python"
                }
            }
        }
    }' 
```

| プロパティ | 説明 |
| --- | --- |
| `name` | エンジンの名前。 このエンジンに対応するレシピは、UIに表示されるこの値をレシピ名として継承します。 |
| `description` | エンジンのオプションの説明です。 このエンジンに対応するレシピは、UIに表示されるこの値をレシピの説明として継承します。 このプロパティが必要です。説明を指定しない場合は、値を空の文字列に設定します。 |
| `type` | エンジンの実行タイプ。 この値は、Dockerイメージの構築元の言語に対応し、「Python」、「R」、「Tensorflow」のいずれかを指定できます。 |
| `algorithm` | 機械学習アルゴリズムのタイプを指定する文字列。 サポートされるアルゴリズムのタイプには、「分類」、「回帰」または「カスタム」があります。 |
| `artifacts.default.image.location` | Docker URLによってリンクされたDockerイメージの場所。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。 この値は、Dockerイメージの構築元の言語に対応し、「Python」、「R」、「Tensorflow」のいずれかを指定できます。 |

**PySpark/Scalaのリクエスト**

PySparkレシピをリクエストする際、と `executionType` は「PySpark `type` 」となります。 Scalaレシピのリクエストを行う場合、と `executionType` は「Spark」 `type` になります。 次のScalaレシピの例では、Sparkを使用しています。

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
| `name` | エンジンの名前。 このエンジンに対応するレシピは、UIに表示されるこの値をレシピ名として継承します。 |
| `description` | エンジンのオプションの説明です。 このエンジンに対応するレシピは、UIに表示されるこの値をレシピの説明として継承します。 このプロパティが必要です。説明を指定しない場合は、値を空の文字列に設定します。 |
| `type` | エンジンの実行タイプ。 この値は、Dockerイメージを構築する言語に対応します。 値はSparkまたはPySparkに設定できます。 |
| `mlLibrary` | PySparkおよびScalaレシピ用のエンジンを作成する際に必要なフィールド。 このフィールドはに設定する必要がありま `databricks-spark`す。 |
| `artifacts.default.image.location` | Dockerイメージの場所。 Azure ACRまたはパブリック（未認証）Dockerhubのみがサポートされています。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。 この値は、Dockerイメージを構築する言語に対応します。 これは&quot;Spark&quot;か&quot;PySpark&quot;のどちらかです。 |

**応答**

成功した応答は、新たに作成されたエンジンの詳細を含むペイロードを、その一意の識別子(`id`)を返します。 次に、Pythonエンジンのレスポンス例を示します。 すべてのエンジンの応答は次の形式に従います。

```json
{
    "id": "{ENGINE_ID}",
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
                "location": "{DOCKER_URL}",
                "name": "An additional name for the Docker image",
                "executionType": "Python",
                "packagingType": "docker"
            }
        }
    }
}
```

## エンジンのリストの取得

エンジンのリストは、1回のGETリクエストを実行することで取得できます。 結果のフィルタリングに役立つように、リクエストパスでクエリパラメーターを指定できます。 使用可能なクエリのリストについては、アセット取得のための [クエリパラメータの付録の節を参照してください](./appendix.md#query)。

**API形式**

```http
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

成功した応答は、エンジンのリストとその詳細を返します。

```json
{
    "children": [
        {
            "id": "{ENGINE_ID}",
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
            "id": "{ENGINE_ID}",
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
            "id": "{ENGINE_ID}",
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

特定のエンジンの詳細を取得するには、目的のエンジンのIDをリクエストパスに含むGETリクエストを実行します。

**API形式**

```http
GET /engines/{ENGINE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{ENGINE_ID}` | 既存のエンジンのID。 |

**リクエスト**

```shell
curl -X GET \
    https://platform.adobe.io/data/sensei/engines/{ENGINE_ID} \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}'
```

**応答**

成功した応答は、目的のエンジンの詳細を含むペイロードを返します。

```json
{
    "id": "{ENGINE_ID}",
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
                "location": "wasbs://artifact-location.blob.core.windows.net/{ENGINE_ID}/default.egg",
                "name": "file.egg",
                "executionType": "PySpark",
                "packagingType": "egg"
            }
        }
    }
}
```

## エンジンの更新

既存のターゲットを変更および更新するには、要求パスにエンジンエンジンのIDを含むPUT要求を通じてそのプロパティを上書きし、更新されたプロパティを含むJSONペイロードを提供します。

>[!NOTE] このPUTリクエストを確実に成功させるため、最初にGETリクエストを実行し、IDでエンジンを取得す [ることをお勧めします](#retrieve-specific)。 次に、返されたJSONオブジェクトを変更および更新し、変更されたJSONオブジェクトの全体をPUT要求のペイロードとして適用します。

以下のサンプルAPI呼び出しを使用すると、これらのプロパティを最初に持つ間に、エンジンの名前と説明が更新されます。

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

**API形式**

```http
PUT /engines/{ENGINE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{ENGINE_ID}` | 既存のエンジンのID。 |

**リクエスト**

```shell
curl -X PUT \
    https://platform.adobe.io/data/sensei/engines/{ENGINE_ID} \
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

成功した応答は、エンジンの更新された詳細を含むペイロードを返します。

```json
{
    "id": "{ENGINE_ID}",
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

エンジンを削除するには、リクエストパスでターゲットエンジンのIDを指定しながらDELETEリクエストを実行します。 エンジンを削除すると、そのエンジンを参照するすべてのMLInstancesが、それらのMLInstanceに属する実験と実験の実行も含めて、カスケードで削除されます。

**API形式**

```http
DELETE /engines/{ENGINE_ID}
```

| パラメーター | 説明 |
| --- | --- |
| `{ENGINE_ID}` | 既存のエンジンのID。 |

**リクエスト**

```shell
curl -X DELETE \
    https://platform.adobe.io/data/sensei/engines/{ENGINE_ID} \
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

## 非推奨のリクエスト

>[!IMPORTANT]
>バイナリアーティファクトはサポートされなくなり、後で削除されるように設定されます。 新しいPySparkとScalaレシピは、Engineを作成するために [docker画像](#docker-image) の例に従う必要があります。

## バイナリアーティファクトを使用したエンジンの作成 — 廃止

ローカルまたはバイナリのアーティファクトを使用し `.jar` てエンジ `.egg` ンを作成するには、メタデータとアーティファクトのパスをマルチパートフォームに提供しながらPOSTリクエストを実行します。

**API形式**

```http
POST /engines
```

**リクエスト**

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
        "algorithm": "Classification",
        "type": "PySpark",
    }' \
    -F 'defaultArtifact=@path/to/binary/artifact/file.egg'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | エンジンの名前。 このエンジンに対応するレシピは、UIに表示されるこの値をレシピ名として継承します。 |
| `description` | エンジンのオプションの説明です。 このエンジンに対応するレシピは、UIに表示されるこの値をレシピの説明として継承します。 このプロパティが必要です。説明を指定しない場合は、値を空の文字列に設定します。 |
| `algorithm` | 機械学習アルゴリズムのタイプを指定する文字列。 サポートされるアルゴリズムのタイプには、「分類」、「回帰」または「カスタム」があります。 |
| `type` | エンジンの実行タイプ。 この値は、バイナリアーティファクトが構築される言語に対応し、&quot;PySpark&quot;または&quot;Spark&quot;のいずれかを指定できます。 |


**応答**

成功した応答は、新たに作成されたエンジンの詳細を含むペイロードを、その一意の識別子(`id`)を返します。

```json
{
    "id": "{ENGINE_ID}",
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
                "location": "wasbs://artifact-location.blob.core.windows.net/Engine_ID/default.egg",
                "name": "file.egg",
                "executionType": "PySpark",
                "packagingType": "egg"
            }
        }
    }
}
```

## バイナリアーティファクトを使用してフィーチャパイプラインエンジンを作成する — 廃止 {#create-a-feature-pipeline-engine-using-binary-artifacts}

>[!IMPORTANT]
>バイナリアーティファクトはサポートされなくなり、後で削除されるように設定されます。

ローカルまたはバイナリのアーティファクトを使用してフィーチャーパイプラインエ `.jar``.egg` ンジンを作成するには、メタデータとアーティファクトのパスをマルチパートフォームに提供しながらPOSTリクエストを実行します。 PySparkまたはSpark Engineは、コア数やメモリ量などの計算リソースを指定できます。 詳細は、PySparkとSparkのリソース設定に関する付録の [節を参照してください](./appendix.md#resource-config) 。

**API形式**

```http
POST /engines
```

**リクエスト**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/engines \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: multipart/form-data' \
    -F 'engine={
        "name": "Feature Pipeline Engine",
        "description": "A feature pipeline Engine",
        "algorithm":"fp",
        "type": "PySpark"
    }' \
    -F 'featurePipelineOverrideArtifact=@path/to/binary/artifact/feature_pipeline.egg' \
    -F 'defaultArtifact=@path/to/binary/artifact/feature_pipeline.egg'
```

| プロパティ | 説明 |
| --- | --- |
| `name` | エンジンの名前。 このエンジンに対応するレシピは、UIに表示されるこの値をレシピ名として継承します。 |
| `description` | エンジンのオプションの説明です。 このエンジンに対応するレシピは、UIに表示されるこの値をレシピの説明として継承します。 このプロパティが必要です。説明を指定しない場合は、値を空の文字列に設定します。 |
| `algorithm` | 機械学習アルゴリズムのタイプを指定する文字列。 この作成をフィーチャーパイプラインエンジンに指定するには、この値を「fp」に設定します。 |
| `type` | エンジンの実行タイプ。 この値は、バイナリアーティファクトが構築される言語に対応し、&quot;PySpark&quot;または&quot;Spark&quot;を指定できます。 |

**応答**

成功した応答は、新たに作成されたエンジンの詳細を含むペイロードを、その一意の識別子(`id`)を返します。

```json
{
    "id": "{ENGINE_ID}",
    "name": "Feature Pipeline Engine",
    "description": "A feature pipeline Engine",
    "type": "PySpark",
    "algorithm": "fp",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "Jane_Doe@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z",
    "artifacts": {
        "default": {
            "image": {
                "location": "wasbs://artifact-location.blob.core.windows.net/Engine_ID/default.egg",
                "name": "file.egg",
                "executionType": "PySpark",
                "packagingType": "egg"
            }
        }
    }
}
```
