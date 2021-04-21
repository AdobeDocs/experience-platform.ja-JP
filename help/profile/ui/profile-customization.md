---
keywords: Experience Platform;プロファイル；リアルタイム顧客プロファイル；ユーザインターフェイス；UI；カスタマイズ；プロファイルの詳細；詳細
title: UIのプロファイル詳細のカスタマイズ
description: 'このガイドでは、Adobe Experience PlatformUI内でリアルタイム顧客プロファイルデータが表示される方法をカスタマイズする手順を順を追って説明します。 '
topic-legacy: guide
exl-id: 76cf8420-cc50-4a56-9f6d-5bfc01efcdb3
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1183'
ht-degree: 0%

---

# [!DNL Real-time Customer Profile] 詳細カスタマイズ  {#profile-detail-customization}

Adobe Experience Platformのユーザーインターフェイス内では、顧客プロファイルの形で[!DNL Real-time Customer Profile]データを表示し、操作できます。 UIに表示されるプロファイル情報は、複数のプロファイルフラグメントから結合され、個々の顧客の1つの表示を形成しています。 基本属性、リンクされたID、チャネル環境設定などの詳細が含まれます。 プロファイルに表示されるデフォルトのフィールドは、組織レベルで変更して、好みの[!DNL Profile]属性を表示することもできます。 このガイドでは、プラットフォームUI内で[!DNL Profile]データが表示される方法をカスタマイズするための手順を順を追って説明します。

プロファイルUIの完全なガイドについては、[プロファイルUIガイド](user-guide.md)を参照してください。

## カードの順序の変更とサイズ変更{#reorder-and-resize-cards}

顧客プロファイルの「**[!UICONTROL 詳細]**」タブから、「**[!UICONTROL ダッシュボードを変更]**」を選択して、既存のカードのサイズ変更と並べ替えを行うことができます。

![](../images/profile-customization/profiles-modify-dashboard.png)

ダッシュボードの変更を選択した後、カードのタイトルを選択し、カードを目的の順序にドラッグ&amp;ドロップして、カードの順序を変更できます。 カードの右下隅にある角度記号(`⌟`)を選択し、カードを目的のサイズにドラッグして、カードのサイズを変更することもできます。 この例では、**[!UICONTROL 基本属性]**&#x200B;カードのサイズが変更されています。

![](../images/profile-customization/profiles-resize-cards.png)

選択したカードは希望のサイズに調整され、周囲のカードは動的に再配置されます。 これにより、一部のカードが追加の行に移動する場合があり、すべてのカードを表示するには下にスクロールする必要があります。 例えば、「[!UICONTROL 基本属性]」カードのサイズを変更すると、「[!UICONTROL Linked identitys]」カードは最上行に表示されなくなり、プロファイル内の新しい2行目に表示されます（図には示されていません）。 「[!UICONTROL Linked identitys]」カードを先頭行に戻すには、「[!UICONTROL チャネルの環境設定]」カードの現在の位置にドラッグ&amp;ドロップします。

![](../images/profile-customization/profiles-card-resized.png)

## カードの編集と削除

カードのサイズ変更と順序の変更に加えて、特定のカードのコンテンツを編集したり、ダッシュボードからカードを完全に削除したりできます。 カードを編集または削除するには、カードの右上隅にある三点リーダー(`...`)を選択します。 これにより、選択したカードのプロパティに応じてカードを編集または削除するオプションを含むドロップダウンが開きます。

>[!NOTE]
>
>すべてのカードを編集または削除できるわけではありません。 これは、一部のカードに読み取り専用または必須の情報が含まれているためです。 カードの右上隅に楕円が表示されない場合、カードには読み取り専用のAND必須情報が含まれ、編集も削除もできません。 カードの隅に「三点リーダー」があり、これを選択すると、カードを削除するオプションのみが表示され、カードの情報は読み取り専用になり、編集できません。

![](../images/profile-customization/profiles-edit-remove-resized.png)

ドロップダウンで「**[!UICONTROL 編集]**」を選択して&#x200B;**[!UICONTROL 編集ウィジェット]**&#x200B;ワークスペースを開き、カードタイトルの更新、表示属性の並べ替えや削除、**[!UICONTROL 追加属性]**&#x200B;ボタンを使用した追加の属性の追加を行うことができます。

![](../images/profile-customization/profiles-edit-widget-basic-attributes.png)

## 追加属性{#add-attributes}

**[!UICONTROL ウィジェット]**&#x200B;を編集画面で、カードの右上隅にある&#x200B;**[!UICONTROL 追加属性]**&#x200B;を選択して、カードへの属性の追加を開始します。

![](../images/profile-customization/profiles-edit-widget-basic-add-attributes.png)

**[!UICONTROL 和集合スキーマフィールドを選択]**&#x200B;ダイアログが開くと、ダイアログの左側に完全な[!UICONTROL XDM個別プロファイル]和集合スキーマが表示され、その下にフィールドがネストされます。 和集合スキーマの詳細については、 [!DNL Profile] ユーザーガイド](user-guide.md#union-schema)の[和集合スキーマの節を参照してください。

ダイアログの右側の&#x200B;**[!UICONTROL 選択した属性]**&#x200B;セクションには、編集中のカードに現在含まれている属性が表示されます。 属性の削除や並べ替えはここでも行うことができます。 選択した属性の総数と、1つのカードに追加できる属性の最大数(20)が表示されます。

![](../images/profile-customization/profiles-select-field-before.png)

使用可能な和集合スキーマフィールドのいずれかを選択して、編集するカードの属性をカスタマイズできます。 選択したフィールドの横にチェックマークが付き、選択した属性のリストに自動的に追加されます。 カードに表示したい属性をすべて追加したら、「****」を選択して&#x200B;**[!UICONTROL 編集ウィジェット]**&#x200B;画面に戻ります。

![](../images/profile-customization/profiles-select-field-after.png)

**[!UICONTROL 編集ウィジェット]**&#x200B;画面に戻ると、選択内容を反映するようにカードの属性のリストが更新されます。 必要に応じて、カードの属性を削除または並べ替えたり、カードのタイトルを編集したりできます。 編集が完了したら、「**[!UICONTROL 保存]**」を選択して変更を保存します。

![](../images/profile-customization/profiles-edit-widget-new-attributes.png)

保存すると、「**[!UICONTROL 詳細]**」タブに戻り、更新されたカードと属性が表示されます。

![](../images/profile-customization/profiles-resized-card-new-attributes.png)

## 追加新しいカード{#add-a-new-card}

Experience Platform内でのプロファイルの外観をさらにカスタマイズするには、ダッシュボードに新しいカードを追加して、それらのカードに表示する属性を選択します。 最初に、「**[!UICONTROL 詳細]**」タブの「ダッシュボード&#x200B;]**を変更」を選択します。**[!UICONTROL 

![](../images/profile-customization/profiles-modify-dashboard.png)

次に、ダッシュボードの左上隅にある追加&#x200B;**[!UICONTROL ウィジェット]**&#x200B;を選択します。

![](../images/profile-customization/profiles-add-widget.png)

新しいカードを追加するよう選択すると、**[!UICONTROL ウィジェット]**&#x200B;を編集画面が開き、新しいカードのタイトルを指定し、カードに表示する属性を選択できます。 カードへの属性の追加を開始するには、**[!UICONTROL 追加属性]**&#x200B;を選択します。

![](../images/profile-customization/profiles-edit-new-widget.png)

**[!UICONTROL 和集合スキーマフィールドを選択]**&#x200B;ダイアログが開くと、ダイアログの左側に完全な[!UICONTROL XDM個別プロファイル]和集合スキーマが表示され、ダイアログの右側の&#x200B;**[!UICONTROL 選択した属性]**&#x200B;セクションにカード用に選択した属性が表示されます。 属性の追加について詳しくは、このドキュメントの前述の[属性の追加に関する節を参照してください。](#add-attributes)

選択した属性の総数と、1つのカードに追加できる属性の最大数(20)が表示されます。 また、この画面から選択した属性を削除し、並べ替えを行うこともできます。 カードに表示したい属性をすべて追加したら、「****」を選択して&#x200B;**[!UICONTROL 編集ウィジェット]**&#x200B;画面に戻ります。

![](../images/profile-customization/profiles-add-fields-new-widget.png)

**[!UICONTROL 編集ウィジェット]**&#x200B;画面に戻ると、カードの属性のリストには前の画面での選択が反映されます。 また、必要に応じてカード属性の順序を変更したり、削除したりすることもできます。

新しいカードを保存するには、まず&#x200B;**[!UICONTROL カードタイトル]**&#x200B;を指定する必要があります。その後、**[!UICONTROL 保存]**&#x200B;を選択して、カード作成プロセスを完了できます。

![](../images/profile-customization/profiles-edit-new-widget-with-fields.png)

保存すると、「**[!UICONTROL 詳細]**」タブに戻り、新しいカードと属性が表示されます。

![](../images/profile-customization/profiles-detail-new-widget.png)

## カードをデフォルトに戻す

それ以降に取り外したカードをデフォルトに戻す場合は、すばやく簡単に復元できます。 まず、「**[!UICONTROL ダッシュボードを変更]**」を選択し、次に「**[!UICONTROL デフォルトのカードを復元]**」を選択します。 デフォルトのカードが表示されたら、「**[!UICONTROL 保存]**」を選択して変更を保存するか、デフォルトのカードを復元しない場合は「**[!UICONTROL キャンセル]**」を選択します。

![](../images/profile-customization/profiles-restore-default.png)

## 次の手順

このドキュメントに従うと、カードの追加と削除、カードの詳細と属性の編集、カードの順序変更とサイズ変更など、組織のプロファイル表示を更新できるようになります。 Experience PlatformUIでの[!DNL Profile]データの操作について詳しくは、[[!DNL Profile] ユーザーガイド](user-guide.md)を参照してください。
