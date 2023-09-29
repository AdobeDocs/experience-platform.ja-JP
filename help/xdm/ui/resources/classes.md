---
keywords: Experience Platform；ホーム；人気のトピック；API;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；クラス；
solution: Experience Platform
title: UI でのクラスの作成と編集
description: ユーザーインターフェイスでクラスを作成および編集するExperience Platformを説明します。
exl-id: 1b4c3996-2319-45dd-9edd-a5bcad46578b
source-git-commit: 4214339c4a661c6bca2cd571919ae205dcb47da1
workflow-type: tm+mt
source-wordcount: '1374'
ht-degree: 8%

---

# UI でのクラスの作成と編集 {#ui-create-and-edit}

>[!CONTEXTUALHELP]
>id="platform_schemas_class_filter"
>title="標準またはカスタムのクラスフィルター"
>abstract="使用可能なクラスのリストは、作成方法に基づいて事前にフィルタリングされています。ラジオボタンを選択して、「標準」オプションと「カスタム」オプションのいずれかを選択します。「標準」オプションは、アドビが作成したエンティティを表示し、XDM 個人プロファイルクラスと XDM エクスペリエンスイベントクラスの両方を含みます。「カスタム」オプションは、組織内で作成したエンティティを表示します。クラスの作成と編集について詳しくは、ドキュメントを参照してください。"

Adobe Experience Platformでは、スキーマのクラスは、スキーマに含まれるデータ（レコードまたは時系列）の動作面を定義します。 これに加えて、クラスは、そのクラスに基づくすべてのスキーマに含める必要のある共通のプロパティの最小数を記述し、複数の互換性のあるデータセットを結合する方法を提供します。

Adobeは、次のような、いくつかの標準（「コア」）Experience Data Model(XDM) クラスを提供します。 [!DNL XDM Individual Profile] および [!DNL XDM ExperienceEvent]. これらのコアクラスに加えて、独自のカスタムクラスを作成して、組織の具体的な使用例を説明することもできます。

このドキュメントでは、Experience PlatformUI でカスタムクラスを作成、編集、管理する方法の概要を説明します。

## 前提条件

このガイドでは、XDM システムに関する十分な知識が必要です。 詳しくは、 [XDM の概要](../../home.md) Experience Platformエコシステム内での XDM の役割、および [スキーマ構成の基本](../../schema/composition.md) クラスが XDM スキーマにどのように貢献するかを学ぶため。

このガイドは必須ではありませんが、 [UI でのスキーマの構成](../../tutorials/create-schema-ui.md) の様々な機能を身に付ける [!DNL Schema Editor].

## はじめに

Platform UI で、「 」を選択します。 **[!UICONTROL スキーマ]** 左側のナビゲーションで、 [!UICONTROL スキーマ] ワークスペースに移動して、 **[!UICONTROL クラス]** タブをクリックします。 使用可能なクラスのリストが表示されます。

## クラスをフィルター {#filter}

クラスのリストは、作成方法に基づいて自動的にフィルタリングされます。 デフォルト設定では、Adobeで定義されたクラスが表示されます。 また、リストをフィルタリングして、組織で作成したリストを表示することもできます。 ラジオボタンを選択して、 [!UICONTROL 標準] および [!UICONTROL カスタム] オプション。 The [!UICONTROL 標準] 「 」オプションは、Adobeで作成されたエンティティと [!UICONTROL カスタム] 「 」オプションは、組織内で作成されたエンティティを表示します。

![The [!UICONTROL クラス] タブ [!UICONTROL スキーマ] ワークスペース [!UICONTROL 標準] および [!UICONTROL カスタム] ハイライト表示されました。](../../images/ui/resources/classes/standard-and-custom-classes.png)

>[!TIP]
>
>ワークスペースの検索機能を使用して、スキーマを見つけやすくすることができます。 次のガイドを参照してください： [XDM リソースの調査](../explore.md) を参照してください。

## 新しいクラスの作成 {#create}

Platform UI でクラスを作成するには、2 つのメソッドがあります。 Adobe Analytics の **[!UICONTROL スキーマ]** ワークスペース、選択 **[!UICONTROL スキーマを作成]**、または [!UICONTROL クラス] タブの選択 **[!UICONTROL クラスを作成]**.

![The [!UICONTROL クラス] タブ [!UICONTROL スキーマ] ワークスペース [!UICONTROL スキーマを作成] および [!UICONTROL クラスを作成] ハイライト](../../images/ui/resources/classes/create-class-methods.png)

次を選択した場合、 **[!UICONTROL クラスを作成]**、 [!UICONTROL クラスを作成] ダイアログが表示されます。 を入力します。 [!UICONTROL 名前] および [!UICONTROL 説明] クラスの場合は、ラジオボタンを使用してクラスの意図した動作を選択します。 クラスは、レコード系列または時系列です。 選択 **[!UICONTROL 作成]** をクリックして選択を確定します。

![The [!UICONTROL クラスを作成] ～との対話 [!UICONTROL 作成] ハイライト表示されました。](../../images/ui/resources/classes/create-class-dialog.png)

The [!DNL Schema Editor] が表示され、作成したカスタムクラスに基づく新しいスキーマがキャンバスに表示されます。 クラスにはまだフィールドが追加されていないので、スキーマには `_id` フィールド： [!DNL Schema Registry].

![](../../images/ui/resources/classes/schema.png)

>[!IMPORTANT]
>
>組織で定義されたクラスを実装するスキーマを構築する場合、スキーマフィールドグループは互換性のあるクラスでのみ使用できることに注意してください。 定義したクラスは新しいので、互換性のあるフィールドグループは「 **[!UICONTROL フィールドグループを追加]** ダイアログ。 代わりに、 [新しいフィールドグループを作成する](./field-groups.md#create) そのクラスで使用する 次に新しいクラスを実装するスキーマを作成すると、定義したフィールドグループが一覧表示され、使用できます。

### クラスの作成または編集 {#create-or-edit}

次を選択した場合、 **[!UICONTROL スキーマを作成]**、 [!UICONTROL スキーマを作成] ワークフローが表示されます。 Adobe Analytics の [!UICONTROL スキーマの詳細] セクション、選択 **[!UICONTROL その他]**. 使用可能なクラスのリストが表示されます。 ここから、新しいクラスを基にする既存のクラスを参照およびフィルタリングできます。

>[!NOTE]
>
>完全に編集およびカスタマイズできるのは、組織で定義されたカスタムクラスのみです。 Adobeで定義されたコアクラスの場合、各スキーマのコンテキスト内では、フィールドの表示名のみを編集できます。 詳しくは、 [スキーマフィールドの表示名の編集](./schemas.md#display-names) 」を参照してください。
>
>カスタムクラスを保存し、データ取り込みで使用した後は、その後は追加的な変更のみおこなうことができます。 詳しくは、 [スキーマ進化のルール](../../schema/composition.md#evolution) を参照してください。

![The [!UICONTROL スキーマを作成] を使用したワークフロー [!UICONTROL その他] で強調表示された [!UICONTROL スキーマの詳細] 」セクションに入力します。](../../images/ui/resources/classes/other-schema-details.png)

ラジオボタンを選択して、カスタムクラスか標準クラスかに基づいてクラスをフィルタリングします。 また、業種に基づいて使用可能な結果をフィルタリングしたり、検索フィールドを使用して特定のクラスを検索したりできます。

![The [!UICONTROL スキーマを作成] 検索バーを含むワークフロー [!UICONTROL カスタム]、および [!UICONTROL 業界] ハイライト表示されました。](../../images/ui/resources/classes/filter-and-search.png)

適切なクラスを決定するのに役立つ情報 (![情報アイコン。](../../images/ui/resources/classes/info.png)) とプレビュー (![プレビューアイコン。](../../images/ui/resources/classes/preview.png)) 個のアイコンをクラスごとに追加します。 情報アイコンをクリックすると、関連付けられているクラスと業界の説明を示すダイアログが開きます。 プレビューアイコンを使用すると、スキーマダイアグラムとそのプロパティを含むクラスのプレビューダイアログが開きます。

![スキーマ図とクラスプロパティがハイライト表示された、選択したクラスのプレビュー。](../../images/ui/resources/classes/class-preview.png)

任意の行を選択してクラスを選択し、「 」を選択します。 **[!UICONTROL 次へ]** をクリックして選択を確定します。

![The [!UICONTROL スキーマを作成] 使用可能なクラスのテーブルから選択したクラスを持つワークフローと [!UICONTROL 次へ] ハイライト表示されました。](../../images/ui/resources/classes/select-class.png)

The [!UICONTROL 名前と説明] セクションが表示されます。 この節では、スキーマを識別する名前と説明を指定します。&#x200B;キャンバスには、選択したクラスとスキーマ構造を確認できるように、スキーマの基本構造（クラスで提供）が表示されます。

クラスの短く、わかりやすい、一意の、わかりやすい名前を [!UICONTROL スキーマの表示名] テキストフィールド。 次に、適切な説明を入力して、スキーマが定義するデータの動作を識別します。 スキーマの構造を確認し、設定に満足したら、「 」を選択します。 **[!UICONTROL 完了]** をクリックしてスキーマを作成します。

![The [!UICONTROL 名前とレビュー] のセクション [!UICONTROL スキーマを作成] ワークフローと [!UICONTROL スキーマの表示名], [!UICONTROL 説明]、および [!UICONTROL 完了] ハイライト表示されました。](../../images/ui/resources/classes/name-and-review-class.png)

The [!DNL Schema Editor] が表示され、スキーマの構造がキャンバスに表示されます。 これで、以下を開始できます。 [クラスへのフィールドの追加](#add-fields).

![](../../images/ui/resources/classes/edit.png)

## クラスにフィールドを追加する {#add-fields}

カスタムクラスを使用するスキーマをで開いたら、 [!UICONTROL スキーマエディター]を使用すると、クラスへのフィールドの追加を開始できます。 新しいフィールドを追加するには、 **プラス (+)** スキーマ名の横にあるアイコン。

![](../../images/ui/resources/classes/add-field.png)

>[!IMPORTANT]
>
>クラスに追加するフィールドは、そのクラスを使用するすべてのスキーマで使用されます。 したがって、すべてのスキーマの使用例で役立つフィールドを慎重に検討する必要があります。 このクラスの一部のスキーマでのみ使用される可能性のあるフィールドを追加する場合は、次の方法でそれらのスキーマに追加することを検討してください。 [フィールドグループの作成](./field-groups.md#create) 代わりに、

An **[!UICONTROL 名称未設定フィールド]** プレースホルダーがキャンバスに表示され、右側のレールが更新されて、フィールドのプロパティを設定するコントロールが表示されます。 の下 **[!UICONTROL 割り当て先]**&#x200B;を選択します。 **[!UICONTROL クラス]**.

![](../../images/ui/resources/classes/assign-to-class.png)

![](../../images/ui/resources/classes/assign-to-class.png)

次のガイドを参照してください： [UI でのフィールドの定義](../fields/overview.md#define) を参照してください。 引き続き、必要な数のフィールドをクラスに追加します。 終了したら、「 」を選択します。 **[!UICONTROL 保存]** をクリックして、スキーマとクラスの両方を保存します。

![](../../images/ui/resources/classes/save.png)

このクラスを使用するスキーマを以前に作成したことがある場合、新しく追加されたフィールドは、これらのスキーマに自動的に表示されます。

## スキーマクラスの変更 {#schema}

スキーマのクラスは、最初の作成プロセス中、保存前の任意の時点で変更できます。 次のガイドを参照してください： [スキーマの作成と編集](./schemas.md#change-class) を参照してください。

## 次の手順

このドキュメントでは、Platform UI を使用してクラスを作成および編集する方法について説明しました。 の機能の詳細については、 [!UICONTROL スキーマ] ワークスペースについては、 [[!UICONTROL スキーマ] workspace の概要](../overview.md).

を使用してクラスを管理する方法を学ぶには [!DNL Schema Registry] API( [クラスエンドポイントガイド](../../api/classes.md).
