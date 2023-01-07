---
keywords: Experience Platform；ホーム；人気のトピック；API;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；クラス；
solution: Experience Platform
title: UI でのクラスの作成と編集
description: ユーザーインターフェイスでクラスを作成および編集するExperience Platformを説明します。
topic-legacy: user guide
exl-id: 1b4c3996-2319-45dd-9edd-a5bcad46578b
source-git-commit: a854a40034666159fabca550227efe9f3a47fb53
workflow-type: tm+mt
source-wordcount: '901'
ht-degree: 5%

---

# UI でのクラスの作成と編集

Adobe Experience Platformでは、スキーマのクラスは、スキーマに含まれるデータ（レコードまたは時系列）の動作面を定義します。 これに加えて、クラスは、そのクラスに基づくすべてのスキーマに含める必要のある共通のプロパティの最小数を記述し、複数の互換性のあるデータセットを結合する方法を提供します。

Adobeは、次のような、いくつかの標準（「コア」）Experience Data Model(XDM) クラスを提供します。 [!DNL XDM Individual Profile] および [!DNL XDM ExperienceEvent]. これらのコアクラスに加えて、独自のカスタムクラスを作成して、組織の具体的な使用例を説明することもできます。

このドキュメントでは、Experience PlatformUI でカスタムクラスを作成、編集、管理する方法の概要を説明します。

## 前提条件

このガイドでは、XDM システムに関する十分な知識が必要です。 詳しくは、 [XDM の概要](../../home.md) Experience Platformエコシステム内での XDM の役割、および [スキーマ構成の基本](../../schema/composition.md) クラスが XDM スキーマにどのように貢献するかを学ぶため。

このガイドは必須ではありませんが、 [UI でのスキーマの構成](../../tutorials/create-schema-ui.md) の様々な機能を身に付ける [!DNL Schema Editor].

## 新しいクラスの作成 {#create}

内 **[!UICONTROL スキーマ]** ワークスペース、選択 **[!UICONTROL スキーマを作成]**&#x200B;を選択し、「 **[!UICONTROL 参照]** をドロップダウンから選択します。

![](../../images/ui/resources/classes/browse-classes.png)

使用可能なクラスのリストから選択できるダイアログが表示されます。 ダイアログの上部で、「 」を選択します。 **[!UICONTROL 新しいクラスを作成]**. 次に、新しいクラスに、表示名（クラスを説明する短く、一意で、わかりやすいクラス名）、説明、およびスキーマが定義するデータの動作 (**[!UICONTROL レコード]** または **[!UICONTROL 時系列]**) をクリックします。

終了したら、「 」を選択します。 **[!UICONTROL クラスを割り当て]**.

![](../../images/ui/resources/classes/class-details.png)

この [!DNL Schema Editor] が表示され、作成したカスタムクラスに基づく新しいスキーマがキャンバスに表示されます。 クラスにはまだフィールドが追加されていないので、スキーマには `_id` フィールド： [!DNL Schema Registry].

![](../../images/ui/resources/classes/schema.png)

>[!IMPORTANT]
>
>組織で定義されたクラスを実装するスキーマを構築する場合、スキーマフィールドグループは互換性のあるクラスでのみ使用できることに注意してください。 定義したクラスは新しいので、互換性のあるフィールドグループは「 **[!UICONTROL フィールドグループを追加]** ダイアログ。 代わりに、 [新しいフィールドグループを作成](./field-groups.md#create) そのクラスで使用する 次に新しいクラスを実装するスキーマを作成すると、定義したフィールドグループが一覧表示され、使用できます。

これで、 [クラスへのフィールドの追加](#add-fields)：クラスを使用するすべてのスキーマで共有されます。

## 既存のクラスの編集 {#edit}

>[!NOTE]
>
>完全に編集およびカスタマイズできるのは、組織で定義されたカスタムクラスのみです。 Adobeで定義されたコアクラスの場合、各スキーマのコンテキスト内では、フィールドの表示名のみを編集できます。 詳しくは、 [スキーマフィールドの表示名の編集](./schemas.md#display-names) 」を参照してください。
>
>カスタムクラスを保存し、データ取り込みで使用した後は、その後は追加的な変更のみおこなうことができます。 詳しくは、 [スキーマ進化のルール](../../schema/composition.md#evolution) を参照してください。

既存のクラスを編集するには、 **[!UICONTROL 参照]** タブをクリックし、編集するクラスを使用するスキーマの名前を選択します。

![](../../images/ui/resources/classes/select-for-edit.png)

>[!TIP]
>
>ワークスペースの検索およびフィルタリング機能を使用すると、スキーマを見つけやすくなります。 詳しくは、 [XDM リソースの調査](../explore.md) を参照してください。

この [!DNL Schema Editor] が表示され、スキーマの構造がキャンバスに表示されます。 これで、 [クラスへのフィールドの追加](#add-fields).

![](../../images/ui/resources/classes/edit.png)

## クラスにフィールドを追加する {#add-fields}

カスタムクラスを使用するスキーマをで開いたら、 [!UICONTROL スキーマエディター]を使用すると、クラスへのフィールドの追加を開始できます。 新しいフィールドを追加するには、 **プラス (+)** スキーマ名の横にあるアイコン。

![](../../images/ui/resources/classes/add-field.png)

>[!IMPORTANT]
>
>クラスに追加するフィールドは、そのクラスを使用するすべてのスキーマで使用されます。 したがって、すべてのスキーマの使用例で役立つフィールドを慎重に検討する必要があります。 このクラスの一部のスキーマでのみ使用される可能性のあるフィールドを追加する場合は、次の方法でそれらのスキーマに追加することを検討してください。 [フィールドグループの作成](./field-groups.md#create) 代わりに、

A **[!UICONTROL 新しいフィールド]** がキャンバスに表示され、右側のレールが更新されて、フィールドのプロパティを設定するコントロールが表示されます。 の下 **[!UICONTROL 割り当て先]**&#x200B;を選択します。 **[!UICONTROL クラス]**.

![](../../images/ui/resources/classes/assign-to-class.png)

詳しくは、 [UI でのフィールドの定義](../fields/overview.md#define) を参照してください。 引き続き、必要な数のフィールドをクラスに追加します。 終了したら、「 」を選択します。 **[!UICONTROL 保存]** スキーマとクラスの両方を保存します。

![](../../images/ui/resources/classes/save.png)

このクラスを使用するスキーマを以前に作成したことがある場合、新しく追加されたフィールドは、これらのスキーマに自動的に表示されます。

## スキーマクラスの変更 {#schema}

スキーマのクラスは、最初の作成プロセス中、保存前の任意の時点で変更できます。 詳しくは、 [スキーマの作成と編集](./schemas.md#change-class) を参照してください。

## 次の手順

このドキュメントでは、Platform UI を使用してクラスを作成および編集する方法について説明しました。 の機能の詳細については、 [!UICONTROL スキーマ] ワークスペース ( [[!UICONTROL スキーマ] workspace の概要](../overview.md).

を使用してクラスを管理する方法を学ぶには [!DNL Schema Registry] API( [クラスエンドポイントガイド](../../api/classes.md).
