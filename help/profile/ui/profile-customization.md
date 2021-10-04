---
keywords: Experience Platform、プロファイル、リアルタイム顧客プロファイル、ユーザーインターフェイス、UI、カスタマイズ、プロファイルの詳細、詳細
title: UI でのプロファイルの詳細のカスタマイズ
description: 'このガイドでは、Adobe Experience Platform UI 内でリアルタイム顧客プロファイルデータを表示する方法をカスタマイズする手順を説明します。 '
topic-legacy: guide
exl-id: 76cf8420-cc50-4a56-9f6d-5bfc01efcdb3
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1183'
ht-degree: 0%

---

# [!DNL Real-time Customer Profile] 詳細なカスタマイズ {#profile-detail-customization}

Adobe Experience Platformのユーザーインターフェイス内で、顧客プロファイルの形式で [!DNL Real-time Customer Profile] データを表示し、操作できます。 UI に表示されるプロファイル情報は、複数のプロファイルフラグメントから結合され、各顧客の単一の表示を形成しています。 これには、基本属性、リンクされた ID、チャネル環境設定などの詳細が含まれます。 プロファイルに表示されるデフォルトのフィールドは、組織レベルで変更して、好みの [!DNL Profile] 属性を表示することもできます。 このガイドでは、Platform UI 内での [!DNL Profile] データの表示方法をカスタマイズする手順を順を追って説明します。

プロファイル UI の完全なガイドについては、『[ プロファイル UI ガイド ](user-guide.md)』を参照してください。

## カードの並べ替えとサイズ変更 {#reorder-and-resize-cards}

顧客プロファイルの「**[!UICONTROL 詳細]**」タブで、「**[!UICONTROL ダッシュボードを変更]**」を選択して、既存のカードのサイズ変更と並べ替えをおこなうことができます。

![](../images/profile-customization/profiles-modify-dashboard.png)

ダッシュボードの変更を選択した後、カードのタイトルを選択し、カードを目的の順序にドラッグ&amp;ドロップして、カードを並べ替えることができます。 カードの右下隅の角度記号 (`⌟`) を選択し、カードを目的のサイズにドラッグして、カードのサイズを変更することもできます。 この例では、**[!UICONTROL 基本属性]** カードのサイズが変更されています。

![](../images/profile-customization/profiles-resize-cards.png)

選択されたカードは、目的のサイズに合わせて調整され、周囲のカードは動的に再配置されます。 これにより、一部のカードが追加の行に移動し、すべてのカードを表示するために下にスクロールする必要が生じる場合があります。 例えば、「[!UICONTROL  基本属性 ]」カードのサイズが変更されると、「[!UICONTROL  リンクされた ID]」カードは上の行に表示されなくなり、プロファイル内の新しい 2 行目に表示されます（図はない）。 「[!UICONTROL  リンクされた ID]」カードを上の行に戻すには、「[!UICONTROL  チャネル環境設定 ]」カードの現在の位置にドラッグ&amp;ドロップします。

![](../images/profile-customization/profiles-card-resized.png)

## カードの編集と削除

カードのサイズ変更と並べ替えに加え、特定のカードのコンテンツを編集し、ダッシュボードから一部のカードを完全に削除できます。 カードの右上隅にある省略記号 (`...`) を選択して、編集または削除します。 選択したカードのプロパティに応じて、カードを編集または削除するオプションを含むドロップダウンが開きます。

>[!NOTE]
>
>一部のカードは編集または削除できません。 これは、一部のカードに読み取り専用または必須の情報が含まれているためです。 カードの右上隅に省略記号が表示されない場合は、読み取り専用の AND 必須情報が含まれ、編集も削除もできません。 カードの隅に省略記号が表示され、カードを選択すると、そのカードを削除するオプションのみが表示される場合、カード情報は読み取り専用になり、編集できません。

![](../images/profile-customization/profiles-edit-remove-resized.png)

ドロップダウンで **[!UICONTROL 編集]** を選択して **[!UICONTROL 編集ウィジェット]** ワークスペースを開き、カードタイトルの更新、表示属性の並べ替えや削除、**[!UICONTROL 属性の追加]** ボタンを使用した属性の追加を行えます。

![](../images/profile-customization/profiles-edit-widget-basic-attributes.png)

## 属性の追加 {#add-attributes}

**[!UICONTROL 編集ウィジェット]** 画面から、カードの右上隅にある **[!UICONTROL 属性を追加]** を選択して、そのカードへの属性の追加を開始します。

![](../images/profile-customization/profiles-edit-widget-basic-add-attributes.png)

**[!UICONTROL 和集合スキーマフィールド]** を選択ダイアログが開くと、ダイアログの左側に完全な [!UICONTROL XDM 個別プロファイル ] の和集合スキーマが表示され、その下にフィールドがネストされます。 和集合スキーマの詳細については、『 [!DNL Profile]  ユーザガイド ](user-guide.md#union-schema)』の「[ 和集合スキーマ」の節を参照してください。

ダイアログの右側の **[!UICONTROL 選択した属性]** セクションに、現在編集中のカードに含まれている属性が表示されます。 ここでも属性の削除と並べ替えを行うことができます。 選択した属性の総数と、1 つのカードに追加できる属性の最大数 (20) が表示されます。

![](../images/profile-customization/profiles-select-field-before.png)

使用可能な任意の和集合スキーマフィールドを選択して、編集するカードの属性をカスタマイズできます。 選択したフィールドの横にチェックマークが付き、選択した属性のリストに自動的に追加されます。 カードに表示したい属性をすべて追加したら、**[!UICONTROL 選択]** を選択して **[!UICONTROL 編集ウィジェット]** 画面に戻ります。

![](../images/profile-customization/profiles-select-field-after.png)

**[!UICONTROL 編集ウィジェット]** 画面に戻ると、選択内容を反映するように、カードの属性のリストが更新されます。 必要に応じて、カード属性の削除や並べ替えを行ったり、カードタイトルを編集したりできます。 編集が完了したら、「**[!UICONTROL 保存]**」を選択して変更を保存します。

![](../images/profile-customization/profiles-edit-widget-new-attributes.png)

保存すると、「**[!UICONTROL 詳細]**」タブに戻り、更新されたカードと属性が表示されます。

![](../images/profile-customization/profiles-resized-card-new-attributes.png)

## 新しいカードの追加 {#add-a-new-card}

Experience Platform内のプロファイルの外観をさらにカスタマイズするには、新しいカードをダッシュボードに追加し、それらのカードに表示する属性を選択します。 まず、「**[!UICONTROL 詳細]**」タブで「**[!UICONTROL ダッシュボード]** を変更」を選択します。

![](../images/profile-customization/profiles-modify-dashboard.png)

次に、ダッシュボードの左上隅にある「**[!UICONTROL ウィジェット]** を追加」を選択します。

![](../images/profile-customization/profiles-add-widget.png)

新しいカードを追加することを選択すると、**[!UICONTROL 編集ウィジェット]** 画面が開き、新しいカードのタイトルを指定し、表示するカードの属性を選択できます。 カードへの属性の追加を開始するには、**[!UICONTROL 属性の追加]** を選択します。

![](../images/profile-customization/profiles-edit-new-widget.png)

**[!UICONTROL 和集合スキーマフィールドを選択]** ダイアログが開くと、ダイアログの左側に完全な [!UICONTROL XDM 個別プロファイル ] 和集合スキーマが表示され、ダイアログの右側にある **[!UICONTROL 選択済み属性]** セクションに、カード用に選択した属性が表示されます。 属性の追加の詳細については、このドキュメントで前述した属性 ](#add-attributes) の追加に関する [ の節を参照してください。

選択した属性の総数と、1 つのカードに追加できる属性の最大数 (20) が表示されます。 また、この画面から選択した属性を削除して並べ替えることもできます。 カードに表示するすべての属性を追加したら、**[!UICONTROL 選択]** を選択して **[!UICONTROL 編集ウィジェット]** 画面に戻ります。

![](../images/profile-customization/profiles-add-fields-new-widget.png)

**[!UICONTROL 編集ウィジェット]** 画面に戻ると、カードの属性のリストに前の画面からの選択が反映されます。 必要に応じて、カード属性の順序を変更したり、カード属性を削除したりすることもできます。

新しいカードを保存するには、まず **[!UICONTROL カードのタイトル]** を指定する必要があります。その後、**[!UICONTROL 保存]** を選択して、カード作成プロセスを完了できます。

![](../images/profile-customization/profiles-edit-new-widget-with-fields.png)

保存後、「**[!UICONTROL 詳細]**」タブに戻り、新しいカードと属性が表示されます。

![](../images/profile-customization/profiles-detail-new-widget.png)

## デフォルトのカードを復元

いつでも、取り外したデフォルトのカードを復元したいと思う場合は、すばやく簡単に復元できます。 まず、「**[!UICONTROL ダッシュボードを変更]**」を選択し、「**[!UICONTROL デフォルトのカードを復元]**」を選択します。 デフォルトのカードが表示されたら、「**[!UICONTROL 保存]**」を選択して変更内容を保存するか、「**[!UICONTROL キャンセル]**」を選択してデフォルトのカードを復元しない場合に選択します。

![](../images/profile-customization/profiles-restore-default.png)

## 次の手順

このドキュメントに従うと、カードの追加と削除、カードの詳細と属性の編集、カードの並べ替えとサイズ変更など、組織の縦断ビューを更新できます。 Experience PlatformUI での [!DNL Profile] データの操作について詳しくは、[[!DNL Profile]  ユーザーガイド ](user-guide.md) を参照してください。
