---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマエディターを使用した 2 つのスキーマ間の関係の定義
topic: tutorials
translation-type: tm+mt
source-git-commit: d847329f675c7ac34a4feabb9e57a9e97f7e3ed1
workflow-type: tm+mt
source-wordcount: '911'
ht-degree: 44%

---


# Define a relationship between two schemas using the [!DNL Schema Editor]

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platform の重要な部分です。Defining these relationships within the structure of your [!DNL Experience Data Model] (XDM) schemas allows you to gain complex insights into your customer data.

スキーマの関係は、和集合スキーマを使用して推論できますが、 [!DNL Real-time Customer Profile]これは同じクラスを共有するスキーマにのみ適用されます。 異なるクラスに属する2つのスキーマ間の関係を確立するには、目的のスキーマのIDを参照するソーススキーマに、専用の **関係フィールド** を追加する必要があります。

This document provides a tutorial for defining a relationship between two schemas using the Schema Editor in the [!DNL Experience Platform] user interface. API を使用してスキーマ関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。

## はじめに

This tutorial requires a working understanding of [!DNL XDM System] and the Schema Editor in the [!DNL Experience Platform] UI. このチュートリアルを始める前に、次のドキュメントを確認してください。

* [Experience PlatformのXDMシステム](../home.md):XDMとその実装の概要を、で説明し [!DNL Experience Platform]ます。
* [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [スキーマエディターを使用したスキーマの作成](create-schema-ui.md):を使用する基本的な作業に関するチュートリアル [!DNL Schema Editor]です。

## ソースと宛先のスキーマの定義

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。For demonstration purposes, this tutorial creates a relationship between members of an organization&#39;s loyalty program (defined in a &quot;[!UICONTROL Loyalty Members]&quot; schema) and their favorite hotels (defined in a &quot;[!DNL Hotels]&quot; schema).

>[!IMPORTANT]
>
>関係を確立するには、両方のスキーマがプライマリIDを定義し、有効にする必要があり [!DNL Real-time Customer Profile]ます。 スキーマをそれに応じて設定する方法のガイダンスが必要な場合は、「プロファイルの作成」チュートリアルの「スキーマをスキーマで使用できるようにする」のセクションを参照して [](./create-schema-ui.md#profile) ください。

スキーマの関係は、 **ソーススキーマ内の、** 宛先スキーマ内の別のフィールドを参照する専用のフィールドで表されます ****。 In the steps that follow, &quot;[!UICONTROL Loyalty Members]&quot; will be the source schema, while &quot;[!DNL Hotels]&quot; will act as the destination schema.

以下の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。

### [!UICONTROL Loyalty Members スキーマ]

ソーススキーマ「[!UICONTROL Loyality Members]」はXDM [!DNL Individual Profile] クラスに基づいており、UIでスキーマを [作成するためのチュートリアルで構築されたスキーマです](create-schema-ui.md)。 It includes a &quot;[!UICONTROL loyalty]&quot; object under its &quot;\_tenantId&quot; namespace, which includes several loyalty-specific fields. One of these fields, &quot;loyaltyId&quot;, serves as the primary identity for the schema under the &quot;[!UICONTROL Email]&quot; namespace. As seen under _[!UICONTROL Schema Properties]_, this schema has been enabled for use in[!DNL Real-time Customer Profile].

![](../images/tutorials/relationship/loyalty-members.png)

### Hotels スキーマ

行き先のスキーマ「[!UICONTROL Hotels]」は、カスタムの「[!UICONTROL Hotels]」クラスに基づいており、ホテルを説明するフィールドが含まれています。 The &quot;[!DNL hotelId]&quot; field serves as the primary identity for the schema under a custom &quot;[!DNL hotelId]&quot; namespace. 「[!UICONTROL 忠誠度メンバー]」と同様、このスキーマも有効になってい [!DNL Real-time Customer Profile]ます。

![](../images/tutorials/relationship/hotels.png)

## 関係の mixin の作成

>[!NOTE]
>
> この手順は、ソーススキーマに別のスキーマへの参照として使用する専用の文字列型フィールドがない場合にのみ必要です。このフィールドがソーススキーマで既に定義されている場合は、次の[関係フィールドを定義](#relationship-field)する手順に進んでください。

2 つのスキーマ間の関係を定義するには、ソーススキーマに、ターゲットスキーマへの参照として使用する専用のフィールドが必要です。新しい mixin を作成して、このフィールドをソーススキーマに追加することができます。

まず、「_[!UICONTROL Mixins]_」セクションの「**[!UICONTROL &#x200B;追加&#x200B;]**」をクリックします。

![](../images/tutorials/relationship/loyalty-add-mixin.png)

_[!UICONTROL Mixin を追加]_ダイアログが表示されます。ここから、「**[!UICONTROL &#x200B;新規 mixin を作成&#x200B;]**」をクリックします。表示されるテキストフィールドに、新しい mixin の表示名と説明を入力します。終了したら「**[!UICONTROL  mixin を追加&#x200B;]**」をクリックします。

<img src="../images/tutorials/relationship/loyalty-create-new-mixin.png" width="750"><br>

The canvas reappears with &quot;[!UICONTROL Loyalty Relationship]&quot; appearing in the _[!UICONTROL Mixins]_section. Click the mixin name, then click**[!UICONTROL  Add Field ]**next to the root-level &quot;[!UICONTROL Loyalty Members]&quot; field.

![](../images/tutorials/relationship/loyalty-add-field.png)

キャンバスの「\_tenantId」名前空間の下に新しいフィールドが表示されます。Under _[!UICONTROL Field Properties]_, provide a field name and display name for the field, and set its type to &quot;[!UICONTROL String]&quot;.

![](../images/tutorials/relationship/relationship-field-details.png)

完了したら、「**[!UICONTROL 適用]**」をクリックします。

![](../images/tutorials/relationship/relationship-field-apply.png)

The updated &quot;[!UICONTROL favoriteHotel]&quot; field appears in the canvas. 「**[!UICONTROL 保存]**」をクリックし、変更をスキーマに確定します。

![](../images/tutorials/relationship/relationship-field-save.png)

## ソーススキーマでの関係フィールドの定義 {#relationship-field}

ソーススキーマに専用の参照フィールドが定義されたら、それを関係フィールドとして指定できます。

Select the reference field in the canvas, then scroll down under _[!UICONTROL Field Properties]_until the**[!UICONTROL  Relationship ]**checkbox appears. このチェックボックスを選択すると、関係フィールドを設定するために必要なパラメーターが表示されます。

![](../images/tutorials/relationship/relationship-checkbox.png)

Select the dropdown for **[!UICONTROL Reference Schema]** and select the destination schema for the relationship (&quot;[!UICONTROL Hotels]&quot; in this example). If the destination schema is enabled for Profile, the **[!UICONTROL Reference Identity Namespace]** field is automatically set to the namespace of the destination schema&#39;s primary identity. スキーマにプライマリ ID が定義されていない場合は、使用する名前空間をドロップダウンメニューから手動で選択する必要があります。終了したら「**[!UICONTROL 適用]**」をクリックします。

![](../images/tutorials/relationship/reference-schema-id-namespace.png)

フィールドは、キャンバスに関係として表示され、宛先スキーマの名前と参照 ID 名前空間が表示されます。「**[!UICONTROL 保存]**」をクリックし 、変更を保存し、ワークフローを完了します。

![](../images/tutorials/relationship/relationship-save.png)

## 次の手順

By following this tutorial, you have successfully created a one-to-one relationship between two schemas using the [!DNL Schema Editor]. API を使用して関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。