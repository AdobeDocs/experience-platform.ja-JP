---
title: ユーザー定義ダッシュボード
description: カスタムダッシュボードを作成および管理する方法について説明します。カスタムダッシュボードでは、カスタムウィジェットを作成、追加および編集して主要指標を視覚化できます。
exl-id: a9ab83f7-b68d-4dbf-9dc6-ef253df5c82c
source-git-commit: a0be2f8625ca60f9c8f355c1230a889002436d6d
workflow-type: tm+mt
source-wordcount: '1307'
ht-degree: 5%

---

# ユーザー定義ダッシュボード

Adobe Experience Platformダッシュボードを使用すると、ユーザー定義のダッシュボード機能でインサイトを迅速に処理し、ビジュアライゼーションをカスタマイズできます。 この機能を使用すると、カスタムダッシュボードを作成および管理できます。カスタムダッシュボードでは、カスタムウィジェットを作成、追加および編集して、組織に関連する主要指標を視覚化できます。

<!-- Getting started / permissions section commented out for Beta. This will be necessary after GA only

## Getting started

To view dashboards in Adobe Experience Platform you must have the appropriate permissions enabled. Please read the [dashboards permissions documentation](./permissions.md#available-permissions) to learn how to grant users the ability to view, edit, and update Experience Platform dashboards using Adobe Admin Console. If you do not have administrator privileges for your organization, contact your product administrator to obtain the required permissions. -->

## カスタムダッシュボードの作成

カスタムダッシュボードを作成するには、まずダッシュボードインベントリに移動します。 選択 **[!UICONTROL ダッシュボード]** Platform UI の左側のナビゲーションから、 **[!UICONTROL ダッシュボードを作成]**.

![左側のナビゲーションにダッシュボードが表示され、「ダッシュボードを作成」がハイライトされたダッシュボードインベントリ。](./images/user-defined-dashboards/create-dashboard.png)

カスタムダッシュボードを追加する前に、ダッシュボードの在庫が空になり、「ダッシュボードが見つかりません」と表示されます。 」という内容のメッセージが表示されます。作成後、ユーザー定義のすべてのダッシュボードがダッシュボードインベントリに表示されます。

>[!NOTE]
>
>既存のダッシュボードを編集するには、在庫リストからダッシュボード名を選択し、鉛筆アイコン (![鉛筆アイコン。](./images/user-defined-dashboards/edit-icon.png))

この [!UICONTROL ダッシュボードを作成] ダイアログが表示されます。 作成するウィジェットのコレクションのわかりやすいわかりやすい名前を入力し、「 」を選択します。 **[!UICONTROL 保存]**.

![ダッシュボードを作成ダイアログ。](./images/user-defined-dashboards/create-dashboard-dialog.png)

新しく作成された空のダッシュボードが、選択した名前で表示されます。

## ウィジェットの作成 {#create-widget}

>[!CONTEXTUALHELP]
>id="platform_dashboards_udd_maxwidgets"
>title="ウィジェットの最大数"
>abstract="ユーザー定義のダッシュボードは、最大 10 個のウィジェットをサポートします。ダッシュボードに 10 個のウィジェットを追加すると、「[!UICONTROL 新しいウィジェットを追加]」オプションが無効になり、グレーで表示されます。"

新しいダッシュボードビューで、「 」を選択します。 **[!UICONTROL 新しいウィジェットを追加]** をクリックして、ウィジェットの作成プロセスを開始します。

>[!IMPORTANT]
>
>ユーザー定義のダッシュボードは、最大 10 個のウィジェットをサポートします。ダッシュボードに 10 個のウィジェットを追加すると、「[!UICONTROL 新しいウィジェットを追加]」オプションが無効になり、グレーで表示されます。

![新しいウィジェットの追加がハイライトされた新しい空のダッシュボード。](./images/user-defined-dashboards/add-new-widget.png)

### Widget composer

Widget Composer のワークスペースが表示されます。 次に、 **[!UICONTROL データを選択]** をクリックして、ウィジェットに属性を追加するデータモデルを選択します。

![ウィジェットコンポーザーのワークスペース。](./images/user-defined-dashboards/widget-composer.png)

#### データモデルを選択 {#select-data-model}

この [!UICONTROL データモデルを選択] ダイアログが表示されます。 左側の列からデータモデルを選択して、使用可能なすべてのテーブルのプレビューリストを表示します。 Real-time Customer Data Platformの事前設定済みデータモデルの名前は、 [!UICONTROL CDPInsights].

>[!TIP]
>
>情報アイコン (![情報アイコン。](./images/user-defined-dashboards/info-icon.png)) をクリックすると、長すぎてデータレールに表示できない場合は完全なデータモデル名が表示されます。

![データを選択ダイアログが表示されます。](./images/user-defined-dashboards/select-data-model-dialog.png)

プレビューリストには、データモデルに含まれるテーブルの詳細が表示されます。 次の表に、列のフィールドとその可能な値の説明を示します。

| 列フィールド | 説明 |
|---|---|
| [!UICONTROL タイトル] | テーブルの名前. |
| [!UICONTROL テーブルタイプ] | テーブルのタイプ。 考えられるタイプは次のとおりです。 `fact`, `dimension`、および `none`. |
| [!UICONTROL レコード] | 選択したテーブルに関連付けられているレコードの数。 |
| [!UICONTROL 参照] | 選択したテーブルに結合されたテーブルの数。 |
| [!UICONTROL 属性] | 選択したテーブルの属性の数。 |

選択 **[!UICONTROL 次へ]** をクリックして、データモデルの選択を確認します。 次のビューには、左側のレールで使用可能なテーブルのリストが表示されます。 テーブルを選択すると、選択したテーブルに含まれているデータの包括的な分類が表示されます。

### ウィジェットの入力 {#populate-widget}

この [!UICONTROL プレビュー] パネルには、次のタブが含まれます [!UICONTROL サンプルレコード] および [!UICONTROL 属性]. この [!UICONTROL サンプルレコード] 「 」タブには、選択したテーブルのレコードのサブセットが表ビューで表示されます。 この [!UICONTROL 属性] 「 」タブには、選択したテーブルに関連付けられているすべての属性の属性名、データ型、およびソーステーブルが表示されます。

左側のレールで使用可能なリストからテーブルを選択してウィジェットのデータを提供し、「 」を選択します。 **[!UICONTROL 選択]** をクリックして、ウィジェットコンポーザーに戻ります。

![選択がハイライト表示されたデータを選択ダイアログが開きます。](./images/user-defined-dashboards/select-a-table.png)

これで、選択したテーブルのデータが Widget Composer に入力されます。

データモデルと現在選択されているテーブルが左側のパネルの上部に表示され、ウィジェットの作成に使用できる属性が「 」にリストされます。 [!UICONTROL 属性] 列。 検索バーを使用して、リストをスクロールする代わりに属性を検索したり、鉛筆アイコン (![鉛筆アイコン。](./images/user-defined-dashboards/edit-icon.png)) をクリックします。

![ウィジェットコンポーザー内でデータが入力されたウィジェット。](./images/user-defined-dashboards/populated-widget-composer.png)

#### 属性の追加とフィルター {#add-and-filter-attributes}

追加アイコン (![追加アイコン。](./images/user-defined-dashboards/add-icon.png)) をクリックし、ウィジェットに属性を追加します。 表示されるドロップダウンメニューでは、ウィジェットの X 軸、Y 軸、色またはフィルターとして属性を追加できます。 この [!UICONTROL カラー] 属性を使用すると、X 軸と Y 軸のマークの結果を色に基づいて区別できます。 これは、3 つ目の属性の構成に基づいて結果を異なる色に分割することで行われます。

>[!TIP]
>
>X 軸と Y 軸の配置を反転する場合は、上下の矢印アイコン (![上下の矢印アイコン。](./images/user-defined-dashboards/switch-axis-icon.png)) をクリックして、配置を切り替えます。

![追加アイコンドロップダウンがハイライト表示されたウィジェットコンポーザー。](./images/user-defined-dashboards/attributes-dropdown.png)

ウィジェットのグラフやグラフのタイプを変更するには、 [!UICONTROL トンボ] ドロップダウンから選択し、使用可能なオプションから選択します。 オプションには、棒、ポイント、ティック、線分、面積が含まれます。 選択すると、ウィジェットの現在の設定のプレビュービジュアライゼーションが生成されます。

![Marks ドロップダウンがハイライトされたウィジェットコンポーザー。](./images/user-defined-dashboards/marks-dropdown.png)

属性をフィルターとして追加することで、ウィジェットに含める値と除外する値を選択できます。 属性リストからフィルターを追加すると、 [!UICONTROL フィルター] ダイアログが表示され、値のチェックボックスを使用して値を選択または選択解除できます。

![ウィジェットから値をフィルタリングするためのフィルターダイアログ。](./images/user-defined-dashboards/filter-dialog.png)

### ウィジェットのプロパティ

プロパティアイコン (![プロパティアイコン。](./images/user-defined-dashboards/properties-icon.png)) を右側のパネルでクリックして、プロパティパネルを開きます。 内 [!UICONTROL プロパティ] パネルで、 [!UICONTROL ウィジェットのタイトル] テキストフィールド。

![プロパティアイコンと「ウィジェットタイトル」フィールドがハイライトされたプロパティパネル。](./images/user-defined-dashboards/properties-panel.png)

ウィジェットのプロパティパネルから、ウィジェットのいくつかの側面を編集できます。 ウィジェットの凡例の場所を編集するための完全なコントロールが用意されています。 凡例を移動するには、 [!UICONTROL 凡例の配置] ドロップダウンを選択し、使用可能なオプションのリストから目的の場所を選択します。 また、凡例に関連付けられているラベルの名前を変更し、X 軸または Y 軸の名前を変更するには、 [!UICONTROL 凡例のタイトル] テキストフィールドまたは [!UICONTROL 軸ラベル] テキストフィールドに設定されます。

#### ウィジェットを保存する {#save-widget}

Widget Composer に保存すると、ウィジェットがダッシュボードにローカルに保存されます。 作業内容を保存し、後で再開する場合は、 **[!UICONTROL 保存]**. ウィジェット名の下にあるチェックマークのアイコンは、ウィジェットが保存されたことを示します。 または、ウィジェットの設定が完了したら、 **[!UICONTROL 保存して閉じる]** を使用して、他のすべてのユーザーがダッシュボードにアクセスできるようにします。 選択 **[!UICONTROL キャンセル]** 作業を中止し、カスタムダッシュボードに戻る

![新しいウィジェットの保存の確認。](./images/user-defined-dashboards/save-confirmation.png)

>[!TIP]
>
>プロパティアイコン (![プロパティアイコン。](./images/user-defined-dashboards/properties-icon.png)) をクリックして、ダッシュボード名の横に表示され、作成の詳細を確認できます。 表示されるダイアログで、ダッシュボードの名前を変更できます。

このワークスペース内でウィジェットの配置やサイズ変更を行うことができます。 選択 **[!UICONTROL 保存]** ダッシュボード名と設定済みのレイアウトを保持する

![カスタムウィジェットと保存ボタンがハイライト表示された、ユーザ定義のダッシュボード。](./images/user-defined-dashboards/user-defined-dashboard.png)

Adobe Real-time Customer Data Platform Insights ダッシュボードの各クエリが効率的に実行するのに十分なリソースを確保するために、API は、各クエリに同時実行スロットを割り当てて、リソースの使用状況を追跡します。 システムは最大 4 つの同時クエリを処理できるので、同時に 4 つのクエリスロットをいつでも使用できます。 クエリは、同時実行スロットに基づいてキューに入れられ、十分な同時実行スロットが使用可能になるまでキューで待ちます。

## 次の手順とその他のリソース

このドキュメントでは、カスタムダッシュボードの作成方法と、そのダッシュボードのカスタムウィジェットの作成、編集、更新方法についてより深く理解しています。

次の用途で使用可能な事前設定済みの指標とビジュアライゼーションを見つけるには： [プロファイル](./guides/profiles.md#standard-widgets), [セグメント](./guides/segments.md#standard-widgets)、および [宛先](./guides/destinations.md#standard-widgets) ダッシュボードについては、各ダッシュボードのドキュメントにある標準ウィジェットのリストを参照してください。

ユーザー定義のダッシュボードのExperience Platformに関する理解を深めるには、次のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3409637?quality=12&learn=on)
