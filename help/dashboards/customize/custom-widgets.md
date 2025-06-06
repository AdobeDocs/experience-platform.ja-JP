---
keywords: Experience Platform;ユーザーインターフェイス;UI;ダッシュボード;ダッシュボード;プロファイル;セグメント;宛先;ライセンス使用;ウィジェット;指標;
title: ダッシュボードのカスタムウィジェットの作成
description: このガイドでは、Adobe Experience Platform ダッシュボードで使用するカスタムウィジェットの作成手順を説明します。
exl-id: 0168ab1e-0b7d-4faf-852e-7208a2b09a04
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '979'
ht-degree: 79%

---

# ダッシュボードのカスタムウィジェットの作成

Adobe Experience Platform では、複数のダッシュボードを使用して、組織のデータを表示および操作できます。 また、ダッシュボードビューに新しいウィジェットを追加することで、特定のダッシュボードを更新することもできます。アドビが提供する標準ウィジェットに加えて、カスタムウィジェットを作成して組織全体で共有することもできます。

このガイドでは、Experience Platform UI の [!UICONTROL &#x200B; プロファイル &#x200B;]、[!UICONTROL &#x200B; セグメント &#x200B;]、[!UICONTROL &#x200B; 宛先 &#x200B;] ダッシュボードにカスタムウィジェットを作成して追加する手順を順を追って説明します。

>[!NOTE]
>
>ダッシュボードに加えられた更新は、組織ごとおよびサンドボックス別に行われます。

標準ウィジェットの詳細については、 [ダッシュボードへの標準ウィジェットの追加](standard-widgets.md) のガイドを参照してください。

## ウィジェットライブラリ {#widget-library}

このガイドでは、Experience Platform 内で [!UICONTROL ウィジェットライブラリ] にアクセスする必要があります。ウィジェットライブラリの詳細とUI 内でのアクセス方法については、まず [ウィジェットライブラリの概要](widget-library.md) を読んでください。

## カスタムウィジェットの概要

ウィジェットライブラリ内の「**[!UICONTROL カスタム]**」タブでは、ウィジェットを作成し、組織内の他のユーザーと共有して、ダッシュボードの外観をカスタマイズできます。

>[!IMPORTANT]
>
>組織はウィジェットライブラリに最大 20 個のカスタムウィジェットを作成できます。

「**[!UICONTROL カスタム]**」タブを選択してカスタムウィジェットの作成を開始するか、組織が既に作成したカスタムウィジェットを表示します。

![ 「カスタム」タブがハイライト表示されたウィジェットライブラリワークスペース。](../images/customization/custom-widgets.png)

## カスタムウィジェットの作成

カスタムウィジェットを作成するには、ウィジェットライブラリの右上隅にある「**[!UICONTROL ウィジェットを作成]**」を選択します。組織で最初のカスタムウィジェットの場合は、ウィジェットライブラリの中央から「**[!UICONTROL 作成]**」を選択します。

![ 作成がハイライト表示されたウィジェットライブラリワークスペースの「カスタム」タブ。](../images/customization/create-widget.png)

**[!UICONTROL ウィジェットを作成]** ダイアログで、新しいウィジェットのタイトルと説明を入力し、ウィジェットに表示する属性を選択します。

>[!NOTE]
>
>使用可能な属性のリストは、組織に対して設定されているスキーマによって異なります。 属性の選択とスキーマの設定について詳しくは、 [カスタムウィジェットを作成するためのスキーマの編集](edit-schema.md) に関するガイドを参照してください。

属性を選択するには、追加する属性の横にあるラジオボタンを選択します。

>[!NOTE]
>
>ウィジェットごとに 1 つの属性のみを選択でき、属性ごとに 1 つのウィジェットのみを作成できます。 ある属性に対してウィジェットが既に作成されている場合は、その属性が灰色で表示されます。

![ ウィジェットを作成ダイアログ ](../images/customization/create-widget-dialog.png)

## ビジュアライゼーションの選択

属性を選択すると、新しいウィジェットのプレビューがダイアログに表示されます。 人工知能は、属性データに最適なビジュアライゼーションを自動的に選択し、手動で選択できる追加のビジュアライゼーションオプションを提供するために使用されます。

属性に応じて、AI では様々なビジュアライゼーションオプションを推奨しています。 ビジュアライゼーションの完全なリストには、次のものが含まれます。

* 横棒グラフ：横線は値を表します。
* 縦棒グラフ：縦線は値を表します。
* ドーナツグラフ：円グラフと同様に、値は全体の一部として表示されます。
* 散布図：横軸と縦軸を使用して値を示します。
* 折れ線グラフ：値は、一定期間の変化を示すために 1 行で表示されます。
* 番号カード：単一のキー値を表す概要数値を表示します。
* データテーブル：値はテーブルに行として表示されます。

>[!NOTE]
>
>現在、すべての属性でサポートされている指標はプロファイル数のみです。
>
>サンプルウィジェットに表示されるデータは説明用です。 プレビューには、組織の実際のデータは表示されません。

新しいウィジェットを保存し、「[!UICONTROL カスタム]」タブに戻るには、「**[!UICONTROL 作成]**」を選択します。

![ ビジュアライゼーションオプションと作成がハイライト表示されたウィジェットを作成ダイアログ ](../images/customization/create-widget-select-attribute.png)

これで、ライブラリからウィジェットを選択し、「**[!UICONTROL ウィジェットを追加]**」をクリックして、新しいウィジェットをダッシュボードに追加できます。

![ 新しいウィジェットと「ウィジェットを追加」がハイライト表示されたウィジェットライブラリワークスペースの「カスタム」タブ。](../images/customization/custom-widgets-new.png)

## カスタムウィジェットの非表示

ウィジェットをライブラリに追加した後、ウィジェットカードの楕円形（`...`）を選択し、「**[!UICONTROL ウィジェットを非表示]**」を選択して、ウィジェットを非表示にすることができます。同じドロップダウンからウィジェットをプレビューして編集することもできます。

非表示になっているウィジェットを表示するには、ウィジェットライブラリの右上隅にある「**[!UICONTROL 非表示のウィジェットを表示]**」を選択します。

>[!WARNING]
>
>ライブラリでウィジェットを非表示にしても、個々のユーザーのダッシュボードからウィジェットは削除されません。 ウィジェットを使用しなくなった場合は、すべてのExperience Platform ユーザーがダッシュボードからウィジェットを削除する必要があるので、必ず直接この情報を伝えてください。

![ ウィジェットのドロップダウンメニューオプションと「非表示のウィジェットを表示」がハイライト表示されたウィジェットライブラリワークスペースの「カスタム」タブ。](../images/customization/hide-widget.png)

## カスタムウィジェットの編集

ウィジェットカードのエリプシス（`...`）を選択し、ドロップダウンメニューから「**[!UICONTROL 編集]**」を選択すると、ウィジェットライブラリでカスタムウィジェットを編集できます。

![ 省略記号と「編集」がハイライト表示されたウィジェットドロップダウンメニューオプション ](../images/customization/custom-widget-edit.png)

**[!UICONTROL 編集ウィジェット]** ダイアログで、ウィジェットのタイトルと説明を編集し、様々なビジュアライゼーションをプレビューして選択できます。 編集が完了したら「**[!UICONTROL 保存]**」を選択して変更を保存し、「カスタムウィジェット」タブに戻ります。

>[!WARNING]
>
>ライブラリでウィジェットを編集しても、個々のユーザーのウィジェットは更新されません。 ウィジェットが更新された場合は、古いウィジェットをダッシュボードから削除し、更新されたウィジェットをウィジェットライブラリから選択して追加する必要があるので、必ずExperience Platformのすべてのユーザーに直接伝えてください。

![ 編集ウィジェットのダイアログ ](../images/customization/edit-widget.png)

## 次の手順

このドキュメントを読んだ後、ウィジェットライブラリにアクセスして組織のカスタムウィジェットを作成および追加することができます。 ダッシュボードに表示されるウィジェットのサイズと場所を変更するには、 [ダッシュボードの変更ガイド](modify.md) を参照してください。
