---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ使用ラベルのユーザーガイド
topic: labels
translation-type: tm+mt
source-git-commit: 475e774d5e7ebac036b42aa94736ba8e22c7185f

---


# データ使用ラベルのユーザーガイド

このユーザーガイドでは、Experience Platformユーザーインターフェイス内でデータ使用ラベル（DULEラベルとも呼ばれます）を使用する手順を説明します。 このガイドを使用する前に、DULEフレームワークのより強 [力な紹介については](../home.md) 、「Data Governance overview」を参照してください。

## データセットレベルでのデータ使用ラベルの管理

データセットレベルでデータ使用量ラベルを管理するには、既存のデータセットを選択するか、新しいデータセットを作成する必要があります。 Adobe Experience Platformにログインした後、左側のナビゲ **[!UICONTROL ーションで]** 「Datasets _」を選択し、「_ Datasets」ワークスペースを開きます。 このページのリストでは、組織に属するデータセットを作成し、各データセットに関連する有用な詳細情報を示します。

![Data Workspace内の「データセット」タブ](../images/labels/datasets.png)

次の節では、ラベルを適用する新しいデータセットを作成する手順を説明します。 既存のデータセットのラベルを編集する場合は、リストからデータセットを選択し、データセットにデー [タ使用ラベルを追加する前に進みます](#add-labels)。

### 新しいデータセットの作成

>[!NOTE] この例では、事前設定のExperience Data Model(XDM)スキーマを使用してデータセットを作成します。 XDMスキーマについて詳しくは、 [XDM System overview](../../xdm/home.md) and basics of [system compositionを参照してください](../../xdm/schema/composition.md)。

新しいデータセットを作成するには、 **[!UICONTROL Datasets]** ワークスペースの右上隅にある「データセットを作成」をクリ _[!UICONTROL ックします]_。

![](../images/labels/create_dataset.png)

データセ _[!UICONTROL ットの作成]_画面が表示されます。 ここから、「データセットを**[!UICONTROL &#x200B;スキーマから作成&#x200B;]**」

![「Create Dataset from」スキーマ](../images/labels/dataset_create.png)

[ _[!UICONTROL スキーマの選択]_]画面が表示され、リストセットの作成に使用できるすべてのスキーマが選択されます。 選択するには、スキーマの横のラジオボタンをクリックします。 右側の_[!UICONTROL &#x200B;スキーマ]_ ・セクションには、選択したスキーマの詳細が表示されます。 Once you have selected a schema, click **[!UICONTROL Next]**.

![データセットの選択スキーマ](../images/labels/dataset_schema.png)

データセ _ットの設定_ 画面が表示されます。 新しいデ **ータセットの名前** （必須） **と説明** （任意ですが推奨）を入力し、「完了」をクリック **[!UICONTROL します]**。

![名前と説明を使用したデータセットの設定](../images/labels/dataset_configure.png)

「データセ _[!UICONTROL ットアクティビティ]_」ページが開き、新しく作成したデータセットに関する情報が表示されます。 この例では、データセットの名前は「Loyalty Members」なので、トップナビゲーションには_ Datasets/Loyalty Membersと表示されます&#x200B;_。

![データセットアクティビティページ](../images/labels/dataset_activity.png)

### データセ追加ットのデータ使用ラベル {#add-labels}

新しいデータセットを作成するか、 _[!UICONTROL Datasets]_ワークスペースのリストから既存のデータセットを選択したら、「**[!UICONTROL  Data Governance ]**」をクリックして_[!UICONTROL  Data Governance]_ ワークスペースを開きます。 ワークスペースでは、データセットレベルとフィールドレベルでデータ使用ラベルを管理できます。

![「Dataset Data Governance」タブ](../images/labels/dataset_data_governance.png)

データセットレベルでデータ使用ラベルを編集するには、開始セット名の横にある鉛筆アイコンをクリックします。

![データセットレベルのラベルの編集](../images/labels/dataset_labels_edit_button.png)

[ _[!UICONTROL Edit Governance Labels]_]ダイアログが開きます。 ダイアログ内で、データセットに適用するラベルの横にあるチェックボックスをオンにします。 これらのラベルは、データセット内のすべてのフィールドに継承されます。 「適用_[!UICONTROL &#x200B;されたラベル]_ 」ヘッダーは、選択したラベルを表示する各チェックボックスをオンにすると更新されます。 目的のラベルを選択したら、「変更の保存」をク **[!UICONTROL リックします]**。

<img alt="データセットレベルでのガバナンスラベルの適用" src="../images/labels/apply-labels-dataset.png" width="700"><br>

データ _[!UICONTROL 管理ワークスペースが]_、データセットレベルで適用したラベルを表示します。 また、ラベルがデータセット内の各フィールドに継承されることもわかります。

![フィールドに継承されるデータセットラベル](../images/labels/dataset_inherited_labels.png)

データセットレベルでラベルの横に「x」が表示され、ラベルを削除できます。 各フィールドの横にある継承されたラベルの横には「x」が表示されず、「灰色表示」に表示され、削除や編集はできません。 これは、継承され **たフィールドは読み取り専用で**、フィールドレベルでは削除できないためです。

「継承 **[!UICONTROL されたラベルを表示]** 」はデフォルトでオンになっており、データセットからフィールドに継承されたラベルを表示できます。 切り替えをオフにすると、データセット内の継承されたラベルが非表示になります。

![継承されたラベルを非表示にする](../images/labels/hide_inherited_labels.png)

## データセットフィールドレベルでのデータ使用ラベルの管理

データセットレベルでのデ [ータ使用量ラベルの追加と編集のワークフローを継続して](#add-labels)、そのデータセットの _[!UICONTROL Data Governance]_Workspace内のフィールドレベルのラベルを管理することもできます。

データ使用ラベルを個々のフィールドに適用するには、フィールド名の横にあるチェックボックスを選択し、「ガバナンスラベルの編集」 **[!UICONTROL をクリックしま]**&#x200B;す。

![フィールドラベルの編集](../images/labels/fields_single_field.png)

[ _[!UICONTROL Edit Governance Labels]_]ダイアログが表示されます。 このダイアログには、選択したフィールド、適用されたラベル、継承されたラベルを示すヘッダーが表示されます。 継承されたラベル（C2とC5）は、ダイアログで灰色表示になっています。 これらのラベルは、データセットレベルから継承された読み取り専用のラベルなので、データセットレベルでのみ編集できます。

<img alt="個々のフィールドのガバナンスラベルの編集" src="../images/labels/field-label-inheritance.png" width="700"><br>

使用する各ラベルの横にあるチェックボックスをクリックして、フィールドレベルのラベルを選択します。 ラベルを選択すると、「適用されたラ _[!UICONTROL ベル]_」ヘッダーが更新され、「選択されたフィールド」ヘッダーに表示されるフィールドに適用されたラベル_[!UICONTROL &#x200B;が表示されます]_ 。 フィールドレベルのラベルの選択が完了したら、「変更の保存」をク **[!UICONTROL リックしま]**&#x200B;す。

<img alt="フィールドレベルのラベルの適用" src="../images/labels/apply-labels-field.png" width="700"><br>

[ _[!UICONTROL Data Governance]_]ワークスペースが再び表示され、選択したフィールドレベルのラベルがフィールド名の横の行に表示されます。 フィールドレベルのラベルの横には「x」が表示され、ラベルを削除できます。

![フィールドレベルのラベルを表示するフィールド](../images/labels/fields_show_field_level_labels.png)

この手順を繰り返すと、フィールドレベルのラベルを追加のフィールドに対して追加および編集し続けることができます。複数のフィールドを選択して、フィールドレベルのラベルを同時に適用することもできます。

![複数のフィールドを選択して、フィールドレベルのラベルを同時に適用します。](../images/labels/fields_select_multiple.png)

継承の移動は最上位レベルから下（データセット→フィールド）にのみ行われるので、フィールドレベルで適用されたラベルは他のフィールドやデータセットには反映されません。

## 次の手順

これで、データセットおよびフィールドレベルでデータ使用ラベルを追加したので、Experience Platformへのデータの取り込みを開始できます。 詳しくは、開始の取り込みに関するドキュメ [ントを参照してくださ](../../ingestion/home.md)い。

適用したラベルに基づいてデータ使用ポリシーを定義することもできます。 詳しくは、データ使用ポリシーの概 [要を参照してください](../policies/overview.md)。

## その他のリソース

次のビデオは、データガバナンスの理解を深め、データセットと個々のフィールドにラベルを適用する方法の概要を説明しています。

>[!VIDEO](https://video.tv.adobe.com/v/29709?quality=12&enable10seconds=on&speedcontrol=on)
