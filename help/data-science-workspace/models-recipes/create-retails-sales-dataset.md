---
keywords: Experience Platform；小売の販売手法；データサイエンスワークスペース；人気の高いトピック；レシピ
solution: Experience Platform
title: 小売売上スキーマとデータセットの作成
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、他のすべての Adobe Experience Platform Data Science Workspace　チュートリアルに必要な前提条件とアセットについて説明します。完了すると、Experience Platform 上の IMS 組織のメンバーと共に、小売販売スキーマとデータセットを利用できるようになります。
exl-id: 1b868c8c-7c92-4f99-8486-54fd7aa1af48
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '554'
ht-degree: 63%

---

# 小売販売スキーマとデータセットの作成

このチュートリアルでは、他のすべての[!DNL Adobe Experience Platform] [!DNL Data Science Workspace]チュートリアルに必要な前提条件とアセットについて説明します。 完了すると、[!DNL Experience Platform]上のIMS組織のメンバーと共に、小売販売スキーマとデータセットを利用できるようになります。

## はじめに

このチュートリアルを開始する前に、次の前提条件を満たす必要があります。
- [!DNL Adobe Experience Platform]にアクセスします。 [!DNL Experience Platform]のIMS組織にアクセスできない場合は、先に進む前に、システム管理者にお問い合わせください。
- [!DNL Experience Platform] API呼び出しを行うための認証。 このチュートリアルを正しく完了するには、『[ Adobe Experience Platform API の認証とアクセス](https://www.adobe.com/go/platform-api-authentication-en)』チュートリアルを完了して次の値を取得してください。
   - Authorization: `{ACCESS_TOKEN}`
   - x-api-key: `{API_KEY}`
   - x-gw-ims-org-id: `{IMS_ORG}`
   - クライアント秘密鍵：`{CLIENT_SECRET}`
   - クライアント証明書：`{PRIVATE_KEY}`
- [小売販売レシピ](../pre-built-recipes/retail-sales.md)のデータとソースファイルの例。このチュートリアルと他の[!DNL Data Science Workspace]チュートリアルに必要なアセットを、[AdobeのパブリックGitリポジトリ](https://github.com/adobe/experience-platform-dsw-reference/)からダウンロードします。
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

1. [!DNL Experience Platform]チュートリアルリソースパッケージ内で、`bootstrap`ディレクトリに移動し、`config.yaml`を開きます。
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

1. ターミナルアプリケーションを開き、[!DNL Experience Platform]チュートリアルリソースディレクトリに移動します。
2. `bootstrap` ディレクトリを現在の作業パスに設定し、次のコマンドを入力して `bootstrap.py` スクリプトを実行します。[!DNL Python]

   ```bash
   python bootstrap.py
   ```

   >[!NOTE]
   >
   > スクリプトの完了には数分かかる場合があります。

## 次の手順

ブートストラップスクリプトが正常に完了すると、小売売上の入出力スキーマーとデータセットを[!DNL Experience Platform]で表示できます。 詳しくは、『[プレビュースキーマデータのチュートリアル](./preview-schema-data.md)』を参照してください。

また、提供されたブートストラップスクリプトを使用して、小売販売のサンプルデータを[!DNL Experience Platform]に正しく取り込みました。

取得したデータを引き続き使用するには、以下を実行します。
- [Jupyterノートブックを使用してデータを分析](../jupyterlab/analyze-your-data.md)
   - Data Science WorkspaceのJupyterノートブックを使用して、データにアクセス、調査、視覚化、理解します。
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - 読み込み可能なレシピファイルにソースファイルをパッケージ化して、独自のモデルを[!DNL Data Science Workspace]に取り込む方法を学ぶには、このチュートリアルに従ってください。
