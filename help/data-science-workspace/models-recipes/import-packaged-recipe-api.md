---
keywords: Experience Platform；パッケージ化されたレシピの読み込み；Data Science Workspace；人気のトピック；レシピ；api;sensei machine learning;create engine
solution: Experience Platform
title: Sensei Machine Learning API を使用したパッケージ化されたレシピの読み込み
type: Tutorial
description: このチュートリアルでは、Sensei機械学習 API を使用して、エンジン（ユーザーインターフェイスのレシピとも呼ばれます）を作成します。
exl-id: c8dde30b-5234-448d-a597-f1c8d32f23d4
source-git-commit: b48c24ac032cbf785a26a86b50a669d7fcae5d97
workflow-type: tm+mt
source-wordcount: '1018'
ht-degree: 50%

---

# Sensei Machine Learning API を使用したパッケージ化されたレシピの読み込み

>[!NOTE]
>
>Data Science Workspaceは購入できなくなりました。
>
>このドキュメントは、Data Science Workspaceの以前の使用権限を持つ既存のお客様を対象としています。

このチュートリアルでは、[[!DNL Sensei Machine Learning API]](https://developer.adobe.com/experience-platform-apis/references/sensei-machine-learning/) を使用して、ユーザーインターフェイスのレシピとも呼ばれる [Engine](../api/engines.md) を作成します。

始める前に、Adobe Experience Platform [!DNL Data Science Workspace] では、API と UI 内で類似の要素を参照するために異なる用語を使用していることに注意してください。 API の用語はこのチュートリアル全体で使用され、次の表に、関連する用語の概要を示します。

| UI の用語 | API 用語 |
| ---- | ---- |
| レシピ | [エンジン](../api/engines.md) |
| モデル | [MLInstance](../api/mlinstances.md) |
| トレーニングと評価 | [実験](../api/experiments.md) |
| サービス | [MLService](../api/mlservices.md) |

エンジンには、特定の問題を解決するための機械学習アルゴリズムとロジックが含まれています。次の図は、[!DNL Data Science Workspace] の API ワークフローを示すビジュアライゼーションを示しています。 このチュートリアルでは、機械学習モデルの頭脳であるエンジンの作成に焦点を当てます。

![](../images/models-recipes/import-package-api/engine_hierarchy_api.png)

## はじめに

このチュートリアルでは、Docker URL の形式のパッケージ化されたレシピファイルが必要です。 「[ソースファイルをレシピにパッケージ化する](./package-source-files-recipe.md)」チュートリアルに従ってパッケージ化されたレシピファイルを作成するか、独自のレシピファイルを提供します。

- `{DOCKER_URL}`：インテリジェントサービスの Docker イメージへの URL アドレス。

このチュートリアルでは、[!DNL Experience Platform] API を正しく呼び出すために、[Adobe Experience Platformへの認証 ](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja) チュートリアルを完了している必要があります。 次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- `{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。
- `{ORG_ID}`：組織の資格情報が、一意のAdobe Experience Platform統合で見つかりました。
- `{API_KEY}`：固有の Adobe Experience Platform 統合で見つかった特定の API キーの値。

## エンジンの作成

エンジンは、/engines エンドポイントに POST リクエストを実行することで作成できます。 作成されたエンジンは、API リクエストの一部として含める必要がある、パッケージ化されたレシピファイルの形式に基づいて設定されます。

### Docker URL を使用したエンジンの作成 {#create-an-engine-with-a-docker-url}

パッケージ化されたレシピファイルを Docker コンテナに格納してエンジンを作成するには、パッケージ化されたレシピファイルの Docker URL を指定する必要があります。

>[!CAUTION]
>
> [!DNL Python] または R を使用している場合は、以下のリクエストを使用します。 PySpark または Scala を使用している場合は、Python/R の例の下にある PySpark/Scala リクエストの例を使用してください。

**API 形式**

```http
POST /engines
```

**Python/R をリクエスト**

```shell
curl -X POST \
    https://platform.adobe.io/data/sensei/engines \
    -H 'Authorization: {ACCESS_TOKEN}' \
    -H 'X-API-KEY: {API_KEY}' \
    -H 'content-type: multipart/form-data' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `engine.name` | エンジンの名前。このエンジンに対応するレシピは、この値を継承して、ユーザーインターフェイスにレシピ [!DNL Data Science Workspace] 名前として表示します。 |
| `engine.description` | エンジンのオプションの説明。このエンジンに対応するレシピは、この値を継承して、ユーザーインターフェイス [!DNL Data Science Workspace] レシピの説明として表示します。 このプロパティを削除しないでください。説明を指定しない場合は、この値を空白の文字列にします。 |
| `engine.type` | エンジンの実行タイプ。この値は、Docker イメージが開発される言語に対応しています。 エンジンを作成するために Docker URL が指定され `type` 場合、`Python`、`R`、`PySpark`、`Spark` （Scala）、`Tensorflow` のいずれかです。 |
| `artifacts.default.image.location` | あなたの `{DOCKER_URL}` はここに行きます。 完全な Docker URL は、次の構造を持ちます。`your_docker_host.azurecr.io/docker_image_file:version` |
| `artifacts.default.image.name` | Docker イメージファイルの追加名。 このプロパティを削除しないでください。Docker 画像ファイル名を指定しない場合は、この値を空白の文字列にします。 |
| `artifacts.default.image.executionType` | このエンジンの実行タイプ。 この値は、Docker イメージが開発される言語に対応しています。 エンジンを作成するために Docker URL が指定され `executionType` 場合、`Python`、`R`、`PySpark`、`Spark` （Scala）、`Tensorflow` のいずれかです。 |

**PySpark をリクエスト**

```shell
curl -X POST \
  https://platform.adobe.io/data/sensei/engines \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `type` | エンジンの実行タイプ。この値は、「PySpark」に基づいて Docker イメージが構築されている言語に対応します。 |
| `mlLibrary` | PySpark と Scala のレシピ用のエンジンを作成する際に必要なフィールド。 |
| `artifacts.default.image.location` | Docker URL によってリンクされた Docker イメージの場所。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。この値は、「Spark」に基づいて Docker イメージが構築される言語に対応します。 |

**Scala を要求**

```shell
curl -X POST \
  https://platform.adobe.io/data/sensei/engines \
    -H 'Authorization: Bearer {ACCESS_TOKEN}' \
    -H 'x-api-key: {API_KEY}' \
    -H 'x-gw-ims-org-id: {ORG_ID}' \
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
| `type` | エンジンの実行タイプ。この値は、「Spark」に基づいて Docker イメージが構築される言語に対応します。 |
| `mlLibrary` | PySpark と Scala のレシピ用のエンジンを作成する際に必要なフィールド。 |
| `artifacts.default.image.location` | Docker URL によってリンクされた Docker イメージの場所。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。この値は、「Spark」に基づいて Docker イメージが構築される言語に対応します。 |

**応答**

リクエストが成功した場合は、一意の ID （`id`）を含む、新しく作成されたエンジンの詳細を含むペイロードが返されます。 次の応答の例は、[!DNL Python] Engine 用です。 `executionType` キーと `type` キーは、指定した POST に応じて変わります。

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

## 次の手順 {#next-steps}

API を使用してエンジンを作成し、応答本文の一部として一意のエンジン識別子を取得しました。次のチュートリアルでは、[API を使用してモデルの作成、トレーニング、評価をおこなう](./train-evaluate-model-api.md)方法について学習しながら、このエンジン識別子を使用できます。
