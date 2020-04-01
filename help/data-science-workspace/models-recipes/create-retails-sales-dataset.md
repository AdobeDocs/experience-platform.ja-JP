---
keywords: Experience Platform;retail sales recipe;Data Science Workspace;popular topics
solution: Experience Platform
title: 小売販売スキーマとデータセットの作成
topic: Tutorial
translation-type: tm+mt
source-git-commit: 91c7b7e285a4745da20ea146f2334510ca11b994

---


# 小売販売スキーマとデータセットの作成

このチュートリアルでは、他のすべてのAdobe Experience Platform Data Science Workspaceチュートリアルに必要な前提条件とアセットについて説明します。 完了すると、Experience Platform上のIMS組織のメンバーと共に、小売販売スキーマとデータセットを利用できるようになります。

## はじめに

このチュートリアルを開始する前に、次の前提条件が必要です。
- Adobe Experience Platformへのアクセス。 Experience PlatformのIMS組織にアクセスできない場合は、先に進む前にシステム管理者にお問い合わせください。
- エクスペリエンスプラットフォームAPI呼び出しを行うための認証。 このチュートリ [アルを正しく完了するには、「](../../tutorials/authentication.md) Authenticate and access Adobe Experience Platform APIs」チュートリアルを完了し、次の値を取得します。
   - 認証: `{ACCESS_TOKEN}`
   - x-api-key: `{API_KEY}`
   - x-gw-ims-org-id: `{IMS_ORG}`
   - Client secret: `{CLIENT_SECRET}`
   - クライアント証明書： `{PRIVATE_KEY}`
- 小売販売レシピのサンプルデータとソ [ースファイル](../pre-built-recipes/retail-sales.md)。 このチュートリアルやその他のData Science Workspaceチュートリアルに必要なアセットを [AdobeのパブリックGitリポジトリからダウンロードします](https://github.com/adobe/experience-platform-dsw-reference/)。
- [Python >= 2.7](https://www.python.org/downloads/) and the following Pythonパッケージ：
   - [パイプ](https://pypi.org/project/pip/)
   - [PyYAML](https://pyyaml.org/)
   - [ディクター](https://pypi.org/project/dictor/)
   - [JWT](https://pypi.org/project/jwt/)
- このチュートリアルで使用する次の概念について、実際に理解しておく必要があります。
   - [エクスペリエンスデータモデル(XDM)](../../xdm/home.md)
   - [スキーマ合成の基本](../../xdm/schema/field-dictionary.md)

## 小売販売スキーマとデータセットの作成

小売販売スキーマとデータセットは、提供されたブートストラップスクリプトを使用して自動的に作成されます。 次の手順を順に実行します。

### ファイルの設定

1. エクスペリエンスプラットフォームチュートリアルリソースパッケージ内で、ディレクトリに移動 `bootstrap`し、適切なテキスト `config.yaml` エディターを使用して開きます。
2. セクションの `Enterprise` 下で、次の値を入力します。

   ```yaml
   Enterprise:
       api_key: {API_KEY}
       org_id: {IMS_ORG}
       tech_acct: {technical_account_id}
       client_secret: {CLIENT_SECRET}
       priv_key_filename: {PRIVATE_KEY}
   ```

3. 次の例のセクションの下にあ `Platform` る値を編集します。

   ```yaml
   Platform:
       platform_gateway: https://platform.adobe.io
       ims_token: {ACCESS_TOKEN}
       ingest_data: "True"
       build_recipe_artifacts: "False"
       kernel_type: Python
   ```

   - `platform_gateway` :API呼び出しのベースパス。 この値は変更しないでください。
   - `ims_token` :君はこ `{ACCESS_TOKEN}` こに行く。
   - `ingest_data` :このチュートリアルの目的では、小売販売のスキーマとデータセ `"True"` ットを作成するために、この値をに設定します。 の値は、 `"False"` スキーマの作成
   - `build_recipe_artifacts` :このチュートリアルの目的で、スクリプトがレシピアーティファクトを生成 `"False"` しないように、この値をに設定してください。
   - `kernel_type` :レシピアーティファクトの実行タイプ。 この値は、がに設定され `Python` ている場 `build_recipe_artifacts` 合と同じままにし、それ以 `"False"`外の場合は正しい実行タイプを指定します。

4. このセクション `Titles` で、Retail Salesサンプルデータに対して次の情報を適切に指定し、編集後にファイルを保存して閉じます。 以下に例を示します。

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

1. ターミナルアプリケーションを開き、Experience Platform Tutorialリソースディレクトリに移動します。
2. ディレクトリ `bootstrap` を現在の作業パスに設定し、次のコマンドを入力し `bootstrap.py` てPythonスクリプトを実行します。

   ```bash
   python bootstrap.py
   ```

   > [!NOTE] スクリプトの完了には数分かかる場合があります。

## 次の手順

ブートストラップスクリプトが正常に完了すると、小売販売の入出力スキーマとデータセットをエクスペリエンスプラットフォームで表示できます。 詳しくは、「 [プレビュースキーマデータのチ](./preview-schema-data.md)ュートリアル」を参照してください。

また、提供されたブートストラップスクリプトを使用して、小売販売のサンプルデータをエクスペリエンスプラットフォームに正常に取り込みました。

取り込んだデータを引き続き使用するには：
- [Jupyterノートブックを使用してデータを分析する](../jupyterlab/analyze-your-data.md)
   - Data Science WorkspaceのJupyterノートブックを使用して、データにアクセス、調査、視覚化、理解します。
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - このチュートリアルでは、読み込み可能なレシピファイルにソースファイルをパッケージ化して、独自のモデルをData Science Workspaceに取り込む方法を説明します。