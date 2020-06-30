---
keywords: Experience Platform;import packaged recipe;Data Science Workspace;popular topics
solution: Experience Platform
title: パッケージ化されたレシピの読み込み(API)
topic: Tutorial
translation-type: tm+mt
source-git-commit: 4b0f0dda97f044590f55eaf75a220f631f3313ee
workflow-type: tm+mt
source-wordcount: '955'
ht-degree: 2%

---


# パッケージ化されたレシピの読み込み(API)

このチュートリアルでは、を使用 [!DNL Sensei Machine Learning API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) して、ユーザインターフェイスで [「](../api/engines.md)エンジン」（レシピとも呼ばれる）を作成します。

最初に、APIとUI内の類似した要素を参照する際に、Adobe Experience Platformは異なる用語を [!DNL Data Science Workspace] 使用することに注意する必要があります。 APIの用語はこのチュートリアル全体で使用され、次の表に、相関する用語の概要を示します。

| UI用語 | API用語 |
| ---- | ---- |
| レシピ | [エンジン](../api/engines.md) |
| モデル | [MLInstance](../api/mlinstances.md) |
| トレーニングと評価 | [テスト](../api/experiments.md) |
| サービス | [MLService](../api/mlservices.md) |

エンジンには、機械学習アルゴリズムと特定の問題を解決するロジックが含まれています。 次の図は、のAPIワークフローを示すビジュアライゼーションを示してい [!DNL Data Science Workspace]ます。 このチュートリアルでは、機械学習モデルの脳であるエンジンの作成に焦点を当てます。

![](../images/models-recipes/import-package-api/engine_hierarchy_api.png)

## はじめに

このチュートリアルでは、Docker URLの形式でパッケージ化されたレシピファイルが必要です。 「 [Package source files into a Recipe](./package-source-files-recipe.md) tutorial」に従って、パッケージ化されたレシピファイルを作成するか、独自のレシピファイルを用意します。

- `{DOCKER_URL}`: インテリジェントサービスのDockerイメージへのURLアドレス。

このチュートリアルでは、APIの呼び出しを正しく行うために、 [Adobe Experience Platformへの認証のチュートリアル](../../tutorials/authentication.md) を完了している必要があり [!DNL Platform] ます。 次に示すように、認証チュートリアルで、すべての [!DNL Experience Platform] API呼び出しに必要な各ヘッダーの値を指定する

- `{ACCESS_TOKEN}`: 認証後に指定された特定のベアラトークン値。
- `{IMS_ORG}`: IMS組織の資格情報が固有のAdobe Experience Platform統合で見つかりました。
- `{API_KEY}`: 固有のAdobe Experience Platform統合で見つかった特定のAPIキー値。

## エンジンの作成

エンジンは、/enginesエンドポイントにPOSTリクエストを行うことで作成できます。 作成したエンジンは、パッケージ化されたレシピファイルの形式に基づいて設定されます。この形式は、APIリクエストの一部として含める必要があります。

### ドッカーURLを使用したエンジンの作成 {#create-an-engine-with-a-docker-url}

Dockerコンテナに格納されたパッケージ済みレシピファイルを含むエンジンを作成するには、パッケージ済みレシピファイルのDocker URLを指定する必要があります。

>[!CAUTION]
> またはRを使用している場合は、以下のリクエストを使用し [!DNL Python] てください。 PySparkまたはScalaを使用している場合は、Python/Rの例の下にあるPySpark/Scalaリクエストの例を使用してください。

**API形式**

```http
POST /engines
```

**Python/Rの要求**

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
| `engine.name` | エンジンの名前。 このエンジンに対応するレシピは、この値を継承して、 [!DNL Data Science Workspace] ユーザーインターフェイスに表示されます。 |
| `engine.description` | エンジンのオプションの説明。 このエンジンに対応するレシピは、この値を継承し、レシピの説明としてユー [!DNL Data Science Workspace] ザーインターフェイスに表示されます。 このプロパティを削除しないでください。説明を入力しない場合は、この値を空の文字列にします。 |
| `engine.type` | エンジンの実行タイプ。 この値は、Dockerイメージが開発される言語に対応します。 エンジンを作成するためにドッカーURL `type` を指定する場合、は、 `Python`、 `R`、、 `PySpark`、 `Spark` (Scala)、またはのいずれか `Tensorflow`です。 |
| `artifacts.default.image.location` | 君はここ `{DOCKER_URL}` に行く。 完全なドッカーURLは、次の構造を持ちます。 `your_docker_host.azurecr.io/docker_image_file:version` |
| `artifacts.default.image.name` | Dockerイメージファイルの追加名。 このプロパティを削除しないでください。Dockerイメージファイル名を追加しない場合は、この値を空の文字列にします。 |
| `artifacts.default.image.executionType` | このエンジンの実行タイプ。 この値は、Dockerイメージが開発される言語に対応します。 エンジンを作成するためにドッカーURL `executionType` を指定する場合、は、 `Python`、 `R`、、 `PySpark`、 `Spark` (Scala)、またはのいずれか `Tensorflow`です。 |

**PySparkの要求**

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
| `description` | エンジンのオプションの説明。 このエンジンに対応するレシピは、UIに表示されるこの値をレシピの説明として継承します。 このプロパティが必要です。説明を指定しない場合は、値を空の文字列に設定します。 |
| `type` | エンジンの実行タイプ。 この値は、Dockerイメージが&quot;PySpark&quot;上に構築される言語に対応します。 |
| `mlLibrary` | PySparkおよびScalaレシピ用のエンジンを作成する場合に必要なフィールドです。 |
| `artifacts.default.image.location` | Docker URLによってリンクされているDockerイメージの場所。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。 この値は、Dockerイメージが「Spark」に構築される言語に対応します。 |

**要求スケーラ**

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
| `description` | エンジンのオプションの説明。 このエンジンに対応するレシピは、UIに表示されるこの値をレシピの説明として継承します。 このプロパティが必要です。説明を指定しない場合は、値を空の文字列に設定します。 |
| `type` | エンジンの実行タイプ。 この値は、Dockerイメージが「Spark」に構築される言語に対応します。 |
| `mlLibrary` | PySparkおよびScalaレシピ用のエンジンを作成する場合に必要なフィールドです。 |
| `artifacts.default.image.location` | Docker URLによってリンクされているDockerイメージの場所。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。 この値は、Dockerイメージが「Spark」に構築される言語に対応します。 |

**応答**

成功した応答は、新たに作成されたエンジンの詳細(一意の識別子(`id`)を含むペイロードを返します。 次に、 [!DNL Python] エンジンに対する応答の例を示します。 キー `executionType` と `type` キーは、指定されたPOSTに基づいて変更されます。

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

正常な応答では、新しく作成されたエンジンに関する情報が含まれたJSONペイロードが表示されます。 この `id` キーは一意のエンジン識別子を表し、MLInstanceを作成するには、次のチュートリアルで必要となります。 次の手順に進む前に、エンジン識別子が保存されていることを確認します。

## 次の手順 {#next-steps}

APIを使用してエンジンを作成し、一意のエンジン識別子が応答本体の一部として取得された。 APIを使用してモデルを [作成、トレーニング、評価する方法を学習する際に、次のチュートリアルでこのエンジン識別子を使用できます](./train-evaluate-model-api.md)。