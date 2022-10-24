---
keywords: Experience Platform；ホーム；人気のトピック；UI;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマエディター；スキーマ；スキーマ；スキーマ；作成；関係；参照；
solution: Experience Platform
title: スキーマエディターを使用した 2 つのスキーマ間の関係の定義
description: このドキュメントでは、スキーマユーザーインターフェイスのスキーマエディターを使用して 2 つのスキーマ間の関係を定義するためのExperience Platformを提供します。
topic-legacy: tutorial
type: Tutorial
exl-id: feed776b-bc8d-459b-9700-e5c9520788c0
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '1173'
ht-degree: 21%

---

# 2 つのスキーマ間で 1 対 1 の関係を定義するには、 [!DNL Schema Editor] {#relationship-ui}

>[!CONTEXTUALHELP]
>id="platform_schemas_relationships"
>title="スキーマの関係"
>abstract="異なるクラスに属するスキーマは、関係フィールドを通じてコンテキスト上でリンクでき、より複雑なセグメント化ルールを作成できます。 スキーマの関係について詳しくは、ドキュメントを参照してください。"

>[!CONTEXTUALHELP]
>id="platform_xdm_1to1_reference_schema"
>title="参照スキーマ"
>abstract="関係を確立するスキーマを選択します。 このスキーマは、現在のスキーマとは異なるクラスにすることができます。 スキーマの関係について詳しくは、ドキュメントを参照してください。"

>[!CONTEXTUALHELP]
>id="platform_xdm_1to1_identity_namespace"
>title="参照 ID 名前空間"
>abstract="参照スキーマのプライマリ ID フィールドの名前空間（タイプ）。 関係に参加するには、参照スキーマに確立されたプライマリ ID フィールドが必要です。 スキーマの関係について詳しくは、ドキュメントを参照してください。"

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platform の重要な部分です。の構造内でこれらの関係を定義する [!DNL Experience Data Model] (XDM) スキーマを使用すると、顧客データに関する複雑なインサイトを得ることができます。

スキーマの関係は、和集合スキーマと [!DNL Real-time Customer Profile]同じクラスを共有するスキーマにのみ適用されます。 異なるクラスに属する 2 つのスキーマ間の関係を確立するには、宛先スキーマの ID を参照するソーススキーマに、専用の関係フィールドを追加する必要があります。

このドキュメントでは、 [!DNL Experience Platform] ユーザーインターフェイス。 API を使用してスキーマ関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。

>[!NOTE]
>
>Adobe Real-time Customer Data Platform B2B Edition で多対 1 の関係を作成する手順については、 [B2B 関係の作成](./relationship-b2b.md).

## はじめに

このチュートリアルでは、 [!DNL XDM System] と、 [!DNL Experience Platform] UI このチュートリアルを始める前に、次のドキュメントを確認してください。

* [XDM システムのExperience Platform](../home.md):XDM と、 [!DNL Experience Platform].
* [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [を使用してスキーマを作成する [!DNL Schema Editor]](create-schema-ui.md):の操作の基本を説明するチュートリアル [!DNL Schema Editor].

## ソースと宛先のスキーマの定義

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。このチュートリアルでは、デモ目的で、組織のロイヤルティプログラム (「[!DNL Loyalty Members]「 」スキーマ ) とそのお気に入りのホテル (「 」で定義[!DNL Hotels]&quot;スキーマ ) です。

>[!IMPORTANT]
>
>関係を確立するには、両方のスキーマでプライマリ ID が定義され、 [!DNL Real-time Customer Profile]. 詳しくは、 [プロファイルで使用するスキーマの有効化](./create-schema-ui.md#profile) スキーマを適切に設定する方法に関するガイダンスが必要な場合は、スキーマ作成のチュートリアルを参照してください。

スキーマの関係は、 **ソーススキーマ** は、 **宛先スキーマ**. 次の手順で、「[!DNL Loyalty Members]」がソーススキーマになり、「[!DNL Hotels]「 」が宛先スキーマとして機能します。

以下の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。

### [!DNL Loyalty Members] schema

ソーススキーマ「 」[!DNL Loyalty Members]」が [!DNL XDM Individual Profile] クラスであり、は、 [UI でのスキーマの作成](create-schema-ui.md). これには、 `loyalty` その下にある物 `_tenantId` 名前空間。複数のロイヤルティ固有のフィールドが含まれます。 この一つのフィールドは `loyaltyId`は、の下のスキーマのプライマリ ID として機能します。 [!UICONTROL 電子メール] 名前空間。 以下に示すように **[!UICONTROL スキーマのプロパティ]**&#x200B;の場合、このスキーマはでの使用に対して有効になっています [!DNL Real-time Customer Profile].

![](../images/tutorials/relationship/loyalty-members.png)

### [!DNL Hotels] スキーマ

宛先スキーマ「 」[!DNL Hotels]&quot;はカスタム&quot;[!DNL Hotels]「 」クラスで、ホテルを説明するフィールドを含んでいます。

![](../images/tutorials/relationship/hotels.png)

関係に参加するには、宛先スキーマにプライマリ ID が必要です。 この例では、 `hotelId` フィールドは、カスタムの「Hotel ID」ID 名前空間を使用して、プライマリ ID として使用されます。

![ホテルのプライマリ ID](../images/tutorials/relationship/hotel-identity.png)

>[!NOTE]
>
>カスタム ID 名前空間の作成方法については、 [ID サービスドキュメント](../../identity-service/namespaces.md#manage-namespaces).

プライマリ ID が設定されたら、宛先スキーマを有効にする必要があります。 [!DNL Real-time Customer Profile].

![プロファイルに対して有効にする](../images/tutorials/relationship/hotel-profile.png)

## 関係スキーマフィールドグループの作成

>[!NOTE]
>
>この手順は、ソーススキーマに、宛先スキーマへの参照として使用する専用の文字列タイプフィールドがない場合にのみ必要です。 このフィールドがソーススキーマで既に定義されている場合は、次の[関係フィールドを定義](#relationship-field)する手順に進んでください。

2 つのスキーマ間の関係を定義するには、ソーススキーマに、ターゲットスキーマへの参照として使用する専用のフィールドが必要です。新しいスキーマフィールドグループを作成して、このフィールドをソーススキーマに追加できます。

最初に選択 **[!UICONTROL 追加]** 内 **[!UICONTROL フィールドグループ]** 」セクションに入力します。

![](../images/tutorials/relationship/loyalty-add-field-group.png)

この [!UICONTROL フィールドグループを追加] ダイアログが表示されます。 ここからを選択します。 **[!UICONTROL 新しいフィールドグループを作成]**. 表示されるテキストフィールドに、新しいフィールドグループの表示名と説明を入力します。 選択 **[!UICONTROL フィールドグループを追加]** 終了したとき。

![](../images/tutorials/relationship/create-field-group.png)

キャンバスが「[!DNL Favorite Hotel]」が **[!UICONTROL フィールドグループ]** 」セクションに入力します。 フィールドグループ名を選択し、「 **[!UICONTROL フィールドを追加]** ルートレベルの横 `Loyalty Members` フィールドに入力します。

![](../images/tutorials/relationship/loyalty-add-field.png)

キャンバスの下に新しいフィールドが表示されます。 `_tenantId` 名前空間。 の下 **[!UICONTROL フィールドプロパティ]**」で、フィールドの名前と表示名を指定し、タイプを「[!UICONTROL 文字列]&quot;.

![](../images/tutorials/relationship/relationship-field-details.png)

終了したら、「 」を選択します。 **[!UICONTROL 適用]**.

![](../images/tutorials/relationship/relationship-field-apply.png)

更新済み `favoriteHotel` フィールドがキャンバスに表示されます。 選択 **[!UICONTROL 保存]** 変更をスキーマに確定します。

![](../images/tutorials/relationship/relationship-field-save.png)

## ソーススキーマでの関係フィールドの定義 {#relationship-field}

ソーススキーマに専用の参照フィールドが定義されたら、それを関係フィールドとして指定できます。

>[!NOTE]
>
>以下の手順では、キャンバスの右側のパネルコントロールを使用して関係フィールドを定義する方法を説明します。 Real-Time CDP B2B Edition にアクセスできる場合は、 [同じ対話](./relationship-b2b.md#relationship-field) 多対 1 の関係を作成する場合と同様です。

を選択します。 `favoriteHotel` キャンバスの「 」フィールドを選択し、下にスクロールします。 **[!UICONTROL フィールドプロパティ]** まで **[!UICONTROL 関係]** チェックボックスが表示されます。 このチェックボックスを選択すると、関係フィールドを設定するために必要なパラメーターが表示されます。

![](../images/tutorials/relationship/relationship-checkbox.png)

次のドロップダウンを選択します。 **[!UICONTROL 参照スキーマ]** をクリックし、関係の宛先スキーマを選択します (&quot;[!DNL Hotels]」と呼ばれます )。 宛先スキーマが [!DNL Profile]、 **[!UICONTROL 参照 ID 名前空間]** フィールドは、宛先スキーマのプライマリ id の名前空間に自動的に設定されます。 スキーマにプライマリ ID が定義されていない場合は、使用する名前空間をドロップダウンメニューから手動で選択する必要があります。選択 **[!UICONTROL 適用]** 終了したとき。

![](../images/tutorials/relationship/reference-schema-id-namespace.png)

この `favoriteHotel` フィールドがキャンバスで関係としてハイライトされ、宛先スキーマの名前と参照 id 名前空間が表示されるようになりました。 選択 **[!UICONTROL 保存]** 変更を保存し、ワークフローを完了します。

![](../images/tutorials/relationship/relationship-save.png)

## 次の手順

このチュートリアルでは、 [!DNL Schema Editor]. API を使用して関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。
