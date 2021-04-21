---
keywords: Experience Platform；ホーム；人気の高いトピック；ui;UI;XDM;XDM system；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマエディタ；スキーマエディタ；スキーマ;スキーマ;スキーマ；作成；リレーションシップ；リファレンス；
solution: Experience Platform
title: スキーマエディタを使用した2つのスキーマ間の関係の定義
description: このドキュメントでは、Experience Platformユーザーインターフェイスのスキーマエディタを使用して、2つのスキーマ間の関係を定義するためのチュートリアルを提供します。
topic-legacy: tutorial
type: Tutorial
exl-id: feed776b-bc8d-459b-9700-e5c9520788c0
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '945'
ht-degree: 31%

---

# [!DNL Schema Editor]

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platform の重要な部分です。[!DNL Experience Data Model] (XDM)スキーマの構造内でこれらの関係を定義すると、顧客データに対する複雑な洞察を得ることができます。

スキーマの関係は、和集合スキーマと[!DNL Real-time Customer Profile]を使用して推定できますが、これは同じクラスを共有するスキーマにのみ当てはまります。 異なるクラスに属する2つのスキーマ間の関係を確立するには、目的のスキーマのIDを参照するソーススキーマに、専用の関係フィールドを追加する必要があります。

このドキュメントでは、[!DNL Experience Platform]ユーザーインターフェイスのスキーマエディタを使用して、2つのスキーマ間の関係を定義するためのチュートリアルを提供します。 API を使用してスキーマ関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。

## はじめに

このチュートリアルでは、[!DNL XDM System]と[!DNL Experience Platform] UIのスキーマエディターに関する十分な理解が必要です。 このチュートリアルを始める前に、次のドキュメントを確認してください。

* [Experience PlatformのXDMシステム](../home.md):XDMとその実装の概要を、で説明し [!DNL Experience Platform]ます。
* [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [次を使用してスキーマを作成します [!DNL Schema Editor]](create-schema-ui.md)。を使用する基本的な作業に関するチュートリアル [!DNL Schema Editor]です。

## ソースと宛先のスキーマの定義

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。このチュートリアルでは、デモ目的で、組織の忠誠度プログラム(「[!DNL Loyalty Members]」スキーマで定義)のメンバーと、お気に入りのホテル(「[!DNL Hotels]」スキーマで定義)との間に関係を作成します。

>[!IMPORTANT]
>
>関係を確立するには、両方のスキーマがプライマリIDを定義し、[!DNL Real-time Customer Profile]に対して有効にしている必要があります。 スキーマの設定方法に関するガイダンスが必要な場合は、スキーマの作成チュートリアルの[プロファイルでのスキーマの使用を有効にする方法の節を参照してください。](./create-schema-ui.md#profile)

スキーマの関係は、**ソーススキーマ**&#x200B;内の、**ターゲットスキーマ**&#x200B;内の別のフィールドを参照する専用のフィールドで表されます。 次の手順では、&quot;[!DNL Loyalty Members]&quot;がソーススキーマ、&quot;[!DNL Hotels]&quot;がターゲットスキーマとして機能します。

以下の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。

### [!DNL Loyalty Members] schema

ソーススキーマ&quot;[!DNL Loyalty Members]&quot;は、[!DNL XDM Individual Profile]クラスに基づいており、[UI](create-schema-ui.md)でスキーマを作成するためのチュートリアルで構築されたスキーマです。 `_tenantId`名前空間の下に`loyalty`オブジェクトが含まれ、これには複数の忠誠度固有のフィールドが含まれます。 これらのフィールドの1つ`loyaltyId`は、[!UICONTROL 電子メール]名前空間の下のスキーマの主なIDとして機能します。 **[!UICONTROL スキーマのプロパティ]**&#x200B;に示すように、このスキーマは[!DNL Real-time Customer Profile]での使用が有効になっています。

![](../images/tutorials/relationship/loyalty-members.png)

### [!DNL Hotels] スキーマ

宛先スキーマ「[!DNL Hotels]」は、カスタムの「[!DNL Hotels]」クラスに基づいており、ホテルを説明するフィールドが含まれています。 `hotelId`フィールドは、カスタム`hotelId`名前空間の下でスキーマの主なIDとして機能します。 [!DNL Loyalty Members]スキーマと同様、このスキーマも[!DNL Real-time Customer Profile]に対して有効になっています。

![](../images/tutorials/relationship/hotels.png)

## 関係の mixin の作成

>[!NOTE]
>
>この手順は、ソーススキーマに、ターゲットスキーマへの参照として使用する専用の文字列型フィールドがない場合にのみ必要です。 このフィールドがソーススキーマで既に定義されている場合は、次の[関係フィールドを定義](#relationship-field)する手順に進んでください。

2 つのスキーマ間の関係を定義するには、ソーススキーマに、ターゲットスキーマへの参照として使用する専用のフィールドが必要です。新しい mixin を作成して、このフィールドをソーススキーマに追加することができます。

**[!UICONTROL ミックスイン]**&#x200B;セクションで&#x200B;**[!UICONTROL 追加]**&#x200B;を選択して開始します。

![](../images/tutorials/relationship/loyalty-add-mixin.png)

[!UICONTROL Mixin を追加]ダイアログが表示されます。ここから、「**[!UICONTROL 新しいミックスインを作成]**」を選択します。 表示されるテキストフィールドに、新しい mixin の表示名と説明を入力します。終了したら、**[!UICONTROL ミ追加ックスイン]**&#x200B;を選択します。

<img src="../images/tutorials/relationship/loyalty-create-new-mixin.png" width="750"><br>

キャンバスが再表示され、「[!DNL Favorite Hotel]」が&#x200B;**[!UICONTROL ミックスイン]**&#x200B;セクションに表示されます。 ミックスイン名を選択し、ルートレベル`Loyalty Members`フィールドの横にある追加&#x200B;**[!UICONTROL フィールド]**&#x200B;を選択します。

![](../images/tutorials/relationship/loyalty-add-field.png)

キャンバスの`_tenantId`名前空間の下に新しいフィールドが表示されます。 **[!UICONTROL フィールドプロパティ]**&#x200B;の下で、フィールド名とフィールドの表示名を指定し、型を「[!UICONTROL 文字列]」に設定します。

![](../images/tutorials/relationship/relationship-field-details.png)

終了したら、「**[!UICONTROL 適用]**」を選択します。

![](../images/tutorials/relationship/relationship-field-apply.png)

更新された`favoriteHotel`フィールドがキャンバスに表示されます。 「**[!UICONTROL 保存]**」を選択して、スキーマに対する変更を終了します。

![](../images/tutorials/relationship/relationship-field-save.png)

## ソーススキーマでの関係フィールドの定義 {#relationship-field}

ソーススキーマに専用の参照フィールドが定義されたら、それを関係フィールドとして指定できます。

キャンバスで`favoriteHotel`フィールドを選択し、**[!UICONTROL フィールドプロパティ]**&#x200B;の下で下にスクロールして、**[!UICONTROL 「Relationship」]**&#x200B;チェックボックスが表示されます。 このチェックボックスを選択すると、関係フィールドを設定するために必要なパラメーターが表示されます。

![](../images/tutorials/relationship/relationship-checkbox.png)

「**[!UICONTROL 参照スキーマ]**」のドロップダウンを選択し、関係の参照先スキーマを選択します（この例では「[!DNL Hotels]」）。 宛先スキーマが[!DNL Profile]に対して有効になっている場合、**[!UICONTROL 参照ID名前空間]**&#x200B;フィールドは、宛先スキーマのプライマリIDの名前空間に自動的に設定されます。 スキーマにプライマリ ID が定義されていない場合は、使用する名前空間をドロップダウンメニューから手動で選択する必要があります。終了したら「**[!UICONTROL 適用]**」を選択します。

![](../images/tutorials/relationship/reference-schema-id-namespace.png)

キャンバスで`favoriteHotel`フィールドがリレーションシップとしてハイライトされ、ターゲットスキーマの名前と参照ID名前空間が表示されます。 「**[!UICONTROL 保存]**」を選択して変更を保存し、ワークフローを完了します。

![](../images/tutorials/relationship/relationship-save.png)

## 次の手順

このチュートリアルに従うと、[!DNL Schema Editor]を使用して2つのスキーマ間に1対1の関係を作成できます。 API を使用して関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。
