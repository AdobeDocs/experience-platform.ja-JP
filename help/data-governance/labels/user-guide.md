---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用ラベルユーザーガイド
topic: labels
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 0%

---


# データ使用ラベルユーザーガイド

このユーザーガイドでは、ユーザーインターフェイス内でデータ使用ラベル（DULEラベルとも呼ばれます）を使用する手順を説明し [!DNL Experience Platform] ます。 このガイドを使用する前に、DULEフレームワークのより強固な概要については、 [データ・ガバナンスの概要](../home.md) を参照してください。

## データセットレベルでのデータ使用量ラベルの管理

データセットレベルでデータ使用量ラベルを管理するには、既存のデータセットを選択するか、新しいデータセットを作成する必要があります。 Adobe Experience Platformにログインした後、左側のナビゲーションで「 **[!UICONTROL Datasets]** 」を選択し、 _Datasets_ Workspaceを開きます。 このページには、組織に属するすべてのデータセットが作成され、各データセットに関連する有用な詳細情報と共にリストされます。

![Data Workspace内の「データセット」タブ](../images/labels/datasets.png)

次の節では、ラベルを適用する新しいデータセットを作成する手順を説明します。 既存のデータセットのラベルを編集する場合は、リストからデータセットを選択し、データセットにデータ使用量ラベルを [追加する前にスキップし](#add-labels)ます。

### 新しいデータセットの作成

>[!NOTE]
>
>この例では、事前設定済みのExperience Data Model(XDM)スキーマを使用してデータセットを作成します。 XDMスキーマについて詳しくは、「 [XDM System overview](../../xdm/home.md) and [basics ofスキーマ構成](../../xdm/schema/composition.md)」を参照してください。

新しいデータセットを作成するには、 **[!UICONTROL Datasets]** ワークスペースの右上隅にある「データセットを _[!UICONTROL 作成]_」をクリックします。

![](../images/labels/create_dataset.png)

「データセット _[!UICONTROL の作成]_」画面が表示されます。 ここから、「スキーマからデータセットを**[!UICONTROL &#x200B;作成&#x200B;]**」をクリックします。

![スキーマからデータセットを作成](../images/labels/dataset_create.png)

[ _[!UICONTROL スキーマの選択]_]画面が表示され、データセットの作成に使用できるすべてのスキーマがリストされます。 スキーマの横にあるラジオボタンをクリックして選択します。 右側の_[!UICONTROL &#x200B;スキーマ]_ ・セクションには、選択したスキーマに関する追加の詳細が表示されます。 Once you have selected a schema, click **[!UICONTROL Next]**.

![データセットスキーマの選択](../images/labels/dataset_schema.png)

データセット _を設定_ 画面が表示されます。 新しいデータセットの **名前** （必須）と **説明** （任意、推奨）を入力し、「 **[!UICONTROL 完了]**」をクリックします。

![名前と説明を使用したデータセットの設定](../images/labels/dataset_configure.png)

「 _[!UICONTROL データセットアクティビティ]_」ページが開き、新しく作成したデータセットに関する情報が表示されます。 この例では、データセットの名前は「Loyality Members」なので、トップナビゲーションには_ Datasets/Loyality Members _と表示されます。

![データセットアクティビティページ](../images/labels/dataset_activity.png)

### データセット追加へのデータ使用ラベル {#add-labels}

新しいデータセットを作成した後、または _[!UICONTROL Datasets]_ワークスペースのリストから既存のデータセットを選択した後、**[!UICONTROL  Data Governance ]**（データ管理）をクリックして_[!UICONTROL  Data Governance]_ （データ管理）ワークスペースを開きます。 このワークスペースでは、データセットレベルとフィールドレベルでデータ使用ラベルを管理できます。

![「Dataset Data Governance」タブ](../images/labels/dataset_data_governance.png)

データセット名の横にある鉛筆アイコンをクリックして、開始レベルでデータ使用ラベルを編集します。

![データセットレベルのラベルの編集](../images/labels/dataset_labels_edit_button.png)

「 _[!UICONTROL 管理ラベルの]_編集」ダイアログが開きます。 ダイアログ内で、データセットに適用するラベルの横にあるチェックボックスをオンにします。 これらのラベルは、データセット内のすべてのフィールドに継承されます。 「_[!UICONTROL &#x200B;適用されたラベル]_ 」ヘッダーは、各チェックボックスをオンにすると更新され、選択したラベルが表示されます。 必要なラベルを選択したら、「変更の **[!UICONTROL 保存]**」をクリックします。

<img alt="データセット・レベルでのガバナンス・ラベルの適用" src="../images/labels/apply-labels-dataset.png" width="700"><br>

「 _[!UICONTROL Data Governance]_」ワークスペースが再表示され、データセットレベルで適用したラベルが表示されます。 また、ラベルがデータセット内の各フィールドに継承されることもわかります。

![フィールドに継承されるデータセットラベル](../images/labels/dataset_inherited_labels.png)

データセットレベルでラベルの横に「x」が表示され、ラベルを削除できます。 各フィールドの横に継承されたラベルの横には「x」が表示されず、「灰色表示」に表示されますが、削除や編集はできません。 これは、 **継承されたフィールドは読み取り専用で**、フィールドレベルでは削除できないためです。

「継承ラベルを **** 表示」はデフォルトでオンになっており、データセットからフィールドに継承されたラベルをすべて表示できます。 オフに切り替えると、データセット内の継承されたラベルは非表示になります。

![継承されたラベルを非表示にする](../images/labels/hide_inherited_labels.png)

## データセットフィールドレベルでのデータ使用量ラベルの管理

データセットレベルでのデータ使用量ラベルの [追加や編集のワークフローを継続して](#add-labels)、そのデータセットの _[!UICONTROL Data Governance]_ワークスペース内でフィールドレベルのラベルを管理することもできます。

個々のフィールドにデータ使用量ラベルを適用するには、フィールド名の横にあるチェックボックスを選択し、「 **[!UICONTROL 管理ラベルの]**&#x200B;編集」をクリックします。

![フィールドラベルを編集](../images/labels/fields_single_field.png)

「 _[!UICONTROL 管理ラベルの]_編集」ダイアログが表示されます。 このダイアログには、選択したフィールド、適用されたラベル、継承されたラベルを示すヘッダーが表示されます。 継承されたラベル（C2およびC5）は、ダイアログ内で灰色表示になっています。 これらのラベルは、データセットレベルから継承された読み取り専用のラベルなので、データセットレベルでのみ編集できます。

<img alt="個々のフィールドのガバナンスラベルの編集" src="../images/labels/field-label-inheritance.png" width="700"><br>

使用する各ラベルの横にあるチェックボックスをクリックして、フィールドレベルのラベルを選択します。 ラベルを選択すると、「 _[!UICONTROL 適用されたラベル]_」ヘッダーが更新され、「_[!UICONTROL &#x200B;選択されたフィールド]_ 」ヘッダーに表示されているフィールドに適用されたラベルが表示されます。 フィールドレベルのラベルの選択が完了したら、「変更の **[!UICONTROL 保存]**」をクリックします。

<img alt="フィールドレベルのラベルの適用" src="../images/labels/apply-labels-field.png" width="700"><br>

[ _[!UICONTROL データ管理]_]ワークスペースが再表示され、選択したフィールドレベルのラベルがフィールド名の横の行に表示されます。 フィールドレベルのラベルの横には「x」が付き、ラベルを削除できます。

![フィールドレベルのラベルを表示するフィールド](../images/labels/fields_show_field_level_labels.png)

フィールドレベルのラベルを同時に適用する複数のフィールドを選択するなど、追加のフィールドに対してフィールドレベルのラベルの追加や編集を続行する場合は、この手順を繰り返します。

![複数のフィールドを選択して、フィールドレベルのラベルを同時に適用します。](../images/labels/fields_select_multiple.png)

継承の移動は最上位レベルから下に行われる（データセット→フィールド）のみであり、フィールドレベルで適用されたラベルは他のフィールドやデータセットには反映されないことに注意する必要があります。

## 次の手順

データセットとフィールドレベルにデータ使用量ラベルを追加したら、にデータを取り込み始めることができ [!DNL Experience Platform]ます。 詳しくは、 [データ取り込みに関するドキュメントを参照して開始](../../ingestion/home.md)。

適用したラベルに基づいてデータ使用ポリシーを定義できるようになりました。 詳しくは、「 [データ使用ポリシーの概要](../policies/overview.md)」を参照してください。

## その他のリソース

次のビデオでは、の理解を深めることを目的としており、データセット [!DNL Data Governance]と個々のフィールドにラベルを適用する方法の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/29709?quality=12&enable10seconds=on&speedcontrol=on)
