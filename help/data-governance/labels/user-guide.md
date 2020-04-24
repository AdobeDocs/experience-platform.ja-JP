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

データセットレベルでデータ使用量ラベルを管理するには、既存のデータセットを選択するか、新しいデータセットを作成する必要があります。 Adobe Experience Platformにログインした後、左側のナビゲ **[!UICONTROL Datasets]** ーションでを選択し、データセットワークスペース _を開き_ ます。 このページのリストでは、組織に属するデータセットを作成し、各データセットに関連する有用な詳細情報を示します。

![Data Workspace内の「データセット」タブ](../images/labels/datasets.png)

次の節では、ラベルを適用する新しいデータセットを作成する手順を説明します。 既存のデータセットのラベルを編集する場合は、リストからデータセットを選択し、データセットにデー [タ使用ラベルを追加する前に進みます](#add-labels)。

### 新しいデータセットの作成

>[!NOTE] この例では、事前設定のExperience Data Model(XDM)スキーマを使用してデータセットを作成します。 XDMスキーマについて詳しくは、 [XDM System overview](../../xdm/home.md) and basics of [system compositionを参照してください](../../xdm/schema/composition.md)。

新しいデータセットを作成するに **[!UICONTROL Create Dataset]** は、ワークスペースの右上隅にあるをクリック _[!UICONTROL Datasets]_します。

![](../images/labels/create_dataset.png)

画面が _[!UICONTROL Create Dataset]_表示されます。 ここからをクリックしま&#x200B;**[!UICONTROL Create Dataset from Schema]**す。

![「Create Dataset from」スキーマ](../images/labels/dataset_create.png)

画面が _[!UICONTROL Select Schema]_表示され、リストセットの作成に使用できるスキーマがすべて表示されます。 選択するには、スキーマの横のラジオボタンをクリックします。 右側_[!UICONTROL Schemas]_ のセクションには、選択したスキーマの詳細が表示されます。 オプションを選択したら、スキーマをクリックしま **[!UICONTROL Next]**&#x200B;す。

![データセットの選択スキーマ](../images/labels/dataset_schema.png)

データセ _ットの設定_ 画面が表示されます。 新しいデ **ータセットの名前** （必須）と **説明** （任意ですが、推奨）を入力し、をクリックしま **[!UICONTROL Finish]**&#x200B;す。

![名前と説明を使用したデータセットの設定](../images/labels/dataset_configure.png)

ページが _[!UICONTROL Dataset Activity]_開き、新しく作成したデータセットに関する情報が表示されます。 この例では、データセットの名前は「Loyalty Members」なので、トップナビゲーションには_ Datasets/Loyalty Membersと表示されます&#x200B;_。

![データセットアクティビティページ](../images/labels/dataset_activity.png)

### データセ追加ットのデータ使用ラベル {#add-labels}

新しいデータセットを作成した後、またはワークスペース内のリストから既存のデータセットを _[!UICONTROL Datasets]_選択した後、をクリ&#x200B;**[!UICONTROL Data Governance]**ックしてワークスペースを_[!UICONTROL Data Governance]_ 開きます。 ワークスペースでは、データセットレベルとフィールドレベルでデータ使用ラベルを管理できます。

![「Dataset Data Governance」タブ](../images/labels/dataset_data_governance.png)

データセットレベルでデータ使用ラベルを編集するには、開始セット名の横にある鉛筆アイコンをクリックします。

![データセットレベルのラベルの編集](../images/labels/dataset_labels_edit_button.png)

ダイアログ _[!UICONTROL Edit Governance Labels]_が開きます。 ダイアログ内で、データセットに適用するラベルの横にあるチェックボックスをオンにします。 これらのラベルは、データセット内のすべてのフィールドに継承されます。 各ボッ_[!UICONTROL Applied Labels]_ クスをオンにするとヘッダーが更新され、選択したラベルが表示されます。 目的のラベルを選択したら、をクリックしま **[!UICONTROL Save Changes]**&#x200B;す。

<img alt="データセットレベルでのガバナンスラベルの適用" src="../images/labels/apply-labels-dataset.png" width="700"><br>

ワーク _[!UICONTROL Data Governance]_スペースが再び表示され、データセットレベルで適用したラベルが表示されます。 また、ラベルがデータセット内の各フィールドに継承されることもわかります。

![フィールドに継承されるデータセットラベル](../images/labels/dataset_inherited_labels.png)

データセットレベルでラベルの横に「x」が表示され、ラベルを削除できます。 各フィールドの横にある継承されたラベルの横には「x」が表示されず、「灰色表示」に表示され、削除や編集はできません。 これは、継承され **たフィールドは読み取り専用で**、フィールドレベルでは削除できないためです。

デフォ **[!UICONTROL Show Inherited Labels]** ルトでは、切り替えがオンになっており、データセットからフィールドに引き継がれたラベルを表示できます。 切り替えをオフにすると、データセット内の継承されたラベルが非表示になります。

![継承されたラベルを非表示にする](../images/labels/hide_inherited_labels.png)

## データセットフィールドレベルでのデータ使用ラベルの管理

データセットレベルでのデ [ータ使用量ラベルの追加と編集のワークフローを継続して](#add-labels)、そのデータセットのワークスペース内でフィールドレベルのラベルを管理する _[!UICONTROL Data Governance]_こともできます。

データ使用ラベルを個々のフィールドに適用するには、フィールド名の横にあるチェックボックスを選択し、をクリックしま **[!UICONTROL Edit Governance Labels]**&#x200B;す。

![フィールドラベルの編集](../images/labels/fields_single_field.png)

ダイアログ _[!UICONTROL Edit Governance Labels]_が表示されます。 このダイアログには、選択したフィールド、適用されたラベル、継承されたラベルを示すヘッダーが表示されます。 継承されたラベル（C2とC5）は、ダイアログで灰色表示になっています。 これらのラベルは、データセットレベルから継承された読み取り専用のラベルなので、データセットレベルでのみ編集できます。

<img alt="個々のフィールドのガバナンスラベルの編集" src="../images/labels/field-label-inheritance.png" width="700"><br>

使用する各ラベルの横にあるチェックボックスをクリックして、フィールドレベルのラベルを選択します。 ラベルを選択すると、ヘッダーが更新 _[!UICONTROL Applied Labels]_され、ヘッダーに表示されるフィールドに適用されたラベルが表示さ_[!UICONTROL Selected Fields]_ れます。 フィールドレベルのラベルの選択が完了したら、をクリックしま **[!UICONTROL Save Changes]**&#x200B;す。

<img alt="フィールドレベルのラベルの適用" src="../images/labels/apply-labels-field.png" width="700"><br>

ワーク _[!UICONTROL Data Governance]_スペースが再び表示され、選択したフィールドレベルのラベルがフィールド名の横の行に表示されます。 フィールドレベルのラベルの横には「x」が表示され、ラベルを削除できます。

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
