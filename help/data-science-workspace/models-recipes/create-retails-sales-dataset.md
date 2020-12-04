---
keywords: Experience Platform;retail sales recipe;Data Science Workspace;popular topics;recipes
solution: Experience Platform
title: 小売販売スキーマとデータセットの作成
topic: tutorial
type: Tutorial
description: このチュートリアルでは、他のすべての Adobe Experience Platform Data Science Workspace　チュートリアルに必要な前提条件とアセットについて説明します。完了すると、Experience Platform 上の IMS 組織のメンバーと共に、小売販売スキーマとデータセットを利用できるようになります。
translation-type: tm+mt
source-git-commit: 97dfd3a9a66fe2ae82cec8954066bdf3b6346830
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 70%

---


# 小売販売スキーマとデータセットの作成

This tutorial provides you with the prerequisites and assets required for all other [!DNL Adobe Experience Platform] [!DNL Data Science Workspace] tutorials. Upon completion, the Retail Sales schema and datasets will be available for you and members of your IMS Organization on [!DNL Experience Platform].

## はじめに

このチュートリアルを開始する前に、次の前提条件を満たす必要があります。
- へのアクセス [!DNL Adobe Experience Platform]。 If you do not have access to an IMS Organization in [!DNL Experience Platform], please speak to your system administrator before proceeding.
- Authorization to make [!DNL Experience Platform] API calls. このチュートリアルを正しく完了するには、『[ Adobe Experience Platform API の認証とアクセス](../../tutorials/authentication.md)』チュートリアルを完了して次の値を取得してください。
   - Authorization: `{ACCESS_TOKEN}`
   - x-api-key: `{API_KEY}`
   - x-gw-ims-org-id: `{IMS_ORG}`
   - クライアント秘密鍵：`{CLIENT_SECRET}`
   - クライアント証明書：`{PRIVATE_KEY}`
- [小売販売レシピ](../pre-built-recipes/retail-sales.md)のデータとソースファイルの例。Download the assets required for this and other [!DNL Data Science Workspace] tutorials from the [Adobe public Git repository](https://github.com/adobe/experience-platform-dsw-reference/).
- [ 2.7 以降](https://www.python.org/downloads/)[!DNL Python]および次の Python パッケージ：
   - [pip](https://pypi.org/project/pip/)
   - [PyYAML](https://pyyaml.org/)
   - [dictor](https://pypi.org/project/dictor/)
   - [JWT](https://pypi.org/project/jwt/)
- 次の概念に関する十分な知識（このチュートリアルで使用）：
   - [[!DNL Experience Data Model (XDM)]](../../xdm/home.md)
   - [スキーマ構成の基本](../../xdm/schema/field-dictionary.md)

## 小売販売スキーマとデータセットの作成

小売販売スキーマとデータセットは、提供されたブートストラップスクリプトを使用して自動的に作成されます。次の手順を順番に実行します。

### ファイルの設定

1. Inside the [!DNL Experience Platform] tutorial resource package, navigate into the directory `bootstrap`, and open `config.yaml` using an appropriate text editor.
2. 「`Enterprise`」セクションの下で、次の値を入力します。

   ```yaml
   Enterprise:
       api_key: {API_KEY}
       org_id: {IMS_ORG}
       tech_acct: {technical_account_id}
       client_secret: {CLIENT_SECRET}
       priv_key_filename: {PRIVATE_KEY}
   ```

3. 次の例にあるように、「`Platform`」セクションの下にある値を編集します。

   ```yaml
   Platform:
       platform_gateway: https://platform.adobe.io
       ims_token: {ACCESS_TOKEN}
       ingest_data: "True"
       build_recipe_artifacts: "False"
       kernel_type: Python
   ```

   - `platform_gateway`：API 呼び出しのベースパス。この値は変更しないでください。
   - `ims_token`：`{ACCESS_TOKEN}` はここに入ります。
   - `ingest_data`：このチュートリアルの目的では、小売販売のスキーマとデータセットを作成するために、この値を `"True"` に設定します。`"False"` の値は、スキーマを作成するだけです。
   - `build_recipe_artifacts`：このチュートリアルの目的で、スクリプトがレシピアーティファクトを生成しないように、この値を `"False"` に設定します。
   - `kernel_type`：レシピアーティファクトの実行タイプ。この値は、`build_recipe_artifacts` が `"False"` に設定されている場合、`Python` のままにし、それ以外の場合は正しい実行タイプを指定します。

4. 「`Titles`」セクションで、Retail Sales データ例に対して次の情報を適切に指定し、編集後にファイルを保存して閉じます。以下に例を示します。

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

1. Open your terminal application and navigate to the [!DNL Experience Platform] tutorial resource directory.
2. `bootstrap` ディレクトリを現在の作業パスに設定し、次のコマンドを入力して `bootstrap.py` スクリプトを実行します。[!DNL Python]

   ```bash
   python bootstrap.py
   ```

   >[!NOTE]
   >
   > スクリプトの完了には数分かかる場合があります。

## 次の手順

Upon successful completion of the bootstrap script, the Retail Sales input and output schemas and datasets can be viewed on [!DNL Experience Platform]. 詳しくは、『[プレビュースキーマデータのチュートリアル](./preview-schema-data.md)』を参照してください。

You have also successfully ingested Retail Sales sample data into [!DNL Experience Platform] using the provided bootstrap script.

取得したデータを引き続き使用するには、以下を実行します。
- [Jupyter ノートブックを使用したデータ分析](../jupyterlab/analyze-your-data.md)
   - Data Science Workspace　の Jupyter ノートブックを使用して、データにアクセスし、調査、視覚化、理解します。
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - Follow this tutorial to learn how to bring your own Model into [!DNL Data Science Workspace] by packaging source files in an importable Recipe file.