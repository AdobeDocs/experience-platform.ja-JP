---
keywords: Experience Platform;import packaged recipe;Data Science Workspace;popular topics
solution: Experience Platform
title: パッケージ化されたレシピ(API)の読み込み
topic: Tutorial
translation-type: tm+mt
source-git-commit: ebf7c883ce89fdf8b0d468ab21d1c3a1ba8aca06

---


# パッケージ化されたレシピ(API)の読み込み

このチュートリアルでは [Sensei Machine Learning API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) (Senesie Machine Learning API [)を使用して](../api/engines.md)、ユーザーインターフェイスで「レシピ」とも呼ばれるエンジンを作成します。

開始する前に、Adobe Experience Platform Data Science Workspaceでは、APIとUI内の類似した要素を参照するために異なる用語を使用することに注意してください。 APIの用語はこのチュートリアル全体で使用され、次の表に、関連する用語の概要を示します。

| UI用語 | API用語 |
| ---- | ---- |
| レシピ | [エンジン](../api/engines.md) |
| モデル | [MLInstance](../api/mlinstances.md) |
| トレーニングと評価 | [実験](../api/experiments.md) |
| サービス | [MLService](../api/mlservices.md) |

エンジンには、特定の問題を解決するための機械学習アルゴリズムとロジックが含まれています。 次の図は、Data Science WorkspaceのAPIワークフローを示すビジュアライゼーションを示しています。 このチュートリアルでは、機械学習モデルの頭脳であるエンジンの作成に焦点を当てます。

![](../images/models-recipes/import-package-api/engine_hierarchy_api.png)

## はじめに

このチュートリアルでは、バイナリアーティファクトまたはDocker URLの形式でパッケージ化されたレシピファイルが必要です。 パッケージ化さ [れたレシピファイルを作成するか、独自のレシピファイルを提供するには](./package-source-files-recipe.md) 、「ソースファイルをレシピにパッケージ化」チュートリアルに従います。

- バイナリアーティファクト（非推奨）:バイナリアーティファクト( JAR、EGG)を使用してエンジンを作成します。
- `{DOCKER_URL}`:インテリジェントサービスのDockerイメージへのURLアドレス。

このチュートリアルでは、プラットフォームAPIを正しく呼び出す [ために、Adobe Experience Platformへの認証のチュートリアルを完了している必要があります](../../tutorials/authentication.md) 。 次に示すように、認証チュートリアルで、すべてのエクスペリエンスプラットフォームAPI呼び出しで必要な各ヘッダーの値を入力します。

- `{ACCESS_TOKEN}`:認証後に指定された特定のベアラトークン値。
- `{IMS_ORG}`:IMS組織の資格情報が、お使いの一意のAdobe Experience Platform統合で見つかりました。
- `{API_KEY}`:固有のAdobe Experience Platform統合で見つかった特定のAPIキーの値。

## エンジンの作成

APIリクエストの一部として含めるパッケージ化されたレシピファイルの形式に応じて、次の2つの方法のいずれかを使用してエンジンが作成されます。

- [ドッカーURLを使用したエンジンの作成](#create-an-engine-with-a-docker-url)
- [バイナリアーティファクト（非推奨）を使用したエンジンの作成](#create-an-engine-with-a-binary-artifact-deprecated)

### ドッカーURLを使用したエンジンの作成

パッケージ化されたレシピファイルをDockerコンテナに格納してエンジンを作成するには、パッケージ化されたレシピファイルのDocker URLを指定する必要があります。

>[!CAUTION]
> PythonまたはRを使用している場合は、以下のリクエストを使用します。 PySparkやScalaを使用している場合は、Python/Rの例の下にあるPySpark/Scalaリクエストの例を使用します。

**API形式**

```http
POST /engines
```

**リクエストPython/R**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/engines \
    -H 'Authorization: {ACCESS_TOKEN}' \
    -H 'X-API-KEY: {API_KEY}' \
    -H 'content-type: multipart/form-data' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H `x-sandbox-name: {SANDBOX_NAME}` \
    -F 'engine={
        "name": "Retail Sales Engine Python",
        "description": "A description for Retail Sales Engine, this Engines execution type is Python",
        "type": "Python"
        "artifacts": {
            "default": {
                "image": {
                    "location": "{DOCKER_URL}",
                    "name": "retail_sales_python",
                    "executionType": "Python"
                }
            }
        }
    }' 
```

| プロパティ | 説明 |
| -------  | ----------- |
| `engine.name` | エンジンの名前。 このエンジンに対応するレシピは、Data Science Workspaceユーザーインターフェイスに表示されるこの値をレシピ名として継承します。 |
| `engine.description` | エンジンのオプションの説明です。 このエンジンに対応するレシピは、Data Science Workspaceユーザーインターフェイスに表示されるこの値をレシピの説明として継承します。 このプロパティを削除しないでください。説明を指定しない場合は、この値を空の文字列にします。 |
| `engine.type` | エンジンの実行タイプ。 この値は、Dockerイメージが開発される言語に対応します。 エンジンを作成するためのドッカーURLが指定され `type` た場合、は、、、、、、、、、、、、、のいず `Python`れか `R``PySpark``Spark``Tensorflow`になります。(Scala)、または、 |
| `artifacts.default.image.location` | 君はこ `{DOCKER_URL}` こに行く。 完全なドッカーURLの構造は次のとおりです。 `your_docker_host.azurecr.io/docker_image_file:version` |
| `artifacts.default.image.name` | Dockerイメージファイルの追加名。 このプロパティを削除しないでください。Dockerイメージファイル名を追加しない場合は、この値を空の文字列にします。 |
| `artifacts.default.image.executionType` | このエンジンの実行タイプ。 この値は、Dockerイメージが開発される言語に対応します。 エンジンを作成するためのドッカーURLが指定され `executionType` た場合、は、、、、、、、、、、、、、のいず `Python`れか `R``PySpark``Spark``Tensorflow`になります。(Scala)、または、 |

**PySparkのリクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/sensei/engines \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -H 'x-sandbox-name: {SANDBOX_NAME}' \
    -H 'content-type: multipart/form-data' \
    -F 'engine={
    "name": "PySpark retail sales recipe",
    "description": "A description for this Engine",
    "type": "PySpark",
    "mlLibrary":"databricks-spark",
    "artifacts": {
        "default": {
            "image": {
                "name": "modelspark",
                "executionType": "PySpark",
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
| `type` | エンジンの実行タイプ。 この値は、Dockerイメージが「PySpark」上に構築される言語に対応します。 |
| `mlLibrary` | PySparkおよびScalaレシピ用のエンジンを作成する際に必要なフィールド。 |
| `artifacts.default.image.location` | Docker URLによってリンクされたDockerイメージの場所。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。 この値は、Dockerイメージが「Spark」に構築される言語に対応します。 |

**Request Scala**

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
| `type` | エンジンの実行タイプ。 この値は、Dockerイメージが「Spark」に構築される言語に対応します。 |
| `mlLibrary` | PySparkおよびScalaレシピ用のエンジンを作成する際に必要なフィールド。 |
| `artifacts.default.image.location` | Docker URLによってリンクされたDockerイメージの場所。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。 この値は、Dockerイメージが「Spark」に構築される言語に対応します。 |

**応答**

成功した応答は、新たに作成されたエンジンの詳細を含むペイロードを、その一意の識別子(`id`)を返します。 次に、Pythonエンジンのレスポンス例を示します。 キーとキ `executionType` ーは、指 `type` 定されたPOSTに基づいて変更されます。

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

成功した応答には、新しく作成されたエンジンに関する情報を含むJSONペイロードが表示されます。 キーは `id` 一意のエンジン識別子を表し、MLInstanceを作成するための次のチュートリアルで必要となります。 次の手順に進む前に、エンジン識別子が保存されていることを確認します。

## 次の手順

APIを使用してエンジンを作成し、応答本文の一部として一意のエンジン識別子を取得した。 APIを使用してモデルの作成、トレーニング、評価を行う方法について学習する際に、 [次のチュートリアルでこのエンジン識別子を使用できます](./train-evaluate-model-api.md)。

### バイナリアーティファクト（非推奨）を使用したエンジンの作成

<!-- Will need to remove binary artifact documentation once the old flags are turned off -->

>[!CAUTION]
> バイナリアーティファクトは、古いPySparkとSparkのレシピで使用されます。 Data Science Workspaceで、すべてのレシピのDocker URLがサポートされるようになりました。 この更新により、すべてのエンジンはDocker URLを使用して作成されます。 このドキュメントの [Docker URLの節を参照](#create-an-engine-with-a-docker-url) 。 バイナリアーティファクトは、以降のリリースで削除されるように設定されます。

ローカルパッケージまたはバイナリアーティファクトを使用してエ `.jar` ンジンを作成す `.egg` るには、ローカルファイルシステム内のバイナリアーティファクトファイルへの絶対パスを指定する必要があります。 ターミナル環境内のバイナリアーティファクトを含むディレクトリに移動し、絶対パスに対して `pwd` Unixコマンドを実行します。

次の呼び出しは、バイナリアーティファクトを持つエンジンを作成します。

**API形式**

```http
POST /engines
```

**リクエスト**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/engines \
    -H 'Authorization: {ACCESS_TOKEN}' \
    -H 'X-API-KEY: {API_KEY}' \
    -H 'content-type: multipart/form-data' \
    -H 'x-gw-ims-org-id: {IMS_ORG}' \
    -F 'engine={
        "name": "Retail Sales Engine PySpark",
        "description": "A description for Retail Sales Engine, this Engines execution type is PySpark",
        "type": "PySpark"
    }' \
    -F 'defaultArtifact=@path/to/binary/artifact/file/pysparkretailapp-0.1.0-py3.7.egg'
```

| プロパティ | 説明 |
| -------  | ----------- |
| `engine.name` | エンジンの名前。 このエンジンに対応するレシピは、Data Science Workspaceユーザーインターフェイスに表示されるこの値をレシピ名として継承します。 |
| `engine.description` | エンジンのオプションの説明です。 このエンジンに対応するレシピは、Data Science Workspaceユーザーインターフェイスに表示されるこの値をレシピの説明として継承します。 このプロパティを削除しないでください。説明を指定しない場合は、この値を空の文字列にします。 |
| `engine.type` | エンジンの実行タイプ。 この値は、バイナリアーティファクトが開発された言語に対応します。 バイナリアーティファクトをアップロードしてエンジンを作成する場合、 `type` またはのい `Spark` ずれかになりま `PySpark`す。 |
| `defaultArtifact` | エンジンの作成に使用されるバイナリアーティファクトファイルの絶対パスです。 ファイルパスの前に必 `@` ずを含めてください。 |

**応答**

```JSON
{
    "id": "00000000-1111-2222-3333-abcdefghijkl",
    "name": "Retail Sales Engine PySpark",
    "description": "A description for Retail Sales Engine, this Engines execution type is PySpark",
    "type": "PySpark",
    "created": "2019-01-01T00:00:00.000Z",
    "createdBy": {
        "userId": "your_user_id@AdobeID"
    },
    "updated": "2019-01-01T00:00:00.000Z",
    "artifacts": {
        "default": {
            "image": {
                "location": "wasbs://some-storage-location.net/some-path/your-uploaded-binary-artifact.egg",
                "name": "pysparkretailapp-0.1.0-py3.7.egg",
                "executionType": "PySpark",
                "packagingType": "egg"
            }
        }
    }
}
```

成功した応答には、新しく作成されたエンジンに関する情報を含むJSONペイロードが表示されます。 キーは `id` 一意のエンジン識別子を表し、MLInstanceを作成するための次のチュートリアルで必要となります。 次の手順に進む前に、エンジン識別子が保存されていることを [確認します](#next-steps)。