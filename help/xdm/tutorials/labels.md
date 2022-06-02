---
title: スキーマのデータ使用状況ラベルの管理
description: Adobe Experience Platform UI でデータ使用ラベルをエクスペリエンスデータモデル (XDM) スキーマフィールドに追加する方法を説明します。
source-git-commit: 6156d84cfdd33f8fe491e9a80e3711cf304733e9
workflow-type: tm+mt
source-wordcount: '691'
ht-degree: 6%

---

# スキーマのデータ使用ラベルの管理

>[!IMPORTANT]
>
>スキーマベースのラベル付けは、 [属性ベースのアクセス制御](../../access-control/abac/overview.md)：現在、米国を拠点とする医療関連のお客様向けの限定リリースで利用できます。 この機能は、完全にリリースされると、すべてのReal-time Customer Data Platformのお客様が利用できるようになります。

Adobe Experience Platformに取り込まれるすべてのデータは、Experience Data Model(XDM) スキーマによって制限されます。 このデータは、組織または法規制によって定義された使用制限の対象となる場合があります。これを考慮するには、プラットフォームでは、 [データ使用ラベル](../../data-governance/labels/overview.md).

スキーマフィールドに適用されるラベルは、その特定のフィールドに含まれるデータに適用される使用ポリシーを示します。

ラベルは個々のデータセット（およびこれらのデータセット内のフィールド）に適用できますが、スキーマレベルでラベルを適用することもできます。 ラベルをスキーマに直接適用する場合、それらのラベルは、そのスキーマに基づく既存および将来のすべてのデータセットに反映されます。

このチュートリアルでは、Platform UI のスキーマエディターを使用して、スキーマにラベルを追加する手順を説明します。

## はじめに

このガイドでは、Adobe Experience Platform の次のコンポーネントに関する十分な知識が必要です。

* [[!DNL Experience Data Model (XDM) System]](../home.md)：[!DNL Experience Platform] がカスタマーエクスペリエンスのデータの整理に使用する、標準化されたフレームワーク。
   * [スキーマエディター](../ui/overview.md):Platform UI でスキーマやその他のリソースを作成および管理する方法について説明します。
* [[!DNL Adobe Experience Platform Data Governance]](../../data-governance/home.md):ラベル付きのデータに対して実行できるマーケティングアクション（または実行できない）を定義するポリシーを使用して、Platform の操作に対してデータ使用制限を適用するためのインフラストラクチャを提供します。

## ラベルを追加するスキーマまたはフィールドを選択

ラベルはスキーマにのみ適用でき、これらのスキーマ（クラス、フィールドグループ、データ型）を構成するコンポーネントには追加できません。 ラベルの追加を開始するには、まず [編集する既存のスキーマを選択](../ui/resources/schemas.md#edit) または [新しいスキーマの作成](../ui/resources/schemas.md#create) をクリックして、スキーマエディターで構造を表示します。

個々のフィールドのラベルを編集するには、キャンバスでフィールドを選択してから、「 **[!UICONTROL アクセスを管理]** をクリックします。

![スキーマエディターキャンバスからフィールドを選択します。](../images/tutorials/labels/manage-access.png)

また、 **[!UICONTROL ラベル]** 」タブで、リストから目的のフィールドを選択し、「 」を選択します。 **[!UICONTROL ガバナンスラベルを編集]** をクリックします。

![次からフィールドを選択： [!UICONTROL ラベル] タブ](../images/tutorials/labels/select-field-on-labels-tab.png)

スキーマ全体のラベルを編集するには、鉛筆アイコン (![](../images/tutorials/labels/pencil-icon.png)) をクリックし、 **[!UICONTROL ラベル]** タブをクリックします。

![次の中からスキーマ名を選択します。 [!UICONTROL ラベル] タブ](../images/tutorials/labels/select-schema-on-labels-tab.png)

>[!NOTE]
>
>免責事項メッセージは、スキーマまたはフィールドのラベルを最初に編集しようとすると表示され、ラベルの使用が組織のポリシーに応じて下流の操作にどのように影響するかを説明します。 選択 **[!UICONTROL 続行]** をクリックして編集を続行します。
>
>![ラベル使用の免責事項](../images/tutorials/labels/disclaimer.png)

## スキーマまたはフィールドのラベルを編集します

選択したフィールドのラベルを編集できるダイアログが表示されます。 個々のオブジェクトタイプフィールドを選択した場合、右側のレールに、適用されたラベルの適用先となるサブフィールドが一覧表示されます。

![表示される選択されたフィールド](../images/tutorials/labels/edit-labels.png)

>[!NOTE]
>
>スキーマ全体のフィールドを編集している場合、右側のレールには該当するフィールドが表示されず、代わりにスキーマ名が表示されます。

表示されたリストを使用して、スキーマまたはフィールドに追加するラベルを選択します。 ラベルを選択すると、 **[!UICONTROL 適用されたラベル]** 「 」セクションが更新され、これまでに選択したラベルが表示されます。

![適用されたラベルが表示されました](../images/tutorials/labels/applied-labels.png)

表示されるラベルをタイプでフィルターするには、左側のレールで目的のカテゴリを選択します。 新しいカスタムラベルを作成するには、「 **[!UICONTROL ラベルを作成]**.

![表示されたラベルをフィルターするか、新しいラベルを作成します](../images/tutorials/labels/filter-and-create-custom.png)

選択したラベルに問題がない場合は、「 」を選択します。 **[!UICONTROL 保存]** をクリックして、フィールドまたはスキーマに適用します。

![選択したラベルを保存](../images/tutorials/labels/save-labels.png)

この **[!UICONTROL ラベル]** 「 」タブが再び表示され、スキーマに適用されたラベルが表示されます。

![適用されるフィールドラベル](../images/tutorials/labels/field-labels-added.png)

## 次の手順

このガイドでは、スキーマとフィールドのデータ使用ラベルの管理方法について説明します。 スキーマレベルではなく特定のデータセットに追加する方法など、データ使用状況ラベルの管理について詳しくは、 [データ使用ラベル UI ガイド](../../data-governance/labels/user-guide.md).
