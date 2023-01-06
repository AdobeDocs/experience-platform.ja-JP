---
keywords: Experience Platform；小売販売レシピ；Data Science Workspace；人気の高いトピック；レシピ
solution: Experience Platform
title: 小売販売スキーマとデータセットの作成
type: Tutorial
description: このチュートリアルでは、他のすべての Adobe Experience Platform Data Science Workspace　チュートリアルに必要な前提条件とアセットについて説明します。完了すると、Experience Platform 上の IMS 組織のメンバーと共に、小売販売スキーマとデータセットを利用できるようになります。
exl-id: 1b868c8c-7c92-4f99-8486-54fd7aa1af48
source-git-commit: 86e6924078c115fb032ce39cd678f1d9c622e297
workflow-type: tm+mt
source-wordcount: '554'
ht-degree: 64%

---


# 小売販売スキーマとデータセットの作成

このチュートリアルでは、その他すべてに必要な前提条件とアセットについて説明します [!DNL Adobe Experience Platform] [!DNL Data Science Workspace] チュートリアル 完了すると、IMS 組織のメンバーと共に、小売販売スキーマとデータセットをで使用できるようになります。 [!DNL Experience Platform].

## はじめに

このチュートリアルを開始する前に、次の前提条件を満たす必要があります。
- アクセス先 [!DNL Adobe Experience Platform]. の IMS 組織へのアクセス権がない場合 [!DNL Experience Platform]続行する前に、システム管理者にお問い合わせください。
- 作成の認証 [!DNL Experience Platform] API 呼び出し。 このチュートリアルを正しく完了するには、『[ Adobe Experience Platform API の認証とアクセス](https://experienceleague.adobe.com/docs/experience-platform/landing/platform-apis/api-authentication.html?lang=ja)』チュートリアルを完了して次の値を取得してください。
   - Authorization: `{ACCESS_TOKEN}`
   - x-api-key: `{API_KEY}`
   - x-gw-ims-org-id: `{ORG_ID}`
   - クライアント秘密鍵：`{CLIENT_SECRET}`
   - クライアント証明書：`{PRIVATE_KEY}`
- [小売販売レシピ](../pre-built-recipes/retail-sales.md)のデータとソースファイルの例。この他に必要なアセットをダウンロードする [!DNL Data Science Workspace] からのチュートリアル [Adobeのパブリック Git リポジトリ](https://github.com/adobe/experience-platform-dsw-reference/).
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

1. 内部 [!DNL Experience Platform] チュートリアルリソースパッケージ、ディレクトリに移動 `bootstrap`を開き、 `config.yaml` 適切なテキストエディターを使用する。
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

   - `platform_gateway`：API 呼び出しのベースパス。この値は変更しないでください。
   - `ims_token`：`{ACCESS_TOKEN}` はここに格納されます。
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

1. ターミナルアプリケーションを開き、 [!DNL Experience Platform] チュートリアルリソースディレクトリ。
2. `bootstrap` ディレクトリを現在の作業パスに設定し、次のコマンドを入力して `bootstrap.py` スクリプトを実行します。[!DNL Python]

   ```bash
   python bootstrap.py
   ```

   >[!NOTE]
   >
   > スクリプトの完了には数分かかる場合があります。

## 次の手順

ブートストラップスクリプトが正常に完了すると、小売販売の入出力スキーマとデータセットをで表示できます。 [!DNL Experience Platform]. 詳しくは、『[プレビュースキーマデータのチュートリアル](./preview-schema-data.md)』を参照してください。

また、小売販売のサンプルデータをに取り込みました [!DNL Experience Platform] 提供されたブートストラップスクリプトを使用する。

取得したデータを引き続き使用するには、以下を実行します。
- [Jupyter Notebook を使用したデータ分析](../jupyterlab/analyze-your-data.md)
   - Data Science Workspace の Jupyter ノートブックを使用して、データにアクセスし、調査、視覚化、理解します。
- [ソースファイルのレシピへのパッケージ化](./package-source-files-recipe.md)
   - このチュートリアルでは、独自のモデルをに取り込む方法を説明します。 [!DNL Data Science Workspace] インポート可能なレシピファイルにソースファイルをパッケージ化する。
