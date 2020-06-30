---
keywords: Experience Platform;retail sales recipe;Data Science Workspace;popular topics
solution: Experience Platform
title: 小売売上スキーマとデータセットの作成
topic: Tutorial
translation-type: tm+mt
source-git-commit: 4b0f0dda97f044590f55eaf75a220f631f3313ee
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 1%

---


# 小売売上スキーマとデータセットの作成

このチュートリアルでは、他のすべての [!DNL Adobe Experience Platform] チュートリアルで必要な前提条件とアセットについて説明し [!DNL Data Science Workspace] ます。 完了すると、お客様およびIMS組織のメンバーが、小売販売スキーマとデータセットを使用でき [!DNL Experience Platform]ます。

## はじめに

このチュートリアルを開始する前に、次の前提条件を満たす必要があります。
- へのアクセス [!DNL Adobe Experience Platform]。 でIMS組織にアクセスできない場合は、先に進む前にシステム管理者にお問い合わせ [!DNL Experience Platform]ください。
- API呼び出しを行うための認証。 [!DNL Experience Platform] このチュートリアルを正常に完了するには、 [Authenticate and accessAdobe Experience PlatformAPIs](../../tutorials/authentication.md) （認証およびアクセスのAPI）チュートリアルを完了し、次の値を取得します。
   - 認証: `{ACCESS_TOKEN}`
   - x-api-key: `{API_KEY}`
   - x-gw-ims-org-id: `{IMS_ORG}`
   - Client secret: `{CLIENT_SECRET}`
   - クライアント証明書： `{PRIVATE_KEY}`
- 小 [売販売レシピ用のサンプルデータおよびソースファイル](../pre-built-recipes/retail-sales.md)。 このチュートリアルと他の [!DNL Data Science Workspace] チュートリアルに必要なアセットを [Adobe Public Gitリポジトリからダウンロードします](https://github.com/adobe/experience-platform-dsw-reference/)。
- [Python >= 2.7](https://www.python.org/downloads/) と次の [!DNL Python] パッケージ：
   - [パイプ](https://pypi.org/project/pip/)
   - [PyYAML](https://pyyaml.org/)
   - [ディクター](https://pypi.org/project/dictor/)
   - [JWT](https://pypi.org/project/jwt/)
- このチュートリアルで使用する次の概念について、実際に理解しておく必要があります。
   - [!DNL Experience Data Model (XDM)](../../xdm/home.md)
   - [スキーマ組成の基本](../../xdm/schema/field-dictionary.md)

## 小売売上スキーマとデータセットの作成

小売販売スキーマとデータセットは、提供されたブートストラップスクリプトを使用して自動的に作成されます。 次の手順を順番に実行します。

### ファイルの設定

1. チュートリアル [!DNL Experience Platform] リソースパッケージ内で、ディレクトリに移動し `bootstrap`、適切なテキストエディタ `config.yaml` ーを使用して開きます。
2. この `Enterprise` セクションで、次の値を入力します。

   ```yaml
   Enterprise:
       api_key: {API_KEY}
       org_id: {IMS_ORG}
       tech_acct: {technical_account_id}
       client_secret: {CLIENT_SECRET}
       priv_key_filename: {PRIVATE_KEY}
   ```

3. 次の例のように、 `Platform` 節の下にある値を編集します。

   ```yaml
   Platform:
       platform_gateway: https://platform.adobe.io
       ims_token: {ACCESS_TOKEN}
       ingest_data: "True"
       build_recipe_artifacts: "False"
       kernel_type: Python
   ```

   - `platform_gateway` : API呼び出しのベースパス。 この値は変更しないでください。
   - `ims_token` : 君はここ `{ACCESS_TOKEN}` に行く。
   - `ingest_data` : このチュートリアルでは、小売販売スキーマとデータセット `"True"` を作成するために、この値をに設定します。 の値は、スキーマ `"False"` のみを作成します。
   - `build_recipe_artifacts` : このチュートリアルでは、スクリプトでレシピアーティファクトが生成されないように、この値 `"False"` をに設定します。
   - `kernel_type` : レシピアーティファクトの実行タイプ。 この値は、がに設定されている場合 `Python` のままにし `build_recipe_artifacts``"False"`、それ以外の場合は、正しい実行タイプを指定します。

4. この `Titles` セクションで、Retail Salesサンプルデータに関する以下の情報を適切に指定し、編集後にファイルを保存して閉じます。 以下に例を示します。

   ```yaml
   Titles:
       input_class_title: retail_sales_input_class
       input_mixin_title: retail_sales_input_mixin
       input_mixin_definition_title: retail_sales_input_mixin_definition
       input_schema_title: retail_sales_input_schema
       input_dataset_title: retail_sales_input_dataset
       file_replace_tenant_id: DSWRetailSalesForXDM0.9.9.9.json
       file_with_tenant_id: DSWRetailSales_with_tenant_id.json
       is_output_schema_different: "True"
       output_mixin_title: retail_sales_output_mixin
       output_mixin_definition_title: retail_sales_output_mixin_definition
       output_schema_title: retail_sales_output_title
       output_dataset_title: retail_sales_output_dataset
   ```

### ブートストラップスクリプトの実行

1. ターミナルアプリケーションを開き、チュートリアルリソースディレクトリに移動し [!DNL Experience Platform] ます。
2. ディレクトリを現在の作業パスとして設定し、次のコマンドを入力して `bootstrap` スクリプ `bootstrap.py`[!DNL Python] トを実行します。

   ```bash
   python bootstrap.py
   ```

   > [!NOTE] スクリプトの完了には数分かかる場合があります。

## 次の手順

ブートストラップスクリプトが正常に完了すると、小売売上の入出力スキーマとデータセットを表示でき [!DNL Experience Platform]ます。 詳しくは、 [プレビュースキーマデータのチュートリアル](./preview-schema-data.md)を参照してください。

また、小売販売サンプルデータは、提供されたブートストラップスクリプトを [!DNL Experience Platform] 使用してに取り込まれました。

取り込んだデータを引き続き使用するには：
- [Jupyterノートブックを使用してデータを分析する](../jupyterlab/analyze-your-data.md)
   - Data Science WorkspaceのJupyterノートブックを使用して、データにアクセス、調査、視覚化、理解します。
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - 読み込み可能なレシピファイルにソースファイルをパッケージ化して、独自のモデルをに取り込む方法 [!DNL Data Science Workspace] を学ぶには、このチュートリアルに従ってください。