---
keywords: Experience Platform;home;popular topics;data governance;data usage label;policy service;data usage labels user guide
solution: Experience Platform
title: データ使用状況ラベルのユーザーガイド
topic: labels
description: このユーザガイドでは、Adobe Experience Platformのユーザインターフェイス内でデータ使用ラベル（DULEラベルとも呼ばれます）を使用する手順を説明します。
translation-type: tm+mt
source-git-commit: c6c5ada52321b11543254f80399c38365f0fb9d7
workflow-type: tm+mt
source-wordcount: '1132'
ht-degree: 79%

---


# データ使用状況ラベルのユーザーガイド

This user guide covers steps for working with data usage labels (also known as DULE labels) within the [!DNL Experience Platform] user interface. このガイドを使用する前に、「[データガバナンスの概要](../home.md)」で DULE フレームワークの詳細を参照してください。

## データセットレベルでのデータ使用状況ラベルの管理

データセットレベルでデータ使用状況ラベルを管理するには、既存のデータセットを選択するか、新しいデータセットを作成する必要があります。Adobe Experience Platform にログインした後、左側のナビゲーションで「**[!UICONTROL データセット]**」を選択し、「_データセット_」ワークスペースを開きます。このページには、組織に属するすべての作成済みデータセットと、各データセットに関する有用な詳細情報がリストされます。

![データワークスペース内の「データセット」タブ](../images/labels/datasets.png)

次の節では、ラベルの適用先の新しいデータセットを作成する手順を説明します。既存のデータセットのラベルを編集する場合は、リストからデータセットを選択し、「[データセットへのデータ使用状況ラベルの追加](#add-labels)」に進みます。

### 新しいデータセットの作成

>[!NOTE]
>
>In this example, a dataset is created using a pre-configured [!DNL Experience Data Model] (XDM) schema. XDM スキーマについて詳しくは、「[XDM システムの概要](../../xdm/home.md)」と「[スキーマ構成の基本](../../xdm/schema/composition.md)」を参照してください。

新しいデータセットを作成するには、「**[!UICONTROL データセット]**」ワークスペースの右上にある「**[!UICONTROL データセットを作成]**」をクリックします。

![](../images/labels/create_dataset.png)

「**[!UICONTROL データセットを作成]**」画面が表示されます。ここで、「**[!UICONTROL スキーマからデータセットを作成]**」をクリックします。

![スキーマからデータセットを作成](../images/labels/dataset_create.png)

「**[!UICONTROL スキーマを選択]**」画面が表示され、データセットの作成に使用できるすべてのスキーマが示されます。スキーマの横にあるラジオボタンをクリックして、スキーマを選択します。右側の「**[!UICONTROL スキーマ]**」セクションには、選択したスキーマの追加の詳細が表示されます。スキーマを選択したら、「**[!UICONTROL 次へ]**」をクリックします。

![データセットスキーマの選択](../images/labels/dataset_schema.png)

「**[!UICONTROL データセットを設定]**」画面が表示されます。新しいデータセットの&#x200B;**名前**（必須）と&#x200B;**説明**（任意ですが推奨）を指定し、「**[!UICONTROL 完了]**」をクリックします。

![データセットの名前と説明を設定](../images/labels/dataset_configure.png)

「**[!UICONTROL データセットアクティビティ]**」ページが開き、新しく作成したデータセットに関する情報が表示されます。この例では、データセットの名前は「ロイヤルティーメンバー」なので、トップナビゲーションには&#x200B;**データセット／ロイヤルティーメンバー**&#x200B;と表示されます。

![「データセットアクティビティ」ページ](../images/labels/dataset_activity.png)

### データセットへのデータ使用状況ラベルの追加 {#add-labels}

「**[!UICONTROL データセット]**」ワークスペースで新しいデータセットを作成するか、リストから既存のデータセットを選択したら、「**[!UICONTROL データガバナンス]**」をクリックして「**[!UICONTROL データガバナンス]**」ワークスペースを開きます。ワークスペースでは、データセットレベルとフィールドレベルでデータ使用状況ラベルを管理できます。

![データセットの「データガバナンス」タブ](../images/labels/dataset_data_governance.png)

データセットレベルでデータ使用状況ラベルを編集するには、まずデータセット名の横にある鉛筆アイコンをクリックします。

![データセットレベルでラベルを編集](../images/labels/dataset_labels_edit_button.png)

「**[!UICONTROL ガバナンスラベルを編集]**」ダイアログが開きます。ダイアログ内で、データセットに適用するラベルの横にあるボックスをオンにします。これらのラベルは、データセット内のすべてのフィールドに継承されることに注意してください。各ボックスをオンにすると、「**[!UICONTROL 適用されたラベル]**」ヘッダーが更新され、選択したラベルが表示されます。目的のラベルを選択したら、「**[!UICONTROL 変更を保存]**」をクリックします。

<img alt="データセットレベルでガバナンスラベルを適用" src="../images/labels/apply-labels-dataset.png" width="700"><br>

「**[!UICONTROL データガバナンス]**」ワークスペースが再び表示され、データセットレベルで適用したラベルが示されます。また、ラベルがデータセット内の各フィールドに継承されていることも確認できます。

![フィールドに継承されるデータセットラベル](../images/labels/dataset_inherited_labels.png)

データセットレベルでラベルの横に「x」が表示されていることに注意してください。この場合、ラベルを削除できます。各フィールドの継承されたラベルの横には「x」がなく、「灰色表示」になっています。これらのラベルは削除したり、編集したりできません。これは、**継承されたフィールドは読み取り専用**&#x200B;で、フィールドレベルでは削除できないためです。

「**[!UICONTROL 継承されたラベルを表示]**」トグルはデフォルトでオンになっており、データセットからフィールドに継承されたラベルを表示できます。トグルをオフに切り替えると、データセット内の継承されたラベルが非表示になります。

![継承されたラベルを非表示にする](../images/labels/hide_inherited_labels.png)

## データセットフィールドレベルでのデータ使用状況ラベルの管理

[データセットレベルでのデータ使用状況ラベルの追加と編集](#add-labels)のワークフローを継続して、そのデータセットの「**[!UICONTROL データガバナンス]**」ワークフロー内のフィールドレベルのラベルを管理することもできます。

データ使用状況ラベルを個々のフィールドに適用するには、フィールド名の横にあるチェックボックスをオンにし、「**[!UICONTROL ガバナンスラベルを編集]**」をクリックします。

![フィールドラベルの編集](../images/labels/fields_single_field.png)

「**[!UICONTROL ガバナンスラベルを編集]**」ダイアログが表示されます。このダイアログには、選択されたフィールド、適用されたラベル、継承されたラベルを示すヘッダーが表示されます。継承されたラベル（C2 と C5）は、ダイアログで灰色表示になることに注意してください。これらのラベルは、データセットレベルから継承された読み取り専用のラベルなので、データセットレベルのみで編集できます。

<img alt="個々のフィールドのガバナンスラベルの編集" src="../images/labels/field-label-inheritance.png" width="700"><br>

使用する各ラベルの横にあるチェックボックスをオンにして、フィールドレベルのラベルを選択します。ラベルを選択すると、「**[!UICONTROL 適用されたラベル]**」ヘッダーが更新され、「**[!UICONTROL 選択されたフィールド]**」ヘッダーに表示されるフィールドに適用されるラベルが表示されます。フィールドレベルのラベルの選択が完了したら、「**[!UICONTROL 変更を保存]**」をクリックします。

<img alt="フィールドレベルのラベルの適用" src="../images/labels/apply-labels-field.png" width="700"><br>

「**[!UICONTROL データガバナンス]**」ワークスペースが再び表示され、フィールド名の横の行にフィールドレベルの選択済みラベルが表示されます。フィールドレベルのラベルの横には「x」が表示され、ラベルを削除できます。

![フィールドレベルのラベルを表示するフィールド](../images/labels/fields_show_field_level_labels.png)

これらの手順を繰り返して、追加のフィールドに対してフィールドレベルのラベルの追加と編集を続けることができます。複数のフィールドを選択して、フィールドレベルのラベルを同時に適用することもできます。

![複数のフィールドを選択して、フィールドレベルのラベルを同時に適用します。](../images/labels/fields_select_multiple.png)

継承は最上位レベルから下のレベル（データセットからフィールド）のみに移動するため、フィールドレベルで適用されたラベルは他のフィールドやデータセットには反映されないことに注意する必要があります。

## カスタムラベルの管理

UIの「 **[!UICONTROL ポリシー]** 」ワークスペース内に、独自のカスタム使用ラベルを作成でき [!DNL Experience Platform] ます。 左側のナビゲーションで「 **[!UICONTROL ポリシー]** 」をクリックし、「 **[!UICONTROL ラベル]** 」をクリックして既存のラベルのリストを表示します。 ここから、「ラベルを **[!UICONTROL 作成]**」をクリックします。

![](../images/labels/create-label-btn.png)

[ **[!UICONTROL ラベルを作成]** ]ダイアログが表示されます。 ここから、新しいラベルに次の情報を入力します。

* **[!UICONTROL 識別子]**:ラベルの一意の識別子。 この値は参照用に使用するので、短く簡潔にする必要があります。
* **[!UICONTROL 名前]**:ラベルのわかりやすい表示名。
* **[!UICONTROL 説明]**:（オプション）詳細なコンテキストを提供するためのラベルの説明。

完了したら、「**[!UICONTROL Create]**」をクリックします。

![](../images/labels/create-label.png)

ダイアログが閉じ、新しく作成したカスタムラベルが「 **[!UICONTROL ラベル]** 」タブのリストに表示されます。

![](../images/labels/label-created.png)

データセットおよびフィールドの使用ラベルを編集する場合や、データ使用ポリシーを作成する場合に、「 **[!UICONTROL カスタムラベル]** 」でラベルを選択できるようになりました。

<img src="../images/labels/add-custom-label.png" width="600" /><br>

## 次の手順

Now that you have added data usage labels at the dataset and field level, you can begin to ingest data into [!DNL Experience Platform]. 詳しくは、[データ取得ドキュメント](../../ingestion/home.md)を参照してください。

適用したラベルに基づいてデータ使用状況ポリシーを定義することもできます。詳しくは、「[データ使用状況ポリシーの概要](../policies/overview.md)」を参照してください。

## その他のリソース

The following video is intended to support your understanding of [!DNL Data Governance], and outlines how to apply labels to a dataset and individual fields.

>[!VIDEO](https://video.tv.adobe.com/v/29709?quality=12&enable10seconds=on&speedcontrol=on)
