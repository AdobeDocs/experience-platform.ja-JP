---
keywords: Experience Platform；ホーム；人気のあるトピック；ui;UI;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；データモデル；スキーマエディタ；スキーマ；スキーマ；スキーマ；作成；関係；参照；
solution: Experience Platform
title: スキーマエディタを使用した 2 つのスキーマ間の関係の定義
description: このドキュメントでは、スキーマユーザーインターフェイスのスキーマエディタを使用して 2 つのスキーマ間の関係を定義するExperience Platformを提供します。
topic-legacy: tutorial
type: Tutorial
exl-id: feed776b-bc8d-459b-9700-e5c9520788c0
source-git-commit: 2118dc175b421e856c6b0a33a83a7238f01b7ee3
workflow-type: tm+mt
source-wordcount: '1020'
ht-degree: 24%

---

# [!DNL Schema Editor]

>[!NOTE]
>
>リアルタイム顧客データプラットフォーム B2B エディションを使用している場合は、代わりに [B2B の関係の作成 ](./relationship-b2b.md) に関するガイドを参照してください。

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platform の重要な部分です。[!DNL Experience Data Model](XDM) スキーマの構造内でこれらの関係を定義すると、顧客データに関する複雑なインサイトを得ることができます。

和集合スキーマと [!DNL Real-time Customer Profile] を使用してスキーマの関係を推論することはできますが、これは同じクラスを共有するスキーマにのみ当てはまります。 異なるクラスに属する 2 つのスキーマ間の関係を確立するには、宛先スキーマの ID を参照するソーススキーマに、専用の関係フィールドを追加する必要があります。

このドキュメントでは、[!DNL Experience Platform] ユーザーインターフェイスのスキーマエディターを使用して、2 つのスキーマ間の関係を定義するためのチュートリアルを提供します。 API を使用してスキーマ関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。

## はじめに

このチュートリアルでは、[!DNL XDM System] と [!DNL Experience Platform] UI のスキーマエディターに関する十分な知識が必要です。 このチュートリアルを始める前に、次のドキュメントを確認してください。

* [XDM システムのExperience Platform](../home.md):XDM と、での実装の概要で [!DNL Experience Platform]す。
* [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [ [!DNL Schema Editor]](create-schema-ui.md)を使用したスキーマの作成：の操作の基本を説明するチュートリアルで [!DNL Schema Editor]す。

## ソースと宛先のスキーマの定義

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。このチュートリアルでは、デモ目的で、組織のロイヤルティプログラム（「[!DNL Loyalty Members]」スキーマで定義）のメンバーと、お気に入りのホテル（「[!DNL Hotels]」スキーマで定義）との間に関係を作成します。

>[!IMPORTANT]
>
>関係を確立するには、両方のスキーマでプライマリ ID が定義され、[!DNL Real-time Customer Profile] に対して有効になっている必要があります。 スキーマを適切に設定する方法に関するガイダンスが必要な場合は、『スキーマ作成チュートリアル』の「プロファイル ](./create-schema-ui.md#profile) でのスキーマの使用の有効化 [」の節を参照してください。

スキーマの関係は、**ソーススキーマ** 内の専用のフィールドで表され、**宛先スキーマ** 内の別のフィールドを参照します。 次の手順では、「[!DNL Loyalty Members]」がソーススキーマで、「[!DNL Hotels]」が宛先スキーマです。

以下の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。

### [!DNL Loyalty Members] schema

ソーススキーマ「[!DNL Loyalty Members]」は [!DNL XDM Individual Profile] クラスに基づいており、[UI でのスキーマの作成 ](create-schema-ui.md) に関するチュートリアルで構築されたスキーマです。 `_tenantId` 名前空間の下に `loyalty` オブジェクトが含まれ、この中に複数のロイヤルティ固有のフィールドが含まれます。 これらのフィールドの 1 つ `loyaltyId` は、[!UICONTROL Email] 名前空間の下のスキーマのプライマリ ID として機能します。 **[!UICONTROL スキーマのプロパティ]** で見たように、このスキーマは [!DNL Real-time Customer Profile] で使用できるようになっています。

![](../images/tutorials/relationship/loyalty-members.png)

### [!DNL Hotels] スキーマ

宛先スキーマ「[!DNL Hotels]」は、カスタムの「[!DNL Hotels]」クラスに基づいており、ホテルを説明するフィールドが含まれています。

![](../images/tutorials/relationship/hotels.png)

関係に参加するには、宛先スキーマにプライマリ ID が必要です。 この例では、 `hotelId` フィールドは、カスタムの「Hotel ID」ID 名前空間を使用して、プライマリ ID として使用されます。

![ホテルの主要 ID](../images/tutorials/relationship/hotel-identity.png)

>[!NOTE]
>
>カスタム ID 名前空間の作成方法については、[ID サービスのドキュメント ](../../identity-service/namespaces.md#manage-namespaces) を参照してください。

プライマリ ID が設定されたら、宛先スキーマを [!DNL Real-time Customer Profile] に対して有効にする必要があります。

![プロファイルに対して有効](../images/tutorials/relationship/hotel-profile.png)

## 関係スキーマフィールドグループの作成

>[!NOTE]
>
>この手順は、ソーススキーマに、宛先スキーマへの参照として使用する専用の文字列型フィールドがない場合にのみ必要です。 このフィールドがソーススキーマで既に定義されている場合は、次の[関係フィールドを定義](#relationship-field)する手順に進んでください。

2 つのスキーマ間の関係を定義するには、ソーススキーマに、ターゲットスキーマへの参照として使用する専用のフィールドが必要です。新しいスキーマフィールドグループを作成して、このフィールドをソーススキーマに追加できます。

まず、「**[!UICONTROL フィールドグループ]**」セクションで「**[!UICONTROL 追加]**」を選択します。

![](../images/tutorials/relationship/loyalty-add-field-group.png)

[!UICONTROL  フィールドグループを追加 ] ダイアログが表示されます。 ここから、「**[!UICONTROL 新しいフィールドグループを作成]**」を選択します。 表示されるテキストフィールドに、新しいフィールドグループの表示名と説明を入力します。 終了したら「**[!UICONTROL フィールドグループを追加]**」を選択します。

![](../images/tutorials/relationship/create-field-group.png)

キャンバスが再び表示され、 **[!UICONTROL フィールドグループ]** セクションに「[!DNL Favorite Hotel]」が表示されます。 フィールドグループ名を選択し、ルートレベルの `Loyalty Members` フィールドの横にある「**[!UICONTROL フィールドを追加]**」を選択します。

![](../images/tutorials/relationship/loyalty-add-field.png)

キャンバスの `_tenantId` 名前空間の下に新しいフィールドが表示されます。 **[!UICONTROL フィールドのプロパティ]** で、フィールドの名前と表示名を指定し、型を「[!UICONTROL String]」に設定します。

![](../images/tutorials/relationship/relationship-field-details.png)

終了したら、「**[!UICONTROL 適用]**」を選択します。

![](../images/tutorials/relationship/relationship-field-apply.png)

更新された `favoriteHotel` フィールドがキャンバスに表示されます。 「**[!UICONTROL 保存]**」を選択して、変更をスキーマに確定します。

![](../images/tutorials/relationship/relationship-field-save.png)

## ソーススキーマでの関係フィールドの定義 {#relationship-field}

ソーススキーマに専用の参照フィールドが定義されたら、それを関係フィールドとして指定できます。

キャンバスで `favoriteHotel` フィールドを選択し、「**[!UICONTROL フィールドのプロパティ]**」の下で「**[!UICONTROL 関係]**」チェックボックスが表示されるまで下にスクロールします。 このチェックボックスを選択すると、関係フィールドを設定するために必要なパラメーターが表示されます。

![](../images/tutorials/relationship/relationship-checkbox.png)

「**[!UICONTROL 参照スキーマ]**」のドロップダウンを選択し、関係の宛先スキーマを選択します（この例では「[!DNL Hotels]」）。 宛先スキーマが [!DNL Profile] に対して有効になっている場合、「**[!UICONTROL 参照 ID 名前空間]**」フィールドは、宛先スキーマのプライマリ ID の名前空間に自動的に設定されます。 スキーマにプライマリ ID が定義されていない場合は、使用する名前空間をドロップダウンメニューから手動で選択する必要があります。終了したら「**[!UICONTROL 適用]**」を選択します。

![](../images/tutorials/relationship/reference-schema-id-namespace.png)

「 `favoriteHotel` 」フィールドがキャンバス内で関係としてハイライトされ、宛先スキーマの名前と参照 ID 名前空間が表示されます。 「**[!UICONTROL 保存]**」を選択して変更を保存し、ワークフローを完了します。

![](../images/tutorials/relationship/relationship-save.png)

## 次の手順

このチュートリアルでは、[!DNL Schema Editor] を使用して 2 つのスキーマ間に 1 対 1 の関係を作成しました。 API を使用して関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。
