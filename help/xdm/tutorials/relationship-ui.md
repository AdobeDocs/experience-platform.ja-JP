---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 2つの関係を定義するには、スキーマスキーマスキーマエディタ
topic: tutorials
translation-type: tm+mt
source-git-commit: f8c34d84e30ae14c3936c2e32ee84a2fcd3abdc3

---


# スキーマエディタを使用して2つのスキーマ間の関係を定義

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platformの重要な部分です。 エクスペリエンスデータモデル(XDM)スキーマの構造内でこれらの関係を定義すると、顧客データに対する複雑なインサイトを得ることができます。

このドキュメントでは、Experience Platformユーザーインターフェイスのスキーマエディターを使用して、組織で定義された2つのスキーマ間の1対1の関係を定義するためのチュートリアルを提供します。 APIを使用してスキーマ関係を定義する手順については、スキーマレジストリAPIを使用し [て関係を定義する方法のチュートリアルを参照してくださ](relationship-api.md)い。

## はじめに

このチュートリアルでは、XDMシステムとエクスペリエンスプラットフォームUIのスキーマエディターに関する十分な知識が必要です。 このチュートリアルを開始する前に、次のドキュメントを確認してください。

* [Experience PlatformのXDM System](../home.md):XDMとエクスペリエンスプラットフォームでの実装の概要を示します。
* [スキーマ構成の基本](../schema/composition.md):XDMスキーマの構築ブロックの紹介。
* [スキーマエディターを使用したスキーマの作成](create-schema-ui.md):チュートリアルでは、スキーマエディタの基本を説明します。

## ソースと宛先のスキーマ

この関係で定義される2つのスキーマが既に作成されていると想定されます。 このチュートリアルでは、デモ目的で、組織の忠誠度プログラム(「忠誠度メンバー」スキーマで定義)のメンバーと、お気に入りのホテル(「ホテル」スキーマで定義)との間に関係を作成します。

スキーマ関係は、宛先スキーマ **内の** 、別のフィールドを参照するフィールドを有するソーススキーマで表 **されます**。 次の手順では、「Loyalty Members」がソーススキーマになり、「Hotels」がターゲットスキーマになります。

以下の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。

### 忠誠度メンバーのスキーマ

ソーススキーマ「Loyalty Members」は、UIでのスキーマの作成のチュートリアルで構 [築されたスキーマです](create-schema-ui.md)。 これには、「\_tenantId」名前空間の下に「忠誠度」オブジェクトが含まれ、この中に複数の忠誠度固有のフィールドが含まれます。 これらのフィールドの1つ、「忠誠度ID」が、「電子メール」名前空間の下のスキーマのプライマリIDとして機能します。 「 _スキーマのプロパティ_」で見たように [、このスキーマはリアルタイムの顧客](../../profile/home.md)プロファイルで使用可能です。

![](../images/tutorials/relationship/loyalty-members.png)

### ホテルズスキーマ

目的のスキーマ「ホテル」には、ホテルの住所、ブランド、客室数、および星評価を示すフィールドが含まれます。 「hotelId」フィールドは、「ECID」名前空間の下のスキーマのプライマリIDです。 「忠誠度メンバー」とは異なり、このスキーマはリアルタイム顧客プロファイルに対して有効になっていません。

![](../images/tutorials/relationship/hotels.png)

## 関係のミックスインの作成

>[!NOTE] この手順は、ソーススキーマに別のスキーマへの参照として使用する専用の文字列型フィールドがない場合にのみ必要です。 このフィールドがソーススキーマで既に定義されている場合は、関係フィールドを定義する次の [手順に進んでください](#relationship-field)。

2つのスキーマ間の関係を定義するには、ソーススキーマに、ターゲットスキーマへの参照として使用する専用のフィールドが必要です。 新しいミックスインを作成して、このフィールドをソーススキーマに追加することができます。

開始を設定 **追加する** には _、_ Mixinsセクションをクリックします。

![](../images/tutorials/relationship/loyalty-add-mixin.png)

[ミッ _追加クスイン_ ]ダイアログが表示されます。 ここから、「新規Mixinを作 **成」をクリックしま**&#x200B;す。 表示されるテキストフィールドに、新しいミックスインの表示名と説明を入力します。 終了したら **追加「** Mixin」をクリックします。

<img src="../images/tutorials/relationship/loyalty-create-new-mixin.png" width="750"><br>

キャンバスが再び表示され、「Mixins」セクションに「Loyality Relationship」が表示 _されます_ 。 Mixin名をクリックし、ルートレベルの「 **追加Loyality Members** 」フィールドの横にある「Field」をクリックします。

![](../images/tutorials/relationship/loyalty-add-field.png)

キャンバスの「\_tenantId」名前空間の下に新しいフィールドが表示されます。 「フィ _ールドのプロパティ_」で、フィールドの名前と表示名を指定し、タイプを「文字列」に設定します。

![](../images/tutorials/relationship/relationship-field-details.png)

完了したら、「適用」をクリ **ックしま**&#x200B;す。

![](../images/tutorials/relationship/relationship-field-apply.png)

更新された「忠誠度の関係」フィールドがキャンバスに表示されます。 「保存」 **をクリックし** 、変更をスキーマに確定します。

![](../images/tutorials/relationship/relationship-field-save.png)

## ソース関係の関係フィールドを定義しますスキーマ {#relationship-field}

ソーススキーマに専用の参照フィールドが定義されたら、それを関係フィールドとして指定できます。

キャンバスの参照フィールドをクリックし、「 _Field Properties_ 」の下で下にスクロールして、「 **Relationship** 」チェックボックスが表示されます。 このチェックボックスを選択すると、関係フィールドを設定するために必要なパラメータが表示されます。

![](../images/tutorials/relationship/relationship-checkbox.png)

[リファレンススキーマ **** ]のドロップダウンをクリックし、関係の宛先スキーマを選択します（この例では「Hotels」）。 宛先スキーマが和集合対応の場合、「 **Reference Identity Identity名前空間** 」フィールドは、宛先スキーマのプライマリIDの名前空間に自動的に設定されます。 スキーマにプライマリIDが定義されていない場合は、使用する名前空間をドロップダウンメニューから手動で選択する必要があります。 Click **Apply** when finished.

![](../images/tutorials/relationship/reference-schema-id-namespace.png)

フィールドは、キャンバスに関係として表示され、宛先のスキーマの名前と参照ID名前空間が表示されます。 「保存」 **をクリックし** 、変更を保存し、ワークフローを完了します。

![](../images/tutorials/relationship/relationship-save.png)

## 次の手順

このチュートリアルに従うと、スキーマエディタを使用して2つのスキーマ間に1対1の関係を作成できます。 APIを使用して関係を定義する手順については、スキーマレジストリAPIを使用して関係を定義 [する方法のチュートリアルを参照してください](relationship-api.md)。