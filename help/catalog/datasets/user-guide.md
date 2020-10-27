---
keywords: Experience Platform;home;popular topics;enable dataset;Dataset;dataset
solution: Experience Platform
title: データセットユーザガイド
topic: datasets
description: このdatasetsユーザーガイドでは、Adobe Experience Platformユーザーインターフェイス内でデータセットを操作する際に、一般的な操作を実行する手順を説明します。
translation-type: tm+mt
source-git-commit: 1c00456ee06c1fc09c8e4ce070c90255f51811e1
workflow-type: tm+mt
source-wordcount: '1146'
ht-degree: 71%

---


# データセットユーザガイド

このユーザガイドでは、Adobe Experience Platform ユーザーインターフェイス内でデータセットを操作する際に、一般的なアクションを実行する手順を説明します。

## はじめに

このユーザガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。 

* [Datasets](overview.md):でのデータ永続性のストレージと管理の構成体 [!DNL Experience Platform]。
* [[!DNL Experience Data Model (XDM) System]](../../xdm/home.md):顧客体験データを [!DNL Experience Platform] 整理する際に使用される標準化されたフレームワーク。
   * [スキーマ構成の基本](../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディタ](../../xdm/tutorials/create-schema-ui.md):ユーザーインターフェイス [!DNL Schema Editor] 内で独自のカスタムXDMスキーマを作成する方法を説明し [!DNL Platform] ます。
* [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [[!DNL Adobe Experience Platform Data Governance]](../../data-governance/home.md):お客様のデータの使用に関する規制、制限、ポリシーへの準拠を確保します。

## データセットの表示

In the [!DNL Experience Platform] UI, click **[!UICONTROL Datasets]** in the left-navigation to open the **[!UICONTROL Datasets]** dashboard. ダッシュボードリストは、組織で使用可能なすべてのデータセットを管理します。リストに表示された各データセットに関する詳細（名前、データセットが適用されるスキーマ、最新の取得実行のステータスなど）が表示されます。

![](../images/datasets/user-guide/browse_datasets.png)

データセットの名前をクリックして、その&#x200B;**[!UICONTROL データセットのアクティビティ]**&#x200B;画面にアクセスし、選択したデータセットの詳細を確認します。「アクティビティ」タブには、消費されるメッセージの割合を視覚化したグラフと、成功および失敗したバッチのリストが含まれます。

![](../images/datasets/user-guide/dataset_activity_1.png)
![](../images/datasets/user-guide/dataset_activity_2.png)

## データセットのプレビュー

**[!UICONTROL データセットアクティビティ]**&#x200B;画面で、画面の右上隅近くにある「**[!UICONTROL データセットのプレビュー]** 」をクリックして、最大 100 行のデータをプレビューします。データセットが空の場合、プレビューリンクは無効になり、プレビューは使用できないと表示されます。

![](../images/datasets/user-guide/click_to_preview.png)

プレビューウィンドウの右側に、データセットのスキーマの階層表示が表示されます。

![](../images/datasets/user-guide/preview_dataset.png)

For more robust methods to access your data, [!DNL Experience Platform] provides downstream services such as [!DNL Query Service] and [!DNL JupyterLab] to explore and analyze data. 詳しくは、次のドキュメントを参照してください。

* [クエリサービスの概要](../../query-service/home.md)
* [JupyterLab ユーザガイド](../../data-science-workspace/jupyterlab/overview.md)

## データセットの作成 {#create}

新しいデータセットを作成するには、まず、「**[!UICONTROL データセット]**」ダッシュボードの「**[!UICONTROL データセットを作成]**」をクリックします。

![](../images/datasets/user-guide/click_to_create.png)

次の画面に、新しいデータセットを作成するための次の 2 つのオプションが表示されます。

* [スキーマからのデータセットの作成](#schema)
* [CSV ファイルからのデータセットの作成](#csv)

### 既存スキーマからのデータセットの作成 {#schema}

**[!UICONTROL データセット作成]**&#x200B;画面で、「**[!UICONTROL スキーマからデータセットを作成]**」をクリックし、新しい空のデータセットを作成します。

![](../images/datasets/user-guide/create_dataset_schema.png)

「**[!UICONTROL スキーマ選択]**」手順が表示されます。「**[!UICONTROL 次へ]**」をクリックする前に、スキーマリストを参照し、データセットの準拠先となるスキーマを選択します。

![](../images/datasets/user-guide/select_schema.png)

**[!UICONTROL データセットの設定]**&#x200B;手順が表示されます。データセットの名前と説明（オプション）を入力し、「**[!UICONTROL 完了]**」をクリックしてデータセットを作成します。

![](../images/datasets/user-guide/configure_dataset_schema.png)

### CSV ファイルを使用したデータセットの作成 {#csv}

CSV ファイルを使用してデータセットを作成する場合、アドホックスキーマが作成され、指定された CSV ファイルと一致する構造のデータセットが提供されます。**[!UICONTROL データセット作成]**&#x200B;画面で、「**[!UICONTROL CSV ファイルからデータセットを作成]**」というボックスをクリックします。

![](../images/datasets/user-guide/create_dataset_csv.png)

**[!UICONTROL 設定]**&#x200B;手順が表示されます。データセットの名前とオプションの説明を入力し、「**[!UICONTROL 次へ]**」をクリックします。

![](../images/datasets/user-guide/configure_dataset_csv.png)

「**[!UICONTROL データ追加]**」手順が表示されます。CSV ファイルを画面の中央にドラッグ&amp;ドロップしてアップロードするか、「**[!UICONTROL 参照]**」をクリックしてファイルディレクトリを表示します。ファイルのサイズは 10 ギガバイトまでです。CSV ファイルがアップロードされたら、「**[!UICONTROL 保存]**」をクリックし 、データセットを作成します。

>[!NOTE]
>
> CSV の列名は英数字で始める必要があり、文字、数字、アンダースコアのみを含めることができます。

![](../images/datasets/user-guide/add_csv_data.png)

## リアルタイム顧客プロファイルデータセットの有効化

すべてのデータセットには、取得したデータによって顧客プロファイルを拡張する機能があります。To do so, the schema that the dataset adheres to must be compatible for use in [!DNL Real-time Customer Profile]. 互換性のあるスキーマは、次の要件を満たします。

* スキーマに、ID プロパティとして指定された属性が 1 つ以上あります。
* スキーマに、プライマリ ID として定義された ID プロパティがあります。

For more information on enabling a schema for [!DNL Profile], see the [Schema Editor user guide](../../xdm/tutorials/create-schema-ui.md).

プロファイルでデータセットを有効にするには、その&#x200B;**[!UICONTROL データセットアクティビティ]**&#x200B;画面にアクセスし、「**[!UICONTROL プロパティ]**」列内の&#x200B;**[!UICONTROL プロファイル]**&#x200B;切り替えをクリックします。有効にすると、データセットに取得されたデータが顧客プロファイルに入力されます。

![](../images/datasets/user-guide/enable_dataset_profiles.png)

If a dataset already contains data and is then enabled for [!DNL Profile], the existing data is not consumed by [!DNL Profile]. After a dataset is enabled for [!DNL Profile], it is recommended that you re-ingest any existing data to have them populate customer profiles.

## データセットのデータガバナンスの管理と実施

データ使用状況ラベルを使用すると、データに適用される使用ポリシーに従ってデータセットを分類できます。ラベルについて詳しくは、『[データガバナンスの概要](../../data-governance/home.md)』を参照してください。また、データセットにラベルを適用する方法については、『[データ使用レベルユーザガイド](../../data-governance/labels/overview.md)』を参照してください。

## データセットの削除

データセットを削除するには、まず&#x200B;**[!UICONTROL データセットアクティビティ]**&#x200B;画面にアクセスします。次に、「**[!UICONTROL データセットの削除]**」をクリックして削除します。

>[!NOTE]
>
>Datasets created and utilized by Adobe applications and services (such as Adobe Analytics, Adobe Audience Manager, or [!DNL Offer Decisioning]) cannot be deleted.

![](../images/datasets/user-guide/delete_dataset.png)

確認ボックスが表示されます。「**[!UICONTROL 削除]**」をクリックし、データセットの削除を確定します。

![](../images/datasets/user-guide/confirm_delete.png)

## プロファイル対応データセットの削除

If a dataset is enabled for [!DNL Profile], deleting it through the UI disables the dataset for ingestion, but does not automatically delete the dataset in the backend. データセットに含まれるプロファイルと ID データを完全に削除するには、追加の削除リクエストを実行する必要があります。For steps on how to properly delete data from the [!DNL Profile] store, see the [!DNL Real-time Customer Profile] API [sub-guide on profile system jobs, also known as &quot;delete requests&quot;](../../profile/api/profile-system-jobs.md).

## データ取得の監視

In the [!DNL Experience Platform] UI, click **[!UICONTROL Monitoring]** in the left-navigation. 「**[!UICONTROL 監視]**」ダッシュボードを使用すると 、バッチ取得またはストリーミング取得から受信データのステータスを表示できます。個々のバッチのステータスを表示するには、「**[!UICONTROL エンドツーエンドのバッチ処理]**」または「**[!UICONTROL エンドツーエンドのストリーミング]**」をクリックします。ダッシュボードは、正常、失敗、または進行中のすべてのバッチ取得またはストリーミング取得ををリストします。各リストには、バッチ ID、ターゲットデータセットの名前、取得したレコード数など、バッチの詳細が表示されます。If the target dataset is enabled for [!DNL Profile], the number of ingested identity and profile records is also displayed.

![](../images/datasets/user-guide/batch_listing.png)

個々の&#x200B;**[!UICONTROL バッチ ID]** をクリックして「**[!UICONTROL バッチの概要]**」ダッシュボードにアクセスし、バッチの取得に失敗した場合にはエラーログを含むバッチの詳細を確認できます。

![](../images/datasets/user-guide/batch_overview.png)

バッチを削除する場合は、ダッシュボードの右上にある「**[!UICONTROL バッチの削除]**」をクリックします。また、バッチの最初の取得先であるデータセットからもレコードが削除されます。

![](../images/datasets/user-guide/delete_batch.png)

## 次の手順

This user guide provided instructions for performing common actions when working with datasets in the [!DNL Experience Platform] user interface. For steps on performing common [!DNL Platform] workflows involving datasets, please refer to the following tutorials:

* [API を使用したデータセットの作成](create.md)
* [データアクセス API を使用したクエリデータセットデータ](../../data-access/home.md)
* [API を使用したリアルタイム顧客プロファイルおよび ID サービスのデータセットの設定](../../profile/tutorials/dataset-configuration.md)