---
keywords: Experience Platform；小売販売のレシピ；Data Science Workspace；人気のトピック；レシピ
solution: Experience Platform
title: 小売販売のスキーマとデータセットの作成
type: Tutorial
description: このチュートリアルでは、他のすべての Adobe Experience Platform Data Science Workspace　チュートリアルに必要な前提条件とアセットについて説明します。完了すると、小売販売のスキーマとデータセットが、Experience Platform時にユーザーと組織のメンバーが使用できるようになります。
exl-id: 1b868c8c-7c92-4f99-8486-54fd7aa1af48
source-git-commit: 81f48de908b274d836f551bec5693de13c5edaf1
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 43%

---


# 小売販売スキーマとデータセットの作成

このチュートリアルでは、他のすべての [!DNL Adobe Experience Platform] [!DNL Data Science Workspace] チュートリアルに必要な前提条件とアセットを提供します。 完了すると、小売販売のスキーマとデータセットが、[!DNL Experience Platform] でユーザーおよび組織のメンバーが使用できるようになります。

## はじめに

このチュートリアルを開始する前に、次の前提条件を満たす必要があります。
- [!DNL Adobe Experience Platform] へのアクセス。 [!DNL Experience Platform] の組織にアクセスする権限がない場合は、続行する前にシステム管理者に問い合わせてください。
- [!DNL Experience Platform] API 呼び出しを行うための認証。 このチュートリアルを正しく完了するには、『[ Adobe Experience Platform API の認証とアクセス](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)』チュートリアルを完了して次の値を取得してください。
   - Authorization: `{ACCESS_TOKEN}`
   - x-api-key: `{API_KEY}`
   - x-gw-ims-org-id: `{ORG_ID}`
   - クライアント秘密鍵：`{CLIENT_SECRET}`
   - クライアント証明書：`{PRIVATE_KEY}`
- [小売販売レシピ](../pre-built-recipes/retail-sales.md)のデータとソースファイルの例。このチュートリアルおよびその他の [!DNL Data Science Workspace] チュートリアルに必要なアセットを、[Adobeの公開 Git リポジトリ ](https://github.com/adobe/experience-platform-dsw-reference/) からダウンロードします。
- [Python >= 2.7](https://www.python.org/downloads/) および次の [!DNL Python] パッケージ：
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

1. [!DNL Experience Platform] チュートリアルリソースパッケージ内のディレクトリ `bootstrap` に移動して、適切なテキストエディターを使用して `config.yaml` を開きます。
2. 「`Enterprise`」セクションの下で、次の値を入力します。

   ```yaml
   Enterprise:
       api_key: {API_KEY}
       org_id: {ORG_ID}
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

   - `platform_gateway`:API 呼び出しのベースパス。 この値は変更しないでください。
   - `ims_token`：あなたの `{ACCESS_TOKEN}` はここに行きます。
   - `ingest_data`：このチュートリアルでは、小売販売スキーマとデータセットを作成するために、この値を `"True"` に設定します。 `"False"` の値は、スキーマを作成するだけです。
   - `build_recipe_artifacts`：このチュートリアルでは、この値を `"False"` に設定して、スクリプトがレシピアーティファクトを生成しないようにします。
   - `kernel_type`：レシピアーティファクトの実行タイプ。 この値は、`build_recipe_artifacts` が `"False"` に設定されている場合、`Python` のままにし、それ以外の場合は正しい実行タイプを指定します。

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

1. ターミナルアプリケーションを開き、[!DNL Experience Platform] チュートリアルリソースディレクトリに移動します。
2. `bootstrap` ディレクトリを現在の作業パスとして設定し、次のコマンドを入力して `bootstrap.py` [!DNL Python] スクリプトを実行します。

   ```bash
   python bootstrap.py
   ```

   >[!NOTE]
   >
   >スクリプトが完了するまでに数分かかる場合があります。

## 次の手順

Bootstrap スクリプトが正常に完了すると、小売販売の入力および出力スキーマとデータセットを [!DNL Experience Platform] で表示できます。 詳しくは、『[プレビュースキーマデータのチュートリアル](./preview-schema-data.md)』を参照してください。

また、提供された Bootstrap スクリプトを使用して、小売販売サンプルデータを [!DNL Experience Platform] に正常に取り込みました。

取得したデータを引き続き使用するには、以下を実行します。
- [Jupyter Notebook を使用してデータを分析](../jupyterlab/analyze-your-data.md)
   - Data Science Workspaceの Jupyter Notebooks を使用して、データにアクセス、調査、視覚化、理解します。
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - このチュートリアルでは、ソースファイルをインポート可能なレシピファイルにパッケージ化して、独自のモデルを [!DNL Data Science Workspace] に取り込む方法を説明します。
