---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマスキーマエディタを使用した2つのスキーマ間の関係の定義
topic: tutorials
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 0%

---


# スキーマエディタを使用した2つのスキーマ間の関係の定義

様々なチャネルにわたる顧客とブランドとの関係を理解する能力は、Adobe Experience Platformの重要な部分です。 エクスペリエンスデータモデル(XDM)スキーマの構造内でこれらの関係を定義すると、顧客データに対する複雑なインサイトを得ることができます。

このドキュメントでは、Experience Platformユーザーインターフェイスのスキーマエディタを使用して、組織で定義された2つのスキーマ間の1対1の関係を定義するためのチュートリアルを提供します。 APIを使用してスキーマの関係を定義する手順については、スキーマレジストリAPIを使用して関係を [定義する方法のチュートリアルを参照してください](relationship-api.md)。

## はじめに

このチュートリアルでは、XDMシステムとExperience PlatformUIのスキーマエディタに関する十分な理解が必要です。 このチュートリアルを開始する前に、次のドキュメントを確認してください。

* [Experience PlatformのXDMシステム](../home.md): XDMとExperience Platformでのその実装の概要を示します。
* [スキーマ構成の基本](../schema/composition.md): XDMスキーマの構築ブロックの紹介。
* [スキーマエディターを使用したスキーマの作成](create-schema-ui.md): スキーマエディタの基本操作に関するチュートリアルです。

## ソースと宛先のスキーマの定義

この関係で定義される2つのスキーマを既に作成済みであることが想定されます。 このチュートリアルでは、デモ目的で、組織の忠誠度プログラム(「忠誠度メンバー」スキーマで定義)のメンバーと、お気に入りのホテル(「ホテル」スキーマで定義)との間に関係を作成します。

スキーマ関係は、 **ソーススキーマ** ( **ソーススキーマ**)が表し、ターゲット内の別のフィールドを参照するフィールドを持ちます。 次の手順では、「Loyalty Members」がソーススキーマになり、「Hotels」がターゲットスキーマになります。

以下の節では、関係を定義する前に、このチュートリアルで使用する各スキーマの構造について説明します。

### 忠誠度メンバースキーマ

ソーススキーマ「Loyality Members」は、UIでスキーマを [作成するためのチュートリアルで構築されたスキーマです](create-schema-ui.md)。 これには、「\_tenantId」名前空間の下に「忠誠度」オブジェクトが含まれ、このオブジェクトには複数の忠誠度固有のフィールドが含まれます。 これらのフィールドの1つである「loyalityId」が、「電子メール」名前空間下のスキーマの主なIDとして機能します。 「 _スキーマのプロパティ_」で確認できるように、このスキーマは [リアルタイム顧客プロファイルでの使用が有効になっています](../../profile/home.md)。

![](../images/tutorials/relationship/loyalty-members.png)

### ホテルズスキーマ

目的のスキーマ「ホテル」には、ホテルの説明、住所、ブランド、客室数、および星評価のフィールドが含まれます。 「hotelId」フィールドは、「ECID」名前空間の下のスキーマの主なIDとして機能します。 「忠誠度メンバー」とは異なり、このスキーマはリアルタイム顧客プロファイルに対して有効になっていません。

![](../images/tutorials/relationship/hotels.png)

## 関係ミックスインの作成

>[!NOTE]
>
>この手順は、ソーススキーマに、別のスキーマへの参照として使用する専用の文字列型フィールドがない場合にのみ必要です。 このフィールドがソーススキーマで既に定義されている場合は、次の手順に進んで関係フィールドを [定義し](#relationship-field)ます。

2つのスキーマ間の関係を定義するには、ソーススキーマに、ターゲットスキーマへの参照として使用する専用のフィールドが必要です。 新しいMixinを作成して、このフィールドをソーススキーマに追加できます。

開始するには、 **Mixins**__ セクションのをクリックします。

![](../images/tutorials/relationship/loyalty-add-mixin.png)

[ _Mixin_ ]ダイアログが表示されます。 ここから、「新規Mixinを **作成**」をクリックします。 表示されるテキストフィールドに、新しいMixinの表示名と説明を入力します。 終了したら **追加「Mixin** 」をクリックします。

<img src="../images/tutorials/relationship/loyalty-create-new-mixin.png" width="750"><br>

キャンバスが再表示され、「 _Mixins_ 」セクションに「Loyality Relationship」が表示されます。 Mixin名をクリックし、ルートレベルの「 **追加Loyality Members」フィールドの横にある「** Field」をクリックします。

![](../images/tutorials/relationship/loyalty-add-field.png)

キャンバスの「\_tenantId」名前空間の下に新しいフィールドが表示されます。 「 _フィールドプロパティ_」で、フィールドの名前と表示名を指定し、タイプを「文字列」に設定します。

![](../images/tutorials/relationship/relationship-field-details.png)

終了したら、「 **適用**」をクリックします。

![](../images/tutorials/relationship/relationship-field-apply.png)

更新された「忠誠度の関係」フィールドがキャンバスに表示されます。 「 **保存** 」をクリックして、スキーマに対する変更を確定します。

![](../images/tutorials/relationship/relationship-field-save.png)

## ソーススキーマの関係フィールドの定義 {#relationship-field}

ソーススキーマに専用の参照フィールドが定義されたら、それを関係フィールドとして指定できます。

キャンバスの参照フィールドをクリックし、「 _フィールドプロパティ_ 」の下で下にスクロールして、「 **Relationship** 」チェックボックスが表示されます。 このチェックボックスを選択すると、関係フィールドの設定に必要なパラメータが表示されます。

![](../images/tutorials/relationship/relationship-checkbox.png)

[ **リファレンススキーマ** ]のドロップダウンをクリックし、関係の表示先スキーマを選択します（この例では「Hotels」）。 宛先スキーマが和集合対応の場合、「 **参照ID名前空間** 」フィールドは、宛先スキーマのプライマリIDの名前空間に自動的に設定されます。 スキーマにプライマリIDが定義されていない場合は、使用する名前空間をドロップダウンメニューから手動で選択する必要があります。 Click **Apply** when finished.

![](../images/tutorials/relationship/reference-schema-id-namespace.png)

フィールドはキャンバスに関係として表示され、宛先スキーマの名前と参照ID名前空間が表示されます。 「 **保存** 」をクリックして変更を保存し、ワークフローを完了します。

![](../images/tutorials/relationship/relationship-save.png)

## 次の手順

このチュートリアルに従うと、スキーマエディタを使用して2つのスキーマ間に1対1の関係を作成できます。 APIを使用して関係を定義する手順については、スキーマレジストリAPIを使用した関係の [定義に関するチュートリアルを参照してください](relationship-api.md)。