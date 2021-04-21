---
keywords: Experience Platform；パッケージレシピの読み込み；Data Science Workspace；人気のあるトピック；レシピ；api;senesie機械学習；エンジンの作成
solution: Experience Platform
title: Senesie Machine Learning APIを使用したパッケージ化されたレシピの読み込み
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは Sensei 機械学習 API を使用して、ユーザーインターフェイスの「レシピ」としても知られるエンジンを作成します。
exl-id: c8dde30b-5234-448d-a597-f1c8d32f23d4
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1007'
ht-degree: 62%

---

# Senesie Machine Learning APIを使用して、パッケージ化されたレシピを読み込みます

このチュートリアルでは、[[!DNL Sensei Machine Learning API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml)を使用して[エンジン](../api/engines.md)を作成します。これはユーザーインターフェイス内でもレシピとして知られています。

始める前に、Adobe Experience Platform[!DNL Data Science Workspace]は、APIとUI内の類似した要素を参照するために異なる用語を使用していることに注意してください。 API の用語はこのチュートリアル全体で使用され、次の表に、関連する用語の概要を示します。

| UI の用語 | API 用語 |
| ---- | ---- |
| レシピ | [エンジン](../api/engines.md) |
| モデル | [MLInstance](../api/mlinstances.md) |
| トレーニングと評価 | [実験](../api/experiments.md) |
| サービス | [MLService](../api/mlservices.md) |

エンジンには、特定の問題を解決するための機械学習アルゴリズムとロジックが含まれています。次の図は、[!DNL Data Science Workspace]のAPIワークフローを示すビジュアライゼーションを示しています。 このチュートリアルでは、機械学習モデルの頭脳であるエンジンの作成に焦点を当てます。

![](../images/models-recipes/import-package-api/engine_hierarchy_api.png)

## はじめに

このチュートリアルでは、Docker URLの形式でパッケージ化されたレシピファイルが必要です。 「[ソースファイルをレシピにパッケージ化する](./package-source-files-recipe.md)」チュートリアルに従ってパッケージ化されたレシピファイルを作成するか、独自のレシピファイルを提供します。

- `{DOCKER_URL}`：インテリジェントサービスの Docker イメージへの URL アドレス。

このチュートリアルでは、 API への呼び出しをおこなうために、[Adobe Experience Platform 認証チュートリアル](https://www.adobe.com/go/platform-api-authentication-en)を完了している必要があります。[!DNL Platform]次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- `{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。
- `{IMS_ORG}`：固有の Adobe Experience Platform 統合にある IMS 組織の資格情報。
- `{API_KEY}`：固有の Adobe Experience Platform 統合で見つかった特定の API キーの値。

## エンジンの作成

エンジンは、/enginesエンドポイントにPOSTリクエストを行うことで作成できます。 作成したエンジンは、パッケージ化されたレシピファイルの形式に基づいて設定されます。この形式は、APIリクエストの一部として含める必要があります。

### Docker URL を使用したエンジンの作成 {#create-an-engine-with-a-docker-url}

パッケージ化されたレシピファイルを Docker コンテナに格納してエンジンを作成するには、パッケージ化されたレシピファイルの Docker URL を指定する必要があります。

>[!CAUTION]
>
> [!DNL Python]またはRを使用している場合は、以下のリクエストを使用してください。 PySparkやScalaを使用している場合は、Python/Rの例の下にあるPySpark/Scalaリクエストの例を使ってください。

**API 形式**

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
| `engine.name` | エンジンの名前。このエンジンに対応するレシピは、[!DNL Data Science Workspace]ユーザーインターフェイスに表示されるこの値をレシピ名として継承します。 |
| `engine.description` | エンジンのオプションの説明。このエンジンに対応するレシピは、[!DNL Data Science Workspace]ユーザーインターフェイスに表示されるこの値をレシピの説明として継承します。 このプロパティを削除しないでください。説明を指定しない場合は、この値を空白の文字列にします。 |
| `engine.type` | エンジンの実行タイプ。この値は、Docker 画像の開発言語に対応します。エンジンを作成するためのDocker URLが指定された場合、`type`は`Python`、`R`、`PySpark`、`Spark`(Scala)、または`Tensorflow`です。 |
| `artifacts.default.image.location` | `{DOCKER_URL}`はここに来ます。 完全なドッカーURLは、次の構造を持ちます。`your_docker_host.azurecr.io/docker_image_file:version` |
| `artifacts.default.image.name` | Dockerイメージファイルの追加名。 このプロパティを削除しないでください。Docker 画像ファイル名を指定しない場合は、この値を空白の文字列にします。 |
| `artifacts.default.image.executionType` | このエンジンの実行タイプ。 この値は、Docker 画像の開発言語に対応します。エンジンを作成するためのDocker URLが指定された場合、`executionType`は`Python`、`R`、`PySpark`、`Spark`(Scala)、または`Tensorflow`です。 |

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
| `name` | エンジンの名前。このエンジンに対応するレシピは、UI に表示されるこの値をレシピ名として継承します。 |
| `description` | エンジンのオプションの説明。このエンジンに対応するレシピは、UI に表示されるこの値をレシピの説明として継承します。このプロパティが必要です。説明を指定しない場合は、値を空の文字列に設定します。 |
| `type` | エンジンの実行タイプ。この値は、Dockerイメージが&quot;PySpark&quot;上に構築される言語に対応します。 |
| `mlLibrary` | PySparkおよびScalaレシピ用のエンジンを作成する場合に必要なフィールドです。 |
| `artifacts.default.image.location` | Docker URL によってリンクされた Docker イメージの場所。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。この値は、Dockerイメージが「Spark」に構築される言語に対応します。 |

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
| `name` | エンジンの名前。このエンジンに対応するレシピは、UI に表示されるこの値をレシピ名として継承します。 |
| `description` | エンジンのオプションの説明。このエンジンに対応するレシピは、UI に表示されるこの値をレシピの説明として継承します。このプロパティが必要です。説明を指定しない場合は、値を空の文字列に設定します。 |
| `type` | エンジンの実行タイプ。この値は、Dockerイメージが「Spark」に構築される言語に対応します。 |
| `mlLibrary` | PySparkおよびScalaレシピ用のエンジンを作成する場合に必要なフィールドです。 |
| `artifacts.default.image.location` | Docker URL によってリンクされた Docker イメージの場所。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。この値は、Dockerイメージが「Spark」に構築される言語に対応します。 |

**応答**

正常な応答は、新たに作成されたエンジンの一意の ID（`id`）を含む詳細を含むペイロードを返します。次の例は、[!DNL Python]エンジンに対する応答です。 `executionType`キーと`type`キーは、指定されたPOSTに基づいて変更されます。

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

成功応答は、新しく作成されたエンジンに関する情報を含む JSON ペイロードが表示されます。`id` キーは、一意のエンジン識別子を表し、次のチュートリアルで MLInstance を作成するために必要となります。次の手順に進む前に、エンジン識別子が保存されていることを確認します。

## 次の手順  {#next-steps}

API を使用してエンジンを作成し、応答本文の一部として一意のエンジン識別子を取得しました。次のチュートリアルでは、[API を使用してモデルの作成、トレーニング、評価をおこなう](./train-evaluate-model-api.md)方法について学習しながら、このエンジン識別子を使用できます。
