---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマスキーマエディタを使用した2つのスキーマ間の関係の定義
topic: tutorials
translation-type: tm+mt
source-git-commit: d04bf35e49488ab7d5e07de91eb77d0d9921b6fa
workflow-type: tm+mt
source-wordcount: '811'
ht-degree: 0%

---


# 2つのスキーマ間の関係の定義には、 [!DNL Schema Editor]

様々なチャネルにわたる顧客とブランドとの関係を理解する能力は、Adobe Experience Platformの重要な部分です。 これらの関係を [!DNL Experience Data Model] (XDM)スキーマの構造内で定義すると、顧客データに対する複雑な洞察を得ることができます。

このドキュメントでは、ユー [!DNL Schema Editor][!DNL Experience Platform] ザーインターフェイスのを使用して、組織で定義された2つのスキーマ間の1対1の関係を定義するためのチュートリアルを提供します。 APIを使用してスキーマの関係を定義する手順については、スキーマレジストリAPIを使用して関係を [定義する方法のチュートリアルを参照してください](relationship-api.md)。

## はじめに

このチュートリアルでは、との [!DNL XDM System] UIに関する十分な理解が必要 [!DNL Schema Editor][!DNL Experience Platform] です。 このチュートリアルを開始する前に、次のドキュメントを確認してください。

* [Experience PlatformのXDMシステム](../home.md): XDMとExperience Platformでのその実装の概要を示します。
* [スキーマ構成の基本](../schema/composition.md): XDMスキーマの構築ブロックの紹介。
* [スキーマエディターを使用したスキーマの作成](create-schema-ui.md): を使用する基本的な作業に関するチュートリアル [!DNL Schema Editor]です。

## ソースと宛先のスキーマの定義

この関係で定義される2つのスキーマを既に作成済みであることが想定されます。 このチュートリアルでは、デモ目的で、組織の忠誠度プログラム(「[!UICONTROL 忠誠度メンバー]」スキーマで定義)のメンバーと、お気に入りのホテル(「ホテル」スキーマで定義)との間に関係を作成します。

スキーマ関係は、 **[!UICONTROL ソーススキーマ]** ( **[!UICONTROL ソーススキーマ]**)が表し、ターゲット内の別のフィールドを参照するフィールドを持ちます。 次の手順では、「[!UICONTROL Loyalty Members]」がソーススキーマになり、「Hotels」がターゲットスキーマになります。

以下の節では、関係を定義する前に、このチュートリアルで使用する各スキーマの構造について説明します。

### [!UICONTROL 忠誠度メンバー] スキーマ

ソーススキーマ「[!UICONTROL 忠誠度メンバー]」は、UIでのスキーマ [作成のチュートリアルで構築されたスキーマです](create-schema-ui.md)。 「[!UICONTROL \_tenantId]」名前空間の下に「[!UICONTROL loyality]」オブジェクトが含まれ、これには複数の忠誠度固有のフィールドが含まれます。 これらのフィールドの1つである「[!UICONTROL loyaltyId]」が、「[!UICONTROL 電子メール]」名前空間下のスキーマの主なIDとして機能します。 「 _スキーマのプロパティ_」の下に示すように、このスキーマはでの使用が有効になってい [!DNL Real-time Customer Profile](../../profile/home.md)ます。

![](../images/tutorials/relationship/loyalty-members.png)

### ホテルズスキーマ

リンク先スキーマ「[!UICONTROL ホテル]」には、ホテルの説明、住所、ブランド、客室数、スターレーティングを含むフィールドが含まれます。 「[!UICONTROL hotelId]」フィールドは、「ECID」名前空間下のスキーマの主なIDとして機能します。 「[!UICONTROL ロイヤルティメンバ]」とは異なり、このスキーマはに対して有効になっていません [!DNL Real-time Customer Profile]。

![](../images/tutorials/relationship/hotels.png)

## 関係ミックスインの作成

>[!NOTE]
>
>この手順は、ソーススキーマに、別のスキーマへの参照として使用する専用の文字列型フィールドがない場合にのみ必要です。 このフィールドがソーススキーマで既に定義されている場合は、次の手順に進んで関係フィールドを [定義し](#relationship-field)ます。

2つのスキーマ間の関係を定義するには、ソーススキーマに、ターゲットスキーマへの参照として使用する専用のフィールドが必要です。 新しいMixinを作成して、このフィールドをソーススキーマに追加できます。

開始するには、 **[!UICONTROL Mixins]**__ セクションのをクリックします。

![](../images/tutorials/relationship/loyalty-add-mixin.png)

[ [!UICONTROL _Mixin _]]ダイアログが表示されます。 ここから、「新規Mixinを**[!UICONTROL &#x200B;作成&#x200B;]**」をクリックします。 表示されるテキストフィールドに、新しいMixinの表示名と説明を入力します。 終了したら**[!UICONTROL &#x200B;追加「Mixin ]**」をクリックします。

<img src="../images/tutorials/relationship/loyalty-create-new-mixin.png" width="750"><br>

キャンバスが再表示され、「[!UICONTROL 忠誠度の関係]」が「ミ _ックスイン_ 」セクションに表示されます。 Mixin名をクリックし、ルートレベルの「 **[!UICONTROL Loyality Members]**[!UICONTROL 」フィールドの横にある「]Field」をクリックします。

![](../images/tutorials/relationship/loyalty-add-field.png)

キャンバスの「[!UICONTROL \_tenantId]」名前空間の下に新しいフィールドが表示されます。 「 [!UICONTROL _フィールドプロパティ&#x200B;_]」で、フィールドの名前と表示名を指定し、タイプを「[!UICONTROL 文字列]」に設定します。

![](../images/tutorials/relationship/relationship-field-details.png)

終了したら、「 **[!UICONTROL 適用]**」をクリックします。

![](../images/tutorials/relationship/relationship-field-apply.png)

更新された「[!UICONTROL 忠誠度の関係]」フィールドがキャンバスに表示されます。 「 **[!UICONTROL 保存]** 」をクリックして、スキーマに対する変更を確定します。

![](../images/tutorials/relationship/relationship-field-save.png)

## ソーススキーマの関係フィールドの定義 {#relationship-field}

ソーススキーマに専用の参照フィールドが定義されたら、それを関係フィールドとして指定できます。

キャンバスの参照フィールドをクリックし、「 _[!UICONTROL フィールドプロパティ]_」の下で下にスクロールして、「**[!UICONTROL  Relationship ]**」チェックボックスが表示されます。 このチェックボックスを選択すると、関係フィールドの設定に必要なパラメータが表示されます。

![](../images/tutorials/relationship/relationship-checkbox.png)

[ **[!UICONTROL リファレンススキーマ]** ]のドロップダウンをクリックし、リレーションシップのリンク先スキーマを選択します(この例では「[!UICONTROL Hotels]」)。 宛先スキーマが和集合対応の場合、「 **[!UICONTROL 参照ID名前空間]** 」フィールドは、宛先スキーマのプライマリIDの名前空間に自動的に設定されます。 スキーマにプライマリIDが定義されていない場合は、使用する名前空間をドロップダウンメニューから手動で選択する必要があります。 Click **[!UICONTROL Apply]** when finished.

![](../images/tutorials/relationship/reference-schema-id-namespace.png)

フィールドはキャンバスに関係として表示され、宛先スキーマの名前と参照ID名前空間が表示されます。 「 **[!UICONTROL 保存]** 」をクリックして変更を保存し、ワークフローを完了します。

![](../images/tutorials/relationship/relationship-save.png)

## 次の手順

このチュートリアルに従うと、を使用して2つのスキーマ間に1対1の関係を作成でき [!DNL Schema Editor]ます。 APIを使用して関係を定義する手順については、スキーマレジストリAPIを使用した関係の [定義に関するチュートリアルを参照してください](relationship-api.md)。