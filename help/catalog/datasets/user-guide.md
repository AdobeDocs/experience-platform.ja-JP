---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: Datasetsユーザーガイド
topic: datasets
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '1181'
ht-degree: 0%

---


# Datasetsユーザーガイド

このユーザーガイドでは、Adobe Experience Platformユーザーインターフェイス内でデータセットを操作する際に、一般的な操作を実行する手順を説明します。

## はじめに

このAdobe Experience Platformガイドでは、次のユーザーのコンポーネントについて、十分に理解している必要があります。

* [Datasets](overview.md): Experience Platformにおけるデータ永続性のストレージと管理の構成。
* [Experience Data Model(XDM)System](../../xdm/home.md): Experience Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../xdm/schema/composition.md): XDMスキーマの基本構成要素について説明します。この基本構成要素には、スキーマ構成の主な原則とベストプラクティスが含まれます。
   * [スキーマエディタ](../../xdm/tutorials/create-schema-ui.md): Platformユーザーインターフェイス内のスキーマエディターを使用して、独自のカスタムXDMスキーマを構築する方法を説明します。
* [リアルタイム顧客プロファイル](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。
* [Data Governance](../../data-governance/home.md): お客様のデータの使用に関する規制、制限、ポリシーへの準拠を確保します。

## 表示データセット

Experience PlatformUIで、左側のナビゲーションの **Datasets** (データセット *)をクリックし、* Datasets（データセット）ダッシュボードを開きます。 ダッシュボードリストは、組織で使用可能なすべてのデータセットを管理します。 リストに表示されている各データセットに関する詳細情報(名前、データセットが準拠するスキーマ、最新の取り込み実行のステータスなど)が表示されます。

![](../images/datasets/user-guide/browse_datasets.png)

データセットの名前をクリックして *データセットアクティビティ* 画面にアクセスし、選択したデータセットの詳細を確認します。 「アクティビティ」タブには、使用されるメッセージの割合を視覚化するグラフと、成功および失敗したバッチのリストが含まれます。

![](../images/datasets/user-guide/dataset_activity_1.png)
![](../images/datasets/user-guide/dataset_activity_2.png)

## データセットのプレビュー

データ *セットアクティビティ* 画面で、画面の右上隅近くにある **プレビューデータセット** をクリックし、最大100行のデータをプレビューします。 データセットが空の場合、プレビューリンクは無効になり、代わりに **プレビューは使用できません**。

![](../images/datasets/user-guide/click_to_preview.png)

プレビューーウィンドウの右側には、データセットのスキーマの階層表示が表示されます。

![](../images/datasets/user-guide/preview_dataset.png)

データにアクセスするためのより堅牢な方法を提供するため、Experience Platformは、クエリサービスやJupyterLabなどの下流のサービスを提供し、データを調査および分析します。 詳しくは、次のドキュメントを参照してください。

* [クエリサービスの概要](../../query-service/home.md)
* [JupterLabユーザガイド](../../data-science-workspace/jupyterlab/overview.md)

## データセットの作成 {#create}

新しいデータセットを作成するには、 **Datasets** ダッシュボードの「データセットを *作成* 」をクリックします。

![](../images/datasets/user-guide/click_to_create.png)

次の画面には、新しいデータセットを作成するための次の2つのオプションが表示されます。

* [スキーマからデータセットを作成](#create-a-dataset-with-an-existing-schema)
* [CSVファイルからデータセットを作成する](#create-a-dataset-with-a-csv-file)

### 既存のスキーマを使用したデータセットの作成

データセット *の作成画面で、「スキーマからデータセットを* 作成 **** 」をクリックし、新しい空のデータセットを作成します。

![](../images/datasets/user-guide/create_dataset_schema.png)

[ *スキーマの選択* ]ステップが表示されます。 「 **次へ**」をクリックする前に、スキーマリストを参照し、データセットが準拠するスキーマを選択します。

![](../images/datasets/user-guide/select_schema.png)

データセット *の設定* 手順が表示されます。 名前と説明（任意選択）をデータセットに付け、「 **完了** 」をクリックしてデータセットを作成します。

![](../images/datasets/user-guide/configure_dataset_schema.png)

### CSVファイルを使用したデータセットの作成

CSVファイルを使用してデータセットを作成する場合は、指定したCSVファイルと一致する構造のデータセットを提供するアドホックスキーマが作成されます。 「データセット *の作成* 」画面で、「CSVファイルからデータセットを **作成**」と表示されているボックスをクリックします。

![](../images/datasets/user-guide/create_dataset_csv.png)

The *Configure* step appears. 名前と説明（任意選択）をデータセットに付け、「 **次へ**」をクリックします。

![](../images/datasets/user-guide/configure_dataset_csv.png)

*追加* data stepが表示されます。 CSVファイルを画面の中央にドラッグ&amp;ドロップしてアップロードするか、「 **参照** 」をクリックしてファイルディレクトリを調べます。 ファイルのサイズは10ギガバイトまでです。 CSVファイルがアップロードされたら、「 **保存** 」をクリックしてデータセットを作成します。

>[!NOTE]
>
>CSV列名は、英数字で開始する必要があり、文字、数字、アンダースコアのみを含めることができます。

![](../images/datasets/user-guide/add_csv_data.png)

## リアルタイム顧客プロファイルのデータセットの有効化

すべてのデータセットには、顧客プロファイルに取り込んだデータを豊富にする機能があります。 そのためには、データセットが適合するスキーマが、リアルタイム顧客プロファイルでの使用に適している必要があります。 互換性のあるスキーマは、次の要件を満たします。

* スキーマに、identityプロパティとして指定された属性が1つ以上あります。
* スキーマには、プライマリIDとして定義されたIDプロパティがあります。

プロファイル用のスキーマを有効にする方法の詳細については、『 [スキーマエディタユーザガイド](../../xdm/tutorials/create-schema-ui.md)』を参照してください。

プロファイル用のデータセットを有効にするには、 *データセットアクティビティ* 画面にアクセスし、 **プロパティ** 列内のプロファイル ** 切り替えをクリックします。 有効にすると、データセットに取り込まれたデータは、顧客のプロファイルに入力する際にも使用されます。

![](../images/datasets/user-guide/enable_dataset_profiles.png)

データセットに既にデータが含まれ、その後プロファイルが有効になっている場合、既存のデータはプロファイルによって消費されません。 プロファイル用のデータセットを有効にした後、既存のデータを取り込み直して、顧客プロファイルにデータを入力させることをお勧めします。

## データセットに対するデータ管理の管理と実施

Data Usage Labeling and Enforcement(DULE)は、Experience Platformのための中核的なデータ管理メカニズムです。 DULEラベルを使用すると、データに適用される使用ポリシーに従ってデータセットとフィールドを分類できます。 ラベルの詳細については、 [データ管理の概要](../../data-governance/home.md) （英語）を参照してください。ラベルをデータセットに適用する方法については、『 [データ使用ラベルユーザガイド](../../data-governance/labels/overview.md) 』を参照してください。

## データセットの削除

データセットを削除するには、まず *データセットアクティビティ* 画面にアクセスします。 次に、「 **データセットの削除** 」をクリックして削除します。

>[!NOTE]
>
>アドビのアプリケーションおよびサービス(アドビのAnalytics、Adobe Audience Manager、Decisioningサービスなど)で作成および使用されるデータセットは削除できません。

![](../images/datasets/user-guide/delete_dataset.png)

確認ボックスが表示されます。 「 **削除** 」をクリックして、データセットの削除を確定します。

![](../images/datasets/user-guide/confirm_delete.png)

## プロファイル対応データセットの削除

データセットのプロファイルが有効になっている場合、UIからデータセットを削除すると、データセットの取り込みが無効になりますが、バックエンド内のデータセットは自動的に削除されません。 データセットに含まれるプロファイルとIDデータを完全に削除するには、追加の削除リクエストを行う必要があります。 プロファイルストアからデータを正しく削除する手順については、プロファイルシステムジョブのリアルタイムプロファイルAPI [サブガイド（「削除要求」とも呼ばれる）を参照してください](../../profile/api/profile-system-jobs.md)。

## データの取り込みの監視

Experience PlatformUIで、左側のナビゲーションにある **Monitoring** （監視）をクリックします。 *監視* ダッシュボードを使用すると、バッチまたはストリーミング取り込みから受信データのステータスを表示できます。 個々のバッチのステータスを表示するには、「エンドツーエンドの *バッチ処理* 」または「エンドツーエンドのストリーミング処理 *」をクリックし*&#x200B;ます。 ダッシュボードは、正常に実行された、失敗した、または進行中のバッチまたはストリーミング取り込みを含む、すべての取り込みの実行をリストします。 各リストには、バッチID、ターゲットデータセットの名前、取り込まれたレコード数など、バッチの詳細が表示されます。 ターゲットデータセットのプロファイルが有効になっている場合は、取り込まれたIDレコードとプロファイルレコードの数も表示されます。

![](../images/datasets/user-guide/batch_listing.png)

個々の **バッチIDをクリックして「** バッチの概要 ** 」ダッシュボードにアクセスし、バッチの取り込みが失敗した場合にエラーログを含むバッチの詳細を確認できます。

![](../images/datasets/user-guide/batch_overview.png)

バッチを削除する場合は、ダッシュボードの右上にある「バッチ **を削除** 」をクリックします。 また、バッチが最初に取り込まれたデータセットからレコードも削除されます。

![](../images/datasets/user-guide/delete_batch.png)

## 次の手順

このユーザーガイドでは、Experience Platformユーザーインターフェイスでデータセットを操作する際に、一般的な操作を実行する手順を説明しています。 データセットに関連する一般的なPlatformワークフローを実行する手順については、次のチュートリアルを参照してください。

* [APIを使用したデータセットの作成](create.md)
* [Data Access APIを使用したクエリデータセットデータ](../../data-access/home.md)
* [APIを使用したリアルタイム顧客プロファイルおよびIDサービスのデータセットの設定](../../profile/tutorials/dataset-configuration.md)