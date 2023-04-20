---
keywords: Experience Platform；ホーム；人気のトピック；UI;XDM;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；スキーマエディター；スキーマ；スキーマ；スキーマ；作成；関係；参照；
solution: Experience Platform
title: スキーマエディターを使用した 2 つのスキーマ間の関係の定義
description: このドキュメントでは、スキーマユーザーインターフェイスのスキーマエディターを使用して 2 つのスキーマ間の関係を定義するためのExperience Platformを提供します。
type: Tutorial
exl-id: feed776b-bc8d-459b-9700-e5c9520788c0
source-git-commit: 5caa4c750c9f786626f44c3578272671d85b8425
workflow-type: tm+mt
source-wordcount: '1109'
ht-degree: 27%

---

# 2 つのスキーマ間で 1 対 1 の関係を定義するには、 [!DNL Schema Editor] {#relationship-ui}

>[!CONTEXTUALHELP]
>id="platform_schemas_relationships"
>title="スキーマの関係"
>abstract="異なるクラスに属しているスキーマは、関係フィールドを使用して文脈的ににリンクできるので、より複雑なセグメント化ルールを作成できます。スキーマの関係について詳しくは、ドキュメントを参照してください。"

>[!CONTEXTUALHELP]
>id="platform_xdm_1to1_reference_schema"
>title="参照スキーマ"
>abstract="関係を確立するスキーマを選択します。このスキーマは、現在のスキーマとは異なるクラスである可能性があります。スキーマの関係について詳しくは、ドキュメントを参照してください。"

>[!CONTEXTUALHELP]
>id="platform_xdm_1to1_identity_namespace"
>title="参照 ID 名前空間"
>abstract="参照スキーマのプライマリ ID フィールドの名前空間 (タイプ)。参照スキーマは、関係に参加するために、確立されたプライマリ ID フィールドが必要です。スキーマの関係について詳しくは、ドキュメントを参照してください。"

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platform の重要な部分です。の構造内でこれらの関係を定義する [!DNL Experience Data Model] (XDM) スキーマを使用すると、顧客データに関する複雑なインサイトを得ることができます。

スキーマの関係は、結合スキーマと [!DNL Real-Time Customer Profile] を使用して推論できますが、同じクラスを共有するスキーマにのみ適用されます。 異なるクラスに属する 2 つのスキーマ間の関係を確立するには、他の関連するスキーマの ID を参照するソーススキーマに、専用の関係フィールドを追加する必要があります。

このドキュメントでは、 [!DNL Experience Platform] ユーザーインターフェイス。 API を使用してスキーマ関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。

>[!NOTE]
>
>Adobe Real-time Customer Data Platform B2B Edition で多対 1 の関係を作成する手順については、 [B2B 関係の作成](./relationship-b2b.md).

## はじめに

このチュートリアルでは、 [!DNL XDM System] と、 [!DNL Experience Platform] UI このチュートリアルを始める前に、次のドキュメントを確認してください。

* [XDM システムのExperience Platform](../home.md):XDM と、 [!DNL Experience Platform].
* [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [を使用してスキーマを作成する [!DNL Schema Editor]](create-schema-ui.md):の操作の基本を説明するチュートリアル [!DNL Schema Editor].

## ソースと参照スキーマの定義

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。このチュートリアルでは、デモ目的で、組織のロイヤルティプログラム (「[!DNL Loyalty Members]「 」スキーマ ) とそのお気に入りのホテル (「 」で定義[!DNL Hotels]&quot;スキーマ ) です。

>[!IMPORTANT]
>
>関係を確立するには、両方のスキーマでプライマリ ID が定義され、 [!DNL Real-Time Customer Profile]. 詳しくは、 [プロファイルで使用するスキーマの有効化](./create-schema-ui.md#profile) スキーマを適切に設定する方法に関するガイダンスが必要な場合は、スキーマ作成のチュートリアルを参照してください。

スキーマの関係は、 **ソーススキーマ** が **参照スキーマ**. 次の手順で、「[!DNL Loyalty Members]」がソーススキーマになり、「[!DNL Hotels]「 」は参照スキーマとして機能します。

次の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。

### [!DNL Loyalty Members] schema

ソーススキーマ「 」[!DNL Loyalty Members]」が [!DNL XDM Individual Profile] クラス。ロイヤルティプログラムのメンバーを説明するフィールドを含みます。 この一つのフィールドは `personalEmail.addess`は、の下のスキーマのプライマリ ID として機能します。 [!UICONTROL 電子メール] 名前空間。 以下に示すように **[!UICONTROL スキーマのプロパティ]**&#x200B;の場合、このスキーマはでの使用に対して有効になっています [!DNL Real-Time Customer Profile].

![](../images/tutorials/relationship/loyalty-members.png)

### [!DNL Hotels] schema

参照スキーマ「 」[!DNL Hotels]&quot;はカスタム&quot;[!DNL Hotels]「 」クラスで、ホテルを説明するフィールドを含んでいます。 関係に参加するには、参照スキーマにもプライマリ ID が定義され、に対して有効になっている必要があります。 [!UICONTROL プロファイル]. この場合、 `_tenantId.hotelId`は、カスタム「 」を使用して、スキーマのプライマリ ID として機能します。[!DNL Hotel ID]&quot; id 名前空間。

![プロファイルに対して有効にする](../images/tutorials/relationship/hotels.png)

>[!NOTE]
>
>カスタム ID 名前空間の作成方法については、 [ID サービスドキュメント](../../identity-service/namespaces.md#manage-namespaces).

## 関係フィールドグループの作成

>[!NOTE]
>
>この手順は、ソーススキーマに参照スキーマのプライマリ ID へのポインターとして使用する専用の文字列型フィールドがない場合にのみ必要です。 このフィールドがソーススキーマで既に定義されている場合は、次の[関係フィールドを定義](#relationship-field)する手順に進んでください。

2 つのスキーマ間の関係を定義するには、参照スキーマのプライマリ ID を示す専用のフィールドがソーススキーマに必要です。 新しいスキーマフィールドグループを作成するか、既存のスキーマフィールドグループを拡張することで、このフィールドをソーススキーマに追加できます。

の場合、 [!DNL Loyalty Members] スキーマ、新しい `preferredHotel` フィールドが追加され、ロイヤルティメンバーの会社訪問に関する優先ホテルを示します。 最初に、プラスアイコン (**+**) をクリックします。

![](../images/tutorials/relationship/loyalty-add-field.png)

キャンバスに新しいフィールドプレースホルダーが表示されます。 の下 **[!UICONTROL フィールドプロパティ]**」で、フィールドの名前と表示名を指定し、タイプを「[!UICONTROL 文字列]&quot;. の下 **[!UICONTROL 割り当て先]**、拡張する既存のフィールドグループを選択するか、固有の名前を入力して新しいフィールドグループを作成します。 この場合、新しい[!DNL Preferred Hotel]」フィールドグループが作成されます。

![](../images/tutorials/relationship/relationship-field-details.png)

完了したら、「**[!UICONTROL 適用]**」を選択します。

![](../images/tutorials/relationship/relationship-field-apply.png)

更新済み `preferredHotel` フィールドがキャンバスに表示されます。 `_tenantId` オブジェクトを選択します。 選択 **[!UICONTROL 保存]** 変更をスキーマに確定します。

![](../images/tutorials/relationship/relationship-field-save.png)

## ソーススキーマでの関係フィールドの定義 {#relationship-field}

ソーススキーマに専用の参照フィールドが定義されたら、それを関係フィールドとして指定できます。

>[!NOTE]
>
>以下の手順では、キャンバスの右側のパネルコントロールを使用して関係フィールドを定義する方法を説明します。 Real-Time CDP B2B Edition にアクセスできる場合は、 [同じ対話](./relationship-b2b.md#relationship-field) 多対 1 の関係を作成する場合と同様です。

を選択します。 `preferredHotel` キャンバスの「 」フィールドを選択し、下にスクロールします。 **[!UICONTROL フィールドプロパティ]** まで **[!UICONTROL 関係]** チェックボックスが表示されます。 このチェックボックスを選択すると、関係フィールドを設定するために必要なパラメーターが表示されます。

![](../images/tutorials/relationship/relationship-checkbox.png)

次のドロップダウンを選択します。 **[!UICONTROL 参照スキーマ]** をクリックし、関係の参照スキーマ ([!DNL Hotels]」と呼ばれます )。 の下 **[!UICONTROL 参照 ID 名前空間]**、参照スキーマの id フィールドの名前空間 ( この場合は「[!DNL Hotel ID]&quot;) です。 選択 **[!UICONTROL 適用]** 終了したとき。

![](../images/tutorials/relationship/reference-schema-id-namespace.png)

この `preferredHotel` フィールドがキャンバスで関係としてハイライト表示され、参照スキーマの名前が表示されます。 選択 **[!UICONTROL 保存]** 変更を保存し、ワークフローを完了します。

![](../images/tutorials/relationship/relationship-save.png)

## 次の手順

このチュートリアルでは、 [!DNL Schema Editor]. API を使用して関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。
