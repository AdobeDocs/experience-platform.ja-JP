---
keywords: Experience Platform;home;popular topics;ui;UI;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema editor;Schema Editor;schema;Schema;schemas;Schemas;create;relationship;Relationship;reference;Reference;
solution: Experience Platform
title: スキーマエディターを使用した 2 つのスキーマ間の関係の定義
description: このドキュメントでは、Experience Platformユーザーインターフェイスのスキーマエディタを使用して、2つのスキーマ間の関係を定義するためのチュートリアルを提供します。
topic: tutorial
type: Tutorial
translation-type: tm+mt
source-git-commit: 097fe219e0d64090de758f388ba98e6024db2201
workflow-type: tm+mt
source-wordcount: '910'
ht-degree: 33%

---


# Define a relationship between two schemas using the [!DNL Schema Editor]

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platform の重要な部分です。Defining these relationships within the structure of your [!DNL Experience Data Model] (XDM) schemas allows you to gain complex insights into your customer data.

スキーマの関係は、和集合スキーマを使用して推論できますが、 [!DNL Real-time Customer Profile]これは同じクラスを共有するスキーマにのみ適用されます。 異なるクラスに属する2つのスキーマ間の関係を確立するには、目的のスキーマのIDを参照するソーススキーマに、専用の関係フィールドを追加する必要があります。

This document provides a tutorial for defining a relationship between two schemas using the Schema Editor in the [!DNL Experience Platform] user interface. API を使用してスキーマ関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。

## はじめに

This tutorial requires a working understanding of [!DNL XDM System] and the Schema Editor in the [!DNL Experience Platform] UI. このチュートリアルを始める前に、次のドキュメントを確認してください。

* [Experience PlatformのXDMシステム](../home.md):XDMとその実装の概要を、で説明し [!DNL Experience Platform]ます。
* [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [次を使用してスキーマを作成します [!DNL Schema Editor]](create-schema-ui.md)。を使用する基本的な作業に関するチュートリアル [!DNL Schema Editor]です。

## ソースと宛先のスキーマの定義

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。For demonstration purposes, this tutorial creates a relationship between members of an organization&#39;s loyalty program (defined in a &quot;[!DNL Loyalty Members]&quot; schema) and their favorite hotel (defined in a &quot;[!DNL Hotels]&quot; schema).

>[!IMPORTANT]
>
>関係を確立するには、両方のスキーマがプライマリIDを定義し、有効にする必要があり [!DNL Real-time Customer Profile]ます。 スキーマをそれに応じて設定する方法のガイダンスが必要な場合は、「プロファイルの作成」チュートリアルの「スキーマをスキーマで使用できるようにする」のセクションを参照して [](./create-schema-ui.md#profile) ください。

スキーマの関係は、 **ソーススキーマ内の、** 宛先スキーマ内の別のフィールドを参照する専用のフィールドで表されます ****。 In the steps that follow, &quot;[!DNL Loyalty Members]&quot; will be the source schema, while &quot;[!DNL Hotels]&quot; will act as the destination schema.

以下の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。

### [!DNL Loyalty Members] schema

The source schema &quot;[!DNL Loyalty Members]&quot; is based on the [!DNL XDM Individual Profile] class, and is the schema that was constructed in the tutorial for [creating a schema in the UI](create-schema-ui.md). It includes a `loyalty` object under its `_tenantId` namespace, which includes several loyalty-specific fields. One of these fields, `loyaltyId`, serves as the primary identity for the schema under the [!UICONTROL Email] namespace. As seen under **[!UICONTROL Schema Properties]**, this schema has been enabled for use in [!DNL Real-time Customer Profile].

![](../images/tutorials/relationship/loyalty-members.png)

### [!DNL Hotels] schema

宛先スキーマ「[!DNL Hotels]」は、カスタムの「[!DNL Hotels]」クラスに基づいており、ホテルを説明するフィールドが含まれています。 The `hotelId` field serves as the primary identity for the schema under a custom `hotelId` namespace. スキーマと同様に、 [!DNL Loyalty Members] このスキーマもに対して有効になってい [!DNL Real-time Customer Profile]ます。

![](../images/tutorials/relationship/hotels.png)

## 関係の mixin の作成

>[!NOTE]
>
>この手順は、ソーススキーマに、ターゲットスキーマへの参照として使用する専用の文字列型フィールドがない場合にのみ必要です。 このフィールドがソーススキーマで既に定義されている場合は、次の[関係フィールドを定義](#relationship-field)する手順に進んでください。

2 つのスキーマ間の関係を定義するには、ソーススキーマに、ターゲットスキーマへの参照として使用する専用のフィールドが必要です。新しい mixin を作成して、このフィールドをソーススキーマに追加することができます。

Start by selecting **[!UICONTROL Add]** in the **[!UICONTROL Mixins]** section.

![](../images/tutorials/relationship/loyalty-add-mixin.png)

[!UICONTROL Mixin を追加]ダイアログが表示されます。From here, select **[!UICONTROL Create new mixin]**. 表示されるテキストフィールドに、新しい mixin の表示名と説明を入力します。Select **[!UICONTROL Add mixin]** when finished.

<img src="../images/tutorials/relationship/loyalty-create-new-mixin.png" width="750"><br>

The canvas reappears with &quot;[!DNL Favorite Hotel]&quot; appearing in the **[!UICONTROL Mixins]** section. Select the mixin name, then select **[!UICONTROL Add field]** next to the root-level `Loyalty Members` field.

![](../images/tutorials/relationship/loyalty-add-field.png)

A new field appears in the canvas under the `_tenantId` namespace. Under **[!UICONTROL Field properties]**, provide a field name and display name for the field, and set its type to &quot;[!UICONTROL String]&quot;.

![](../images/tutorials/relationship/relationship-field-details.png)

When finished, select **[!UICONTROL Apply]**.

![](../images/tutorials/relationship/relationship-field-apply.png)

The updated `favoriteHotel` field appears in the canvas. Select **[!UICONTROL Save]** to finalize your changes to the schema.

![](../images/tutorials/relationship/relationship-field-save.png)

## ソーススキーマでの関係フィールドの定義 {#relationship-field}

ソーススキーマに専用の参照フィールドが定義されたら、それを関係フィールドとして指定できます。

Select the `favoriteHotel` field in the canvas, then scroll down under **[!UICONTROL Field properties]** until the **[!UICONTROL Relationship]** checkbox appears. このチェックボックスを選択すると、関係フィールドを設定するために必要なパラメーターが表示されます。

![](../images/tutorials/relationship/relationship-checkbox.png)

Select the dropdown for **[!UICONTROL Reference schema]** and select the destination schema for the relationship (&quot;[!DNL Hotels]&quot; in this example). If the destination schema is enabled for [!DNL Profile], the **[!UICONTROL Reference identity namespace]** field is automatically set to the namespace of the destination schema&#39;s primary identity. スキーマにプライマリ ID が定義されていない場合は、使用する名前空間をドロップダウンメニューから手動で選択する必要があります。Select **[!UICONTROL Apply]** when finished.

![](../images/tutorials/relationship/reference-schema-id-namespace.png)

The `favoriteHotel` field is now highlighted as a relationship in the canvas, displaying the name and reference identity namespace of the destination schema. Select **[!UICONTROL Save]** to save your changes and complete the workflow.

![](../images/tutorials/relationship/relationship-save.png)

## 次の手順

By following this tutorial, you have successfully created a one-to-one relationship between two schemas using the [!DNL Schema Editor]. API を使用して関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。