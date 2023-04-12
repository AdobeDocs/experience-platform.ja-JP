---
keywords: Experience Platform；パッケージ化されたレシピのインポート；Data Science Workspace；人気の高いトピック；レシピ；api;sensei 機械学習；エンジンの作成
solution: Experience Platform
title: パッケージ化されたレシピの読み込み (Sensei Machine Learning API を使用 )
type: Tutorial
description: このチュートリアルでは Sensei 機械学習 API を使用して、ユーザーインターフェイスの「レシピ」としても知られるエンジンを作成します。
exl-id: c8dde30b-5234-448d-a597-f1c8d32f23d4
source-git-commit: fcd44aef026c1049ccdfe5896e6199d32b4d1114
workflow-type: tm+mt
source-wordcount: '1006'
ht-degree: 61%

---

# パッケージ化されたレシピの読み込み (Sensei Machine Learning API)

このチュートリアルでは、 [[!DNL Sensei Machine Learning API]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/sensei-ml-api.yaml) を作成するには、 [エンジン](../api/engines.md)ユーザーインターフェイスの「レシピ」とも呼ばれます。

使い始める前に、Adobe Experience Platform [!DNL Data Science Workspace] は、API と UI 内の類似した要素を示すために異なる用語を使用します。 API の用語はこのチュートリアル全体で使用され、次の表に、関連する用語の概要を示します。

| UI の用語 | API 用語 |
| ---- | ---- |
| レシピ | [エンジン](../api/engines.md) |
| モデル | [MLInstance](../api/mlinstances.md) |
| トレーニングと評価 | [実験](../api/experiments.md) |
| サービス | [MLService](../api/mlservices.md) |

エンジンには、特定の問題を解決するための機械学習アルゴリズムとロジックが含まれています。次の図は、 [!DNL Data Science Workspace]. このチュートリアルでは、機械学習モデルの頭脳であるエンジンの作成に焦点を当てます。

![](../images/models-recipes/import-package-api/engine_hierarchy_api.png)

## はじめに

このチュートリアルでは、Docker URL の形式でパッケージ化されたレシピファイルが必要です。 「[ソースファイルをレシピにパッケージ化する](./package-source-files-recipe.md)」チュートリアルに従ってパッケージ化されたレシピファイルを作成するか、独自のレシピファイルを提供します。

- `{DOCKER_URL}`：インテリジェントサービスの Docker イメージへの URL アドレス。

このチュートリアルでは、 API への呼び出しをおこなうために、[Adobe Experience Platform 認証チュートリアル](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)を完了している必要があります。[!DNL Platform]次に示すように、すべての [!DNL Experience Platform] API 呼び出しに必要な各ヘッダーの値は認証チュートリアルで説明されています。

- `{ACCESS_TOKEN}`：認証後に提供される特定の Bearer トークン値。
- `{ORG_ID}`:固有のAdobe Experience Platform統合で見つかった組織の資格情報。
- `{API_KEY}`：固有の Adobe Experience Platform 統合で見つかった特定の API キーの値。

## エンジンの作成

エンジンは、/engines エンドポイントにPOSTリクエストを送信することで作成できます。 作成されたエンジンは、API リクエストの一部として含める必要がある、パッケージ化されたレシピファイルの形式に基づいて設定されます。

### Docker URL を使用したエンジンの作成 {#create-an-engine-with-a-docker-url}

パッケージ化されたレシピファイルを Docker コンテナに格納してエンジンを作成するには、パッケージ化されたレシピファイルの Docker URL を指定する必要があります。

>[!CAUTION]
>
> 次を使用する場合： [!DNL Python] または R は以下のリクエストを使用します。 PySpark または Scala を使用している場合は、Python/R の例の下にある PySpark/Scala リクエストの例を使用します。

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
| `engine.name` | エンジンの名前。このエンジンに対応するレシピは、に表示されるこの値を継承します [!DNL Data Science Workspace] レシピ名としてのユーザーインターフェイス。 |
| `engine.description` | エンジンのオプションの説明。このエンジンに対応するレシピは、に表示されるこの値を継承します [!DNL Data Science Workspace] ユーザーインターフェイスをレシピの説明として使用します。 このプロパティを削除しないでください。説明を指定しない場合は、この値を空白の文字列にします。 |
| `engine.type` | エンジンの実行タイプ。この値は、Docker 画像の開発言語に対応します。エンジンを作成するための Docker URL が提供された場合、 `type` 次のいずれか `Python`, `R`, `PySpark`, `Spark` (Scala) または `Tensorflow`. |
| `artifacts.default.image.location` | お使いの `{DOCKER_URL}` ここに行く 完全な Docker URL の構造は次のとおりです。 `your_docker_host.azurecr.io/docker_image_file:version` |
| `artifacts.default.image.name` | Docker イメージファイルの追加名。 このプロパティを削除しないでください。Docker 画像ファイル名を指定しない場合は、この値を空白の文字列にします。 |
| `artifacts.default.image.executionType` | このエンジンの実行タイプ。 この値は、Docker 画像の開発言語に対応します。エンジンを作成するための Docker URL が提供された場合、 `executionType` 次のいずれか `Python`, `R`, `PySpark`, `Spark` (Scala) または `Tensorflow`. |

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
| `type` | エンジンの実行タイプ。この値は、Docker イメージが「PySpark」を基に構築される言語に対応します。 |
| `mlLibrary` | PySpark と Scala のレシピ用のエンジンを作成する際に必要なフィールド。 |
| `artifacts.default.image.location` | Docker URL によってリンクされた Docker イメージの場所。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。この値は、Docker イメージが「Spark」を基に構築される言語に対応します。 |

**Request Scala**

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
| `type` | エンジンの実行タイプ。この値は、Docker イメージが「Spark」を基に構築される言語に対応します。 |
| `mlLibrary` | PySpark と Scala のレシピ用のエンジンを作成する際に必要なフィールド。 |
| `artifacts.default.image.location` | Docker URL によってリンクされた Docker イメージの場所。 |
| `artifacts.default.image.executionType` | エンジンの実行タイプ。この値は、Docker イメージが「Spark」を基に構築される言語に対応します。 |

**応答**

正常な応答は、新たに作成されたエンジンの一意の ID（`id`）を含む詳細を含むペイロードを返します。次のレスポンスの例は、 [!DNL Python] エンジン。 この `executionType` および `type` キーは、指定されたPOSTに基づいて変更されます。

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
