---
title: ユーザー定義ダッシュボード
description: カスタムダッシュボードを作成および管理する方法について説明します。カスタムダッシュボードでは、カスタムウィジェットを作成、追加および編集して主要指標を視覚化できます。
exl-id: a9ab83f7-b68d-4dbf-9dc6-ef253df5c82c
source-git-commit: d874fed681449c6f5114196cface157c8c406d69
workflow-type: tm+mt
source-wordcount: '938'
ht-degree: 1%

---

# ユーザー定義ダッシュボード

Adobe Experience Platformダッシュボードを使用すると、ユーザー定義のダッシュボード機能でインサイトを迅速に処理し、ビジュアライゼーションをカスタマイズできます。 この機能を使用すると、カスタムダッシュボードを作成および管理できます。カスタムダッシュボードでは、カスタムウィジェットを作成、追加および編集して、組織に関連する主要指標を視覚化できます。

>[!IMPORTANT]
>
>Real-time Customer Data Platform Insights ダッシュボードの各クエリが効率的に実行するのに十分なリソースを確保するために、API は、各クエリに同時実行スロットを割り当てて、リソースの使用状況を追跡します。 システムは最大 4 つの同時クエリを処理できるので、同時に 4 つのクエリスロットをいつでも使用できます。 クエリは、同時実行スロットに基づいてキューに入れられ、十分な同時実行スロットが使用可能になるまでキューで待ちます。


<!-- Getting started / permissions section commented out for Beta. This will be necessary after GA only

## Getting started

To view dashboards in Adobe Experience Platform you must have the appropriate permissions enabled. Please read the [dashboards permissions documentation](./permissions.md#available-permissions) to learn how to grant users the ability to view, edit, and update Experience Platform dashboards using Adobe Admin Console. If you do not have administrator privileges for your organization, contact your product administrator to obtain the required permissions. -->

## カスタムダッシュボードの作成

カスタムダッシュボードを作成するには、まずダッシュボードインベントリに移動します。 選択 **[!UICONTROL ダッシュボード]** Platform UI の左側のナビゲーションから、 **[!UICONTROL ダッシュボードを作成]**.

![左側のナビゲーションにダッシュボードが表示され、「ダッシュボードを作成」がハイライトされたダッシュボードインベントリ。](./images/user-defined-dashboards/create-dashboard.png)

カスタムダッシュボードを追加する前に、ダッシュボードの在庫が空になり、「ダッシュボードが見つかりません」と表示されます。 」という内容のメッセージが表示されます。作成後、ユーザー定義のすべてのダッシュボードがダッシュボードインベントリに表示されます。

この [!UICONTROL ダッシュボードを作成] ダイアログが表示されます。 作成するウィジェットのコレクションのわかりやすいわかりやすい名前を入力し、「 」を選択します。 **[!UICONTROL 保存]**.

![ダッシュボードを作成ダイアログ。](./images/user-defined-dashboards/create-dashboard-dialog.png)

新しく作成された空のダッシュボードが、選択した名前で表示されます。

## ウィジェットの作成 {#create-widget}

>[!CONTEXTUALHELP]
>id="platform_dashboards_udd_maxwidgets"
>title="ウィジェットの最大数"
>abstract="ユーザー定義のダッシュボードは、最大 10 個のウィジェットをサポートします。 ダッシュボードに 10 個のウィジェットを追加した後、 [!UICONTROL 新しいウィジェットを追加] オプションは無効で、グレーで表示されます。"

新しいダッシュボードビューで、「 」を選択します。 **[!UICONTROL 新しいウィジェットを追加]** をクリックして、ウィジェットの作成プロセスを開始します。

>[!IMPORTANT]
>
>ユーザー定義のダッシュボードは、最大 10 個のウィジェットをサポートします。 ダッシュボードに 10 個のウィジェットを追加した後、 [!UICONTROL 新しいウィジェットを追加] オプションは無効で、グレーで表示されます。

![新しいウィジェットの追加がハイライトされた新しい空のダッシュボード。](./images/user-defined-dashboards/add-new-widget.png)

### Widget composer

Widget Composer のワークスペースが表示されます。 次に、 **[!UICONTROL データを選択]** をクリックして、ウィジェットに属性を追加するデータモデルを選択します。

![ウィジェットコンポーザーのワークスペース。](./images/user-defined-dashboards/widget-composer.png)

この [!UICONTROL データを選択] ダイアログが表示されます。 左側の列からデータモデルを選択して、使用可能なすべてのテーブルのプレビューリストを表示します。

>[!NOTE]
>
>現在、ユーザー定義ダッシュボードではプロファイルデータモデルのみがサポートされています。 その他のオプションもサポートされます。

![データを選択ダイアログが表示されます。](./images/user-defined-dashboards/select-data-dialog.png)

プレビューリストには、データモデルに含まれるテーブルの詳細が表示されます。 次の表に、列のフィールドとその可能な値の説明を示します。

| 列フィールド | 説明 |
|---|---|
| [!UICONTROL タイトル] | テーブルの名前. |
| [!UICONTROL テーブルタイプ] | テーブルのタイプ。 考えられるタイプは次のとおりです。 `fact`, `dimension`、および `none`. |
| [!UICONTROL 参照] | 選択したテーブルに結合されたテーブルの数。 |

選択 **[!UICONTROL 次へ]** をクリックして、データモデルの選択を確認します。 次のビューには、左側のレールで使用可能なテーブルのリストが表示されます。 テーブルを選択すると、選択したテーブルに含まれているデータの包括的な分類が表示されます。

この [!UICONTROL プレビュー] パネルには、次のタブが含まれます [!UICONTROL サンプルレコード] および [!UICONTROL 属性]. この [!UICONTROL サンプルレコード] 「 」タブには、選択したテーブルのレコードのサブセットが表ビューで表示されます。 この [!UICONTROL 属性] 「 」タブには、選択したテーブルに関連付けられているすべての属性の属性名、データ型、およびソーステーブルが表示されます。

左側のレールで使用可能なリストからテーブルを選択してウィジェットのデータを提供し、「 」を選択します。 **[!UICONTROL 選択]** をクリックして、ウィジェットコンポーザーに戻ります。

![選択がハイライト表示されたデータを選択ダイアログが開きます。](./images/user-defined-dashboards/select-a-table.png)

これで、選択したテーブルのデータが Widget Composer に入力されます。

データモデルと現在選択されているテーブルが左パネルの上部に表示され、ウィジェットの作成に使用できる属性が「属性」列に表示されます。

![ウィジェットコンポーザー内でデータが入力されたウィジェット。](./images/user-defined-dashboards/populated-widget-composer.png)

>[!TIP]
>
>鉛筆アイコン (![鉛筆アイコン。](./images/user-defined-dashboards/edit-icon.png)) をクリックします。

追加アイコン (./images/user-defined-dashboards/add-icon.png) をクリックし、X 軸または Y 軸に属性を追加します。

![ウィジェットコンポーザーでは、追加アイコンドロップダウンがハイライト表示され、ウィジェット軸に属性を追加します。](./images/user-defined-dashboards/attributes-dropdown.png)

次に、 [!UICONTROL トンボ] ドロップダウンを使用して、ウィジェットの現在の設定のプレビュービジュアライゼーションを生成します。 内 [!UICONTROL プロパティ] 画面の右側にあるパネルで、 [!UICONTROL ウィジェットのタイトル] テキストフィールド。

![マークドロップダウンとウィジェットタイトルテキストフィールドがハイライトされたウィジェットコンポーザー。](./images/user-defined-dashboards/marks-dropdown-widget-title.png)

ウィジェットに問題がない場合は、 **[!UICONTROL 保存]**. ウィジェット名の下にあるチェックマークのアイコンは、ウィジェットが保存されたことを示します。

>[!NOTE]
>
>Widget Composer に保存すると、ウィジェットがダッシュボードにローカルに保存されます。 ダッシュボードを保存せずにダッシュボードエディタを終了した場合、ウィジェットはダッシュボードに保存されません。

![新しいウィジェットの保存の確認。](./images/user-defined-dashboards/save-confirmation.png)

選択 **[!UICONTROL キャンセル]** をクリックして、カスタムダッシュボードに戻ります。

![ウィジェットを作成した例を含むウィジェットコンポーザー。](./images/user-defined-dashboards/composed-widget.png)

>[!TIP]
>
>ダッシュボード名の横にある設定アイコンを選択して、その作成の詳細を表示します。 表示されるダイアログで、ダッシュボードの名前を変更できます。

このワークスペース内でウィジェットの配置やサイズ変更を行うことができます。 選択 **[!UICONTROL 保存]** ダッシュボード名と設定済みのレイアウトを保持する

![カスタムウィジェットと保存ボタンがハイライト表示された、ユーザ定義のダッシュボード。](./images/user-defined-dashboards/user-defined-dashboard.png)

## 次の手順

このドキュメントでは、カスタムダッシュボードの作成方法と、そのダッシュボードのカスタムウィジェットの作成、編集、更新方法についてより深く理解しています。

次の用途で使用可能な事前設定済みの指標とビジュアライゼーションを見つけるには： [プロファイル](./guides/profiles.md#standard-widgets), [セグメント](./guides/segments.md#standard-widgets)、および [宛先](./guides/destinations.md#standard-widgets) ダッシュボードについては、各ダッシュボードのドキュメントにある標準ウィジェットのリストを参照してください。
