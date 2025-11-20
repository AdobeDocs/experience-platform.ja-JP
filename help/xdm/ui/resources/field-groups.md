---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: UI でのスキーマフィールドグループの作成と編集
description: Experience Platform ユーザーインターフェイスでスキーマフィールドグループを作成および編集する方法について説明します。
exl-id: 928d70a6-0468-4fb7-a53a-6686ac77f2a3
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '966'
ht-degree: 9%

---

# UI でのスキーマフィールドグループの作成と編集 {#ui-create-and-edit}

>[!CONTEXTUALHELP]
>id="platform_schemas_fieldgroup_filter"
>title="標準またはカスタムのフィールドグループフィルター"
>abstract="使用可能なフィールドグループのリストは、作成方法に基づいて事前にフィルタリングされています。ラジオボタンを選択して、「標準」オプションと「カスタム」オプションのいずれかを選択します。「標準」オプションは、アドビが作成したエンティティを表示し、「カスタム」オプションは、組織内で作成したエンティティを表示します。フィールドグループの作成と編集について詳しくは、ドキュメントを参照してください。"

エクスペリエンスデータモデル（XDM）では、スキーマフィールドグループは、個人の詳細情報、ホテルの環境設定、住所などの特定の機能を実装する 1 つ以上のフィールドを定義する、再利用可能なコンポーネントです。 フィールドグループは、互換性のあるクラスを実装するスキーマの一部として含めることを目的としています。

フィールドグループは、フィールドグループが表すデータ（レコードまたは時系列）の動作に基づいて、互換性のあるクラスを定義します。 つまり、すべてのフィールドグループをすべてのクラスで使用できるわけではありません。

Adobe Experience Platformには、様々なマーケティングユースケースをカバーする多くの標準フィールドグループが用意されています。 ただし、独自のカスタムフィールドグループを作成および編集して、XDM スキーマ内のビジネスに関連する追加の概念を定義することもできます。 このガイドでは、Experience Platform UI で組織のカスタムフィールドグループを作成、編集、管理する方法の概要を説明します。

## 前提条件 {#prerequisites}

このガイドでは、XDM システムに関する十分な知識が必要です。 Experience Platform エコシステム内のXDMの役割の概要については[XDMの概要](../../home.md)およびフィールド グループがXDM スキーマにどのように貢献するかについて[スキーマ構成の](../../schema/composition.md)基本を参照してください。

このガイドでは必須ではありませんが、 [UIでのスキーマの作成](../../tutorials/create-schema-ui.md) に関するチュートリアルもフォローするして、 [!DNL Schema Editor]のさまざまな機能を理解することをお勧めします。

## 新しいフィールドグループの作成 {#create}

新しいフィールドグループを作成するには、まずフィールドグループの追加先となるスキーマを選択する必要があります。 [ 新しいスキーマを作成する ](./schemas.md#create) または [ 編集する既存のスキーマを選択する ](./schemas.md#edit) を選択できます。

[!DNL Schema Editor] でスキーマを開いたら、左側のパネルの **[!UICONTROL Add]** セクションの横にある「[!UICONTROL Field groups]」を選択します。

![](../../images/ui/resources/field-groups/add-field-group.png)

表示されるダイアログで、「**[!UICONTROL Create new field group]**」を選択します。 ここでは、フィールドグループの **[!UICONTROL Display name]** と **[!UICONTROL Description]** を指定できます。 終了したら「**[!UICONTROL Add field groups]**」を選択します。

![](../../images/ui/resources/field-groups/create-field-group.png)

[!DNL Schema Editor] が再び表示され、新しいフィールドグループが左側のパネルにリストされます。 これは全く新しいフィールドグループなので、現在スキーマにフィールドを提供していません。そのため、キャンバスは変更されません。 これで、[ フィールドグループへのフィールドの追加 ](#add-fields) を開始できます。

![](../../images/ui/resources/field-groups/field-group-added.png)

## フィールドグループのフィルタリング {#filter}

使用可能なフィールドグループのリストは、作成方法に基づいて事前にフィルタリングされています。デフォルト設定には、Adobeで定義されたフィールドグループが表示されます。 ただし、リストをフィルタリングして、組織で作成したリストを表示することもできます。 ラジオボタンを選択して、「[!UICONTROL Standard]」オプションと「[!UICONTROL Custom]」オプションのいずれかを選択します。 「[!UICONTROL Standard]」オプションを選択すると、Adobeで作成されたエンティティが表示され、「[!UICONTROL Custom]」オプションを選択すると、組織内で作成されたエンティティが表示されます。

![[!UICONTROL Field groups] と [!UICONTROL Schemas] がハイライト表示された [!UICONTROL Standard] ワークスペースの「[!UICONTROL Custom]」タブ。](../../images/ui/resources/field-groups/standard-and-custom-field-groups.png)

## 既存のフィールドグループの編集 {#edit}

>[!NOTE]
>
>完全に編集およびカスタマイズできるのは、組織で定義されたカスタムフィールドグループのみです。 Adobeで定義されたコアフィールドグループの場合、個々のスキーマのコンテキスト内で編集できるのは、そのフィールドの表示名のみです。 これらのフィールドは、スキーマエディターで南京錠アイコン（![ 南京錠アイコン](/help/images/icons/lock-closed.png)）に設定します。 詳しくは、[ スキーマフィールドの表示名の編集 ](./schemas.md#display-names) の節を参照してください。
>
>カスタムフィールドグループを保存し、データ取り込みのスキーマで使用すると、それ以降はフィールドグループに追加の変更を加えることのみ可能です。 詳しくは、[ スキーマ進化のルール ](../../schema/composition.md#evolution) を参照してください。

既存のフィールド グループを編集するには、まず、 [!DNL Schema Editor]内のフィールド グループを使用するスキーマを開く必要があります。 既存のスキーマを [選択して編集するか](./schemas.md#edit)新しいスキーマを [作成](./schemas.md#create) して目的のフィールドグループを追加することができます。

編集者でスキーマを開いたら、フィールドへのフィールドの追加 [グループ](#add-fields)開始できます。

## フィールドグループへのフィールドの追加 {#add-fields}

>[!NOTE]
>
>この節では、カスタムフィールドグループへのフィールドの追加に焦点を当てます。 標準フィールドグループにカスタムフィールドを追加する方法については、[ スキーマ UI ガイド ](./schemas.md#custom-fields-for-standard-groups) を参照してください。

カスタムフィールドグループにフィールドを追加するには、まず、キャンバスでスキーマ名の横にある **プラス（+）** アイコンを選択します。

![](../../images/ui/resources/field-groups/add-field.png)

キャンバスに **[!UICONTROL Untitled Field]** プレースホルダーが表示されます。また、右側のパネルが更新されて、フィールドのプロパティを設定するためのコントロールが表示されます。 様々なフィールドタイプの設定方法に関する具体的な手順については、[UI でのフィールドの定義 ](../fields/overview.md#define) に関するガイドを参照してください。

「**[!UICONTROL Assign to]**」で「**[!UICONTROL Field Group]**」オプションを選択し、ドロップダウンを使用してリストから目的のフィールドグループを選択します。 フィールドグループの名前を入力して、結果を絞り込むことができます。

![](../../images/ui/resources/field-groups/select-field-group.png)

「**[!UICONTROL Assign to]**」で「**[!UICONTROL Field Group]**」オプションを選択し、ドロップダウンを使用してリストから目的のフィールドグループを選択します。 フィールドグループの名前を入力して、結果を絞り込むことができます。

![](../../images/ui/resources/field-groups/select-field-group.png)

フィールドをスキーマに追加すると、選択したフィールドグループに割り当てられます。 引き続き、フィールドグループに必要な数のフィールドを追加します。 終了したら、「**[!UICONTROL Save]**」を選択して、スキーマとフィールドグループの両方を保存します。

![](../../images/ui/resources/field-groups/complete-field-group.png)

同じフィールドグループが他のスキーマで既に使用されている場合、新しく追加されたフィールドはこれらのスキーマに自動的に表示されます。

## 次の手順 {#next-steps}

このガイドでは、Experience Platform UI を使用してフィールドグループを作成および編集する方法について説明しました。 [!UICONTROL Schemas] workspace の機能について詳しくは、[[!UICONTROL Schemas] workspace の概要を参照してください ](../overview.md)

[!DNL Schema Registry] API を使用してフィールドグループを管理する方法については、[ フィールドグループエンドポイントガイド ](../../api/field-groups.md) を参照してください。
