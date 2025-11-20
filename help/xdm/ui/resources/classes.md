---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；クラス；クラス；
solution: Experience Platform
title: UI でのクラスの作成と編集
description: Experience Platform ユーザーインターフェイスでクラスを作成および編集する方法について説明します。
exl-id: 1b4c3996-2319-45dd-9edd-a5bcad46578b
source-git-commit: a05ee385694b028b513e2fa632079e665ba815bb
workflow-type: tm+mt
source-wordcount: '1562'
ht-degree: 8%

---

# UI でのクラスの作成と編集 {#ui-create-and-edit}

>[!CONTEXTUALHELP]
>id="platform_schemas_class_filter"
>title="標準またはカスタムのクラスフィルター"
>abstract="使用可能なクラスのリストは、作成方法に基づいて事前にフィルタリングされています。ラジオボタンを選択して、「標準」オプションと「カスタム」オプションのいずれかを選択します。「標準」オプションは、アドビが作成したエンティティを表示し、XDM 個人プロファイルクラスと XDM エクスペリエンスイベントクラスの両方を含みます。「カスタム」オプションは、組織内で作成したエンティティを表示します。クラスの作成と編集について詳しくは、ドキュメントを参照してください。"

Adobe Experience Platformでは、スキーマのクラスは、スキーマに含まれるデータ（レコードまたは時系列）の行動の側面を定義します。 これに加えて、クラスは、そのクラスに基づくすべてのスキーマに含める必要のある共通のプロパティの最小数を記述し、複数の互換性のあるデータセットを結合する方法を提供します。

Adobeは、（XDM 個人プロファイル）や [XDM エクスペリエンスイベント &#x200B;](../../classes/individual-profile.md) など、いくつかの標準（「コア」）エクスペリエンスデータモデル（XDM[&#x200B; クラス &#x200B;](../../classes/experienceevent.md) 提供しています。 これらのコアクラスに加えて、独自のカスタムクラスを作成して、組織のより具体的な使用例を記述することもできます。

このドキュメントでは、Experience Platform UI でカスタムクラスを作成、編集、管理する方法の概要を説明します。

## 前提条件 {#prerequisites}

このガイドでは、XDM システムに関する十分な知識が必要です。 クラスが XDM スキーマにどのように寄与するかを学ぶには、Experience Platform エコシステムにおける XDM の役割の概要および [&#x200B; スキーマ構成の基本 &#x200B;](../../home.md) について [XDM の概要 &#x200B;](../../schema/composition.md) を参照してください。

このガイドには必須ではありませんが、[UI でのスキーマの作成 &#x200B;](../../tutorials/create-schema-ui.md) に関するチュートリアルに従って、スキーマエディターの様々な機能を理解することをお勧めします。

## はじめに {#getting-started}

Experience Platform UI で、左側のナビゲーションで **[!UICONTROL Schemas]** を選択して [!UICONTROL Schemas] Workspace を開き、「**[!UICONTROL Classes]**」タブを選択します。 使用可能なクラスのリストが表示されます。

![[!UICONTROL Classes] Workspace [!UICONTROL Schemas] および [!UICONTROL Classes] の [!UICONTROL Schemas] タブ内のクラスのリストがハイライト表示されています。](../../images/ui/resources/classes/available-classes.png)

## クラスをフィルター {#filter}

クラスのリストは、作成方法に基づいて自動的にフィルタリングされます。 デフォルト設定には、Adobeで定義されたクラスが表示されます。 また、リストをフィルタリングして、組織によって作成されたものを表示することもできます。 ラジオボタンを選択して、 [!UICONTROL Standard] オプションと [!UICONTROL Custom] オプションから選択します。 [ [!UICONTROL Standard] ] オプションを選択すると、Adobe Systemsによって作成されたエンティティが表示され、[ [!UICONTROL Custom] ] オプションを選択すると、組織内で作成されたエンティティが表示されます。

![[!UICONTROL Classes]の[!UICONTROL Schemas]タブは、[!UICONTROL Standard]と[!UICONTROL Custom]が強調表示された状態でワークスペースされます。](../../images/ui/resources/classes/standard-and-custom-classes.png)

>[!TIP]
>
>検索機能を使用して、名前に基づいてクラスをフィルター処理または検索します。 詳しくは、 [XDMリソースの探査](../explore.md) に関するガイドを参照してください。

## 新しいクラスの作成 {#create}

Experience Platform UIにクラスを作成するには、 **[!UICONTROL Create class]** メソッドと **[!UICONTROL Create schema]** メソッドの 2 つがあります。

### クラスを作成

**[!UICONTROL Create class]**&#x200B;ワークスペースの[!UICONTROL Classes]タブから[!UICONTROL Schemas]を選択します。

![[!UICONTROL Classes]が強調表示された[!UICONTROL Schemas]ワークスペースの[!UICONTROL Create class]タブ](../../images/ui/resources/classes/create-class.png)

[!UICONTROL Create class]ダイアログが表示されます。クラスの [!UICONTROL Display name] と [!UICONTROL Description] を入力し、ラジオボタンを使用してクラスの目的の動作を選択します。 クラスの型は [!UICONTROL Record] または [!UICONTROL Time-series] にすることができます。 「**[!UICONTROL Create]**」を選択して選択内容を確定し、「[!UICONTROL Classes]」タブに戻ります。

![[!UICONTROL Create class] がハイライト表示された [!UICONTROL Create] ダイアログ &#x200B;](../../images/ui/resources/classes/create-class-dialog.png)

作成したクラスが使用可能になり、[!UICONTROL Classes] ビューに表示されます。

![&#x200B; 最近作成したクラスがハイライト表示されている [!UICONTROL Classes] ワークスペースの「[!UICONTROL Schemas]」タブ &#x200B;](../../images/ui/resources/classes/new-class-listing.png)

### スキーマを作成

または、スキーマを手動で作成してクラスを作成することもできます。 **[!UICONTROL Create schema]** ワークスペースの「[!UICONTROL Classes]」タブから「[!UICONTROL Schemas]」を選択します。

![[!UICONTROL Classes] がハイライト表示された [!UICONTROL Schemas] ワークスペースの「[!UICONTROL Create schema]」タブ &#x200B;](../../images/ui/resources/classes/create-schema.png)

表示される **[!UICONTROL Manual]** ダイアログで「[!UICONTROL Create a schema]」を選択します。

>[!NOTE]
>
>ML で支援されるスキーマ作成ワークフローを使用する場合は、ファイルをアップロードし、ML アルゴリズムを使用して推奨スキーマを生成できます。 そのスキーマ作成ワークフローでは、スキーマの基本クラスを指定する必要はありません。 ML が CSV ファイルに基づいてスキーマ構造をレコメンデーションする方法については、[&#x200B; 機械学習を利用したスキーマ作成ガイド &#x200B;](../ml-assisted-schema-creation.md) を参照してください。

![&#x200B; ワークフローオプションと「選択」がハイライト表示されたスキーマを作成ダイアログ &#x200B;](../../images/ui/resources/classes/manually-create-a-schema.png)

スキーマ作成ワークフローが表示されます。 「[!UICONTROL Schema details]」セクションで、「**[!UICONTROL Other]**」を選択します。 使用可能なクラスのリストが表示されます。 **[!UICONTROL Create class]** を選択します。

![[!UICONTROL Create schema] のセクションで [!UICONTROL Other] がハイライト表示されている [!UICONTROL Schema details] ワークフロー &#x200B;](../../images/ui/resources/classes/other-schema-details.png)

[!UICONTROL Create class] ダイアログが表示されます。 クラスの [!UICONTROL Display name] と [!UICONTROL Description] を入力し、ラジオボタンでクラスの意図した動作を選択します。 クラスのタイプは、[!UICONTROL Record] または [!UICONTROL Time-series] です。 「**[!UICONTROL Create]**」を選択して選択内容を確定し、「[!UICONTROL Classes]」タブに戻ります。

![[!UICONTROL Create class] がハイライト表示された [!UICONTROL Create] ダイアログ &#x200B;](../../images/ui/resources/classes/create-class-from-schema.png)

[!UICONTROL Schema details] セクションでクラスリストが更新され、新しく作成したクラスが自動的に選択されます。 「**[!UICONTROL Next]**」を選択して、スキーマの作成を続行します。

![&#x200B; 新しいクラスが選択され、ハイライト表示され [!UICONTROL Schema details] いる [!UICONTROL Next] セクション &#x200B;](../../images/ui/resources/classes/select-new-class.png)

クラスを選択すると、「[!UICONTROL Name and review]」セクションが表示されます。 このセクションでは、スキーマを識別するための名前と説明を指定します。&#x200B;キャンバスにスキーマの基本構造（クラスによって提供される）が表示され、選択したクラスとスキーマ構造を確認できます。

テキストフィールドに、人間にとってわかりやすい [!UICONTROL Schema display name] を入力します。 次に、スキーマの識別に役立つ適切な説明を入力します。 スキーマ構造をレビューし、設定に満足したら、「**[!UICONTROL Finish]**」を選択してスキーマを作成します。

![[!UICONTROL Name and review]、[!UICONTROL Create schema]、[!UICONTROL Schema display name] がハイライト表示された [!UICONTROL Description] ワークフローの [!UICONTROL Finish] のセクション。](../../images/ui/resources/classes/schema-details.png)

## クラスへのフィールドの追加 {#add-fields}

スキーマエディターでカスタムクラスを採用するスキーマを開いたら、クラスへのフィールドの追加を開始できます。 新しいフィールドを追加するには、スキーマ名の横にある **プラス （+）** アイコンを選択します。

>[!IMPORTANT]
>
>組織で定義されたクラスを実装するスキーマを構築する場合、スキーマフィールドグループは、互換性のあるクラスでのみ使用できます。 定義したクラスは新しいので、**[!UICONTROL Add field group]** ダイアログには互換性のあるフィールドグループは表示されません。 代わりに、そのクラスで使用する [&#x200B; 新しいフィールドグループを作成する &#x200B;](./field-groups.md#create) 必要があります。 次回、新しいクラスを実装するスキーマを作成するとき、定義したフィールドグループが表示され、使用できるようになります。

![&#x200B; 「追加」ボタンがハイライト表示されたスキーマエディター。](../../images/ui/resources/classes/add-field.png)

>[!IMPORTANT]
>
>クラスに追加したフィールドは、そのクラスを使用するすべてのスキーマで使用されることに注意してください。 したがって、どのフィールドがすべてのスキーマのユースケースで役立つかを慎重に検討する必要があります。 このクラスの一部のスキーマでのみ使用される可能性のあるフィールドを追加しようと考えている場合は、代わりに [&#x200B; フィールドグループを作成 &#x200B;](./field-groups.md#create) して、それらのスキーマにそのフィールドを追加することを検討してください。

キャンバスに **[!UICONTROL Untitled Field]** プレースホルダーが表示されます。また、右側のパネルが更新されて、フィールドのプロパティを設定するためのコントロールが表示されます。 「**[!UICONTROL Assign to]**」で、「**[!UICONTROL Class]**」を選択します。

![&#x200B; スキーマエディターのキャンバスにある名称未設定フィールド。[!UICONTROL Class] のフィールドに割り当てプロパティが選択され、ハイライト表示されています。](../../images/ui/resources/classes/assign-to-class.png)

フィールドを設定してクラスに追加する方法に関する具体的な手順については、[UI でのフィールドの定義 &#x200B;](../fields/overview.md#define) に関するガイドを参照してください。 クラスに必要な数のフィールドを追加し続けます。 終了したら、「**[!UICONTROL Save]**」を選択して、スキーマとクラスの両方を保存します。

![[!UICONTROL Save] がハイライト表示された、スキーマエディターのキャンバス上の新しく作成されたスキーマ &#x200B;](../../images/ui/resources/classes/save.png)

このクラスを採用するスキーマを以前に作成した場合、新しく追加されたフィールドはこれらのスキーマに自動的に表示されます。

## クラスの編集 {#edit-a-class}

>[!NOTE]
>
>完全に編集およびカスタマイズできるのは、組織で定義されたカスタムクラスのみです。 Adobeで定義されたコアクラスの場合、フィールドの表示名のみを個々のスキーマのコンテキスト内で編集できます。 詳しくは、[&#x200B; スキーマフィールドの表示名の編集 &#x200B;](./schemas.md#display-names) の節を参照してください。
>
>カスタムクラスが保存され、データ取り込みで使用されると、それ以降は追加の変更のみを行うことができます。 詳しくは、[&#x200B; スキーマ進化のルール &#x200B;](../../schema/composition.md#evolution) を参照してください。

クラスを編集するには、スキーマワークフローを使用して、クラスを拡張する既存のスキーマを編集するか、スキーマを手動で作成します。 クラスを直接編集することはできません。 [!UICONTROL Browse] ワークスペースの「[!UICONTROL Schemas]」タブ内から、既存のクラスまたは **[!UICONTROL Create a schema]** を選択します。

![&#x200B; 既存のクラスと [!UICONTROL Create a schema] がハイライト表示されたスキーマエディター &#x200B;](../../images/ui/resources/classes/edit-class-options.png)

新しいスキーマを作成する場合は、[&#x200B; スキーマの作成 &#x200B;](#create-schema) の節を参照してください。 スキーマの作成が完了したら（または既存のスキーマを選択した後）、スキーマエディターが表示されます。 既存のクラスフィールドを更新するには、スキーマ構造からフィールドを選択します。 フィールドの情報が右側のパネルに表示されます。 [!UICONTROL Assign to]を確認する
オプション **[!UICONTROL Class]** が選択されているか、更新内容がクラスに影響しません。

![スキーマエディターでは、フィールドが選択および強調表示され、右側パネル [!UICONTROL Assign to]強調表示されます。](../../images/ui/resources/classes/edit-existing-field.png)

フィールドに必要な変更を加え、右パネルを下にスクロールして **[!UICONTROL Apply]** を選択し、変更を保存します。

>[!IMPORTANT]
>
> フィールドに加えた更新は、 [スキーマ進化のルール](../../schema/composition.md#evolution)に従って、そのクラスを使用するすべてのスキーマに適用されます。

![フィールドが選択された状態でスキーマエディターが表示され、右側パネル [!UICONTROL Apply]強調表示されます。](../../images/ui/resources/classes/save-changes.png)

新しいフィールドを追加するには、「 [クラスにフィールドを追加する](#add-fields-to-a-class) ガイドフォローするします。 完了したら、[ **[!UICONTROL Save]** ] を選択してスキーマとクラスの両方を保存します。

![[!UICONTROL Save]が強調表示されたスキーマエディター。](../../images/ui/resources/classes/save-schema.png)

## スキーマクラスの変更 {#schema}

スキーマのクラスは、初期作成プロセスのどの時点でも、保存される前に変更できます。 ただし、フィールドグループは特定のクラスとのみ互換性があるため、この操作には注意が必要です。 クラスを変更すると、追加したキャンバスとフィールドがリセットされます。詳しくは [スキーマの作成と編集](./schemas.md#change-class) のガイドを参照してください。

## 次の手順 {#next-steps}

このドキュメントでは、Experience Platform UI を使用してクラスを作成および編集する方法について説明しました。 [!UICONTROL Schemas] workspace の機能について詳しくは、[[!UICONTROL Schemas] workspace の概要を参照してください &#x200B;](../overview.md)

スキーマレジストリ API を使用してクラスを管理する方法については、[&#x200B; クラスエンドポイントガイド &#x200B;](../../api/classes.md) を参照してください。
