---
keywords: Experience Platform;profile;real-time customer profile;user interface;UI;customization;profile details;details
title: プロファイルの詳細のカスタマイズ
description: 'このガイドでは、Adobe Experience PlatformUI内でリアルタイム顧客プロファイルデータが表示される方法をカスタマイズする手順を順を追って説明します。 '
topic: guide
translation-type: tm+mt
source-git-commit: 9068d12e60da63a6a2a2ff18c016080ea581104f
workflow-type: tm+mt
source-wordcount: '1167'
ht-degree: 0%

---


# [!DNL Real-time Customer Profile] 詳細カスタマイズ {#profile-detail-customization}

Adobe Experience Platformのユーザーインターフェイス内では、顧客プロファイルの形で [!DNL Real-time Customer Profile] データを表示し、操作できます。 UIに表示されるプロファイル情報は、複数のプロファイルフラグメントから結合され、個々の顧客の1つの表示を形成しています。 基本属性、リンクされたID、チャネル環境設定などの詳細が含まれます。 プロファイルに表示されるデフォルトのフィールドは、組織レベルで変更して、優先 [!DNL Profile] 属性を表示することもできます。 このガイドでは、プラットフォームUI内での [!DNL Profile] データの表示方法をカスタマイズする手順を順を追って説明します。

プロファイルUIの完全なガイドについては、 [プロファイルユーザガイドを参照してください](user-guide.md)。

## カードの順序の変更とサイズ変更 {#reorder-and-resize-cards}

顧客プロファイルの「 **[!UICONTROL 詳細]** 」タブで、「ダッシュボードを **** 変更」を選択して、既存のカードのサイズ変更と並べ替えを行うことができます。

![](../images/profile-customization/profiles-modify-dashboard.png)

ダッシュボードの変更を選択した後、カードのタイトルを選択し、カードを目的の順序にドラッグ&amp;ドロップして、カードの順序を変更できます。 カードの右下隅にある角度記号(`⌟`)を選択し、カードを目的のサイズにドラッグして、カードのサイズを変更することもできます。 この例では、 **[!UICONTROL 基本属性]** カードのサイズが変更されています。

![](../images/profile-customization/profiles-resize-cards.png)

選択したカードは希望のサイズに調整され、周囲のカードは動的に再配置されます。 これにより、一部のカードが追加の行に移動する場合があり、すべてのカードを表示するには下にスクロールする必要があります。 例えば、「[!UICONTROL 基本属性]」カードのサイズを変更すると、「[!UICONTROL リンクされたID]」カードは一番上の行に表示されなくなり、プロファイル内の新しい2行目に表示されます（図には示されていません）。 「[!UICONTROL Linked identitys]」カードを最上行に戻すには、「[!UICONTROL チャネル環境設定]」カードの現在の位置にドラッグ&amp;ドロップします。

![](../images/profile-customization/profiles-card-resized.png)

## カードの編集と削除

カードのサイズ変更と順序の変更に加えて、特定のカードのコンテンツを編集したり、ダッシュボードからカードを完全に削除したりできます。 カードの編集や削除を行うには、カードの右上隅にある「三点リーダー」(`...`)を選択します。 これにより、選択したカードのプロパティに応じてカードを編集または削除するオプションを含むドロップダウンが開きます。

>[!NOTE]
>
>すべてのカードを編集または削除できるわけではありません。 これは、一部のカードに読み取り専用または必須の情報が含まれているためです。 カードの右上隅に楕円が表示されない場合、カードには読み取り専用のAND必須情報が含まれ、編集も削除もできません。 カードの隅に「三点リーダー」があり、これを選択すると、カードを削除するオプションのみが表示され、カードの情報は読み取り専用になり、編集できません。

![](../images/profile-customization/profiles-edit-remove-resized.png)

ドロップダウンで **[!UICONTROL 「編集]** 」を選択して、 **[!UICONTROL 編集ウィジェット]** ワークスペースを開きます。このワークスペースでは、カードタイトルの更新、表示属性の並べ替えや削除、 **[!UICONTROL 属性]** ボタンを使用した追加の属性の追加を行うことができます。

![](../images/profile-customization/profiles-edit-widget-basic-attributes.png)

## 追加属性 {#add-attributes}

**[!UICONTROL 編集ウィジェット]** 画面で、カードの右上隅にある **** 属性追加を選択し、カードへの属性の追加を開始します。

![](../images/profile-customization/profiles-edit-widget-basic-add-attributes.png)

「 **[!UICONTROL 和集合スキーマフィールドを選択」ダイアログが開くと]** 、ダイアログの左側に「 [!UICONTROL XDM個別プロファイル] 和集合」スキーマが完全に表示され、その下にフィールドがネストされます。 和集合スキーマの詳細については、『ユーザガイド』の「 [和集合のスキーマ」の節を参照し [!DNL Profile] てください](user-guide.md#union-schema)。

ダイアログの右側の **[!UICONTROL 選択した属性]** (Selected Attributes)セクションには、編集中のカードに現在含まれている属性が表示されます。 属性の削除や並べ替えはここでも行うことができます。 選択した属性の総数と、1つのカードに追加できる属性の最大数(20)が表示されます。

![](../images/profile-customization/profiles-select-field-before.png)

使用可能な和集合スキーマフィールドのいずれかを選択して、編集するカードの属性をカスタマイズできます。 選択したフィールドの横にチェックマークが付き、選択した属性のリストに自動的に追加されます。 カードに表示したい属性をすべて追加したら、「 **[!UICONTROL 選択]** 」を選択して **[!UICONTROL 編集ウィジェット]** 画面に戻ります。

![](../images/profile-customization/profiles-select-field-after.png)

Widgetを **** 編集画面に戻ると、選択内容を反映するように、カードの属性のリストが更新されます。 必要に応じて、カードの属性を削除または並べ替えたり、カードのタイトルを編集したりできます。 編集が完了したら、「 **[!UICONTROL 保存]** 」を選択して変更を保存します。

![](../images/profile-customization/profiles-edit-widget-new-attributes.png)

保存すると、「 **[!UICONTROL 詳細]** 」タブに戻り、更新されたカードと属性が表示されます。

![](../images/profile-customization/profiles-resized-card-new-attributes.png)

## Add a new card {#add-a-new-card}

Experience Platform内でのプロファイルの外観をさらにカスタマイズするには、ダッシュボードに新しいカードを追加して、それらのカードに表示する属性を選択します。 まず、「 **[!UICONTROL 詳細]** 」(Detail **[!UICONTROL )タブの「ダッシュボードを修正]** 」(Modify Modify Modify)を選択します。

![](../images/profile-customization/profiles-modify-dashboard.png)

次に、ダッシュボードの左上隅にある **[!UICONTROL Widget]** 追加を選択します。

![](../images/profile-customization/profiles-add-widget.png)

新しいカードを追加するよう選択すると、 **[!UICONTROL 編集ウィジェット]** 画面が開き、新しいカードのタイトルを指定したり、カードに表示する属性を選択したりできます。 カードへの属性の追加を開始するには、 **[!UICONTROL 追加属性を選択します]**。

![](../images/profile-customization/profiles-edit-new-widget.png)

「 **[!UICONTROL 和集合スキーマフィールドを]** 選択」ダイアログが開くと、ダイアログの左側に「 [!UICONTROL XDM個別プロファイル] 和集合」スキーマ全体が表示され、ダイアログの右側の「 **[!UICONTROL 選択した属性]** 」セクションにカード用に選択した属性が表示されます。 属性の追加の詳細については、このドキュメントの前の [セクションで、属性の追加に関する節を参照してください](#add-attributes) 。

選択した属性の総数と、1つのカードに追加できる属性の最大数(20)が表示されます。 また、この画面から選択した属性を削除し、並べ替えを行うこともできます。 カードに表示したい属性をすべて追加したら、「 **[!UICONTROL 選択]** 」を選択して **[!UICONTROL 編集ウィジェット]** 画面に戻ります。

![](../images/profile-customization/profiles-add-fields-new-widget.png)

Widgetを **** 編集画面に戻ると、カードの属性のリストには前の画面での選択が反映されます。 また、必要に応じてカード属性の順序を変更したり、削除したりすることもできます。

新しいカードを保存するには、まず **[!UICONTROL カードタイトルを指定する必要があり]**、「 **[!UICONTROL 保存]** 」を選択してカード作成プロセスを完了できます。

![](../images/profile-customization/profiles-edit-new-widget-with-fields.png)

保存すると、「 **[!UICONTROL 詳細]** 」タブに戻り、新しいカードと属性が表示されます。

![](../images/profile-customization/profiles-detail-new-widget.png)

## カードをデフォルトに戻す

それ以降に取り外したカードをデフォルトに戻す場合は、すばやく簡単に復元できます。 最初に、「 **[!UICONTROL ダッシュボードを変更]**」を選択し、次に「デフォルトのカードを **[!UICONTROL 復元]**」を選択します。 デフォルトのカードが表示されたら、「 **[!UICONTROL 保存]** 」を選択して変更を保存できます。デフォルトのカードを復元しない場合は、「 **[!UICONTROL キャンセル]** 」を選択します。

![](../images/profile-customization/profiles-restore-default.png)

## 次の手順

このドキュメントに従うと、カードの追加と削除、カードの詳細と属性の編集、カードの順序変更とサイズ変更など、組織のプロファイル表示を更新できるようになります。 Experience PlatformUIでの [!DNL Profile] データの操作について詳しくは、 [[!DNL Profile] ユーザガイドを参照してください](user-guide.md)。