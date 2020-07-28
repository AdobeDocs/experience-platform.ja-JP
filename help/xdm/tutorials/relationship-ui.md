---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマエディターを使用した 2 つのスキーマ間の関係の定義
topic: tutorials
translation-type: tm+mt
source-git-commit: 1445646be8fa3416a34408205eadca0a792290c6
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 58%

---


# Define a relationship between two schemas using the [!DNL Schema Editor]

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platform の重要な部分です。Defining these relationships within the structure of your [!DNL Experience Data Model] (XDM) schemas allows you to gain complex insights into your customer data.

This document provides a tutorial for defining a one-to-one relationship between two schemas defined by your organization using the [!DNL Schema Editor] in the [!DNL Experience Platform] user interface. API を使用してスキーマ関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。

## はじめに

このチュートリアルでは、との [!DNL XDM System] UIに関する十分な理解が必要 [!DNL Schema Editor][!DNL Experience Platform] です。 このチュートリアルを始める前に、次のドキュメントを確認してください。

* [Experience Platform の XDM システム](../home.md)：XDM と Experience Platform での実装の概要を示します。
* [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [スキーマエディターを使用したスキーマの作成](create-schema-ui.md): を使用する基本的な作業に関するチュートリアル [!DNL Schema Editor]です。

## ソースと宛先のスキーマの定義

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。For demonstration purposes, this tutorial creates a relationship between members of an organization&#39;s loyalty program (defined in a &quot;[!UICONTROL Loyalty Members]&quot; schema) and their favorite hotels (defined in a &quot;Hotels&quot; schema).

スキーマ関係は、**[!UICONTROL 宛先スキーマ]**&#x200B;内の別のフィールドを参照するフィールドを有する&#x200B;**[!UICONTROL ソーススキーマ]**&#x200B;で表されます。In the steps that follow, &quot;[!UICONTROL Loyalty Members]&quot; will be the source schema, while &quot;Hotels&quot; will act as the destination schema.

以下の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。

### [!UICONTROL Loyalty Members スキーマ]

The source schema &quot;[!UICONTROL Loyalty Members]&quot; is the schema that was constructed in the tutorial for [creating a schema in the UI](create-schema-ui.md). It includes a &quot;[!UICONTROL loyalty]&quot; object under its &quot;[!UICONTROL \_tenantId]&quot; namespace, which includes several loyalty-specific fields. One of these fields, &quot;[!UICONTROL loyaltyId]&quot;, serves as the primary identity for the schema under the &quot;[!UICONTROL Email]&quot; namespace. As seen under _Schema Properties_, this schema has been enabled for use in [!DNL Real-time Customer Profile](../../profile/home.md).

![](../images/tutorials/relationship/loyalty-members.png)

### Hotels スキーマ

The destination schema &quot;[!UICONTROL Hotels]&quot; contains fields that describe a hotel, include its address, brand, number of rooms, and star rating. The &quot;[!UICONTROL hotelId]&quot; field serves as the primary identity for the schema under the &quot;ECID&quot; namespace. 「[!UICONTROL ロイヤルティメンバ]」とは異なり、このスキーマはに対して有効になっていません [!DNL Real-time Customer Profile]。

![](../images/tutorials/relationship/hotels.png)

## 関係の mixin の作成

>[!NOTE]
>
> この手順は、ソーススキーマに別のスキーマへの参照として使用する専用の文字列型フィールドがない場合にのみ必要です。このフィールドがソーススキーマで既に定義されている場合は、次の[関係フィールドを定義](#relationship-field)する手順に進んでください。

2 つのスキーマ間の関係を定義するには、ソーススキーマに、ターゲットスキーマへの参照として使用する専用のフィールドが必要です。新しい mixin を作成して、このフィールドをソーススキーマに追加することができます。

まず、「_Mixins_」セクションの「**[!UICONTROL 追加]**」をクリックします。

![](../images/tutorials/relationship/loyalty-add-mixin.png)

[!UICONTROL _Mixin を追加&#x200B;_]ダイアログが表示されます。ここから、「**[!UICONTROL &#x200B;新規 mixin を作成&#x200B;]**」をクリックします。表示されるテキストフィールドに、新しい mixin の表示名と説明を入力します。終了したら「**[!UICONTROL  mixin を追加&#x200B;]**」をクリックします。

<img src="../images/tutorials/relationship/loyalty-create-new-mixin.png" width="750"><br>

The canvas reappears with &quot;[!UICONTROL Favorite Hotel]&quot; appearing in the _Mixins_ section. Click the mixin name, then click **[!UICONTROL Add Field]** next to the root-level &quot;[!UICONTROL Loyalty Members]&quot; field.

![](../images/tutorials/relationship/loyalty-add-field.png)

A new field appears in the canvas under the &quot;[!UICONTROL \_tenantId]&quot; namespace. Under [!UICONTROL _Field Properties _], provide a field name and display name for the field, and set its type to &quot;[!UICONTROL String]&quot;.

![](../images/tutorials/relationship/relationship-field-details.png)

完了したら、「**[!UICONTROL 適用]**」をクリックします。

![](../images/tutorials/relationship/relationship-field-apply.png)

The updated &quot;[!UICONTROL favoriteHotel]&quot; field appears in the canvas. 「**[!UICONTROL 保存]**」をクリックし、変更をスキーマに確定します。

![](../images/tutorials/relationship/relationship-field-save.png)

## ソーススキーマでの関係フィールドの定義 {#relationship-field}

ソーススキーマに専用の参照フィールドが定義されたら、それを関係フィールドとして指定できます。

キャンバスの参照フィールドをクリックし、「_[!UICONTROL フィールドプロパティ]_」の下で「**[!UICONTROL &#x200B;関係&#x200B;]**」チェックボックスが表示されるまで下にスクロールします。このチェックボックスを選択すると、関係フィールドを設定するために必要なパラメーターが表示されます。

![](../images/tutorials/relationship/relationship-checkbox.png)

Click the dropdown for **[!UICONTROL Reference Schema]** and select the destination schema for the relationship (&quot;[!UICONTROL Hotels]&quot; in this example). 宛先スキーマが和集合対応の場合、「**[!UICONTROL 参照 ID 名前空間]**」フィールドは、宛先スキーマのプライマリ ID の名前空間に自動的に設定されます。スキーマにプライマリ ID が定義されていない場合は、使用する名前空間をドロップダウンメニューから手動で選択する必要があります。終了したら「**[!UICONTROL 適用]**」をクリックします。

![](../images/tutorials/relationship/reference-schema-id-namespace.png)

フィールドは、キャンバスに関係として表示され、宛先スキーマの名前と参照 ID 名前空間が表示されます。「**[!UICONTROL 保存]**」をクリックし 、変更を保存し、ワークフローを完了します。

![](../images/tutorials/relationship/relationship-save.png)

## 次の手順

By following this tutorial, you have successfully created a one-to-one relationship between two schemas using the [!DNL Schema Editor]. API を使用して関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。