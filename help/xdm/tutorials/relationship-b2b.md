---
title: リアルタイム顧客データプラットフォームB2Bエディションでの2つのスキーマ間の関係の定義
description: リアルタイム顧客データプラットフォームB2Bエディションで、2つのスキーマ間の多対1の関係を定義する方法を説明します。
source-git-commit: 5fd82b02eb25f3d575de695c2f2b14a5e5b18400
workflow-type: tm+mt
source-wordcount: '1201'
ht-degree: 6%

---

# リアルタイム顧客データプラットフォームB2Bエディションでの2つのスキーマ間の関係の定義

>[!NOTE]
>
>リアルタイム顧客データプラットフォームB2Bエディションを使用しない場合は、[非B2B関係の作成](./relationship-ui.md)に関するガイドを参照してください。

リアルタイム顧客データプラットフォームB2Bエディションは、[accounts](../classes/b2b/business-account.md)、[opportunity](../classes/b2b/business-opportunity.md)、[campaigns](../classes/b2b/business-campaign.md)など、基本的なB2Bデータエンティティをキャプチャする、複数のエクスペリエンスデータモデル(XDM)クラスを提供します。 これらのクラスに基づいてスキーマを構築し、[リアルタイム顧客プロファイル](../../profile/home.md)で使用できるようにすることで、異なるソースのデータを和集合スキーマと呼ばれる統合表現に結合できます。

ただし、和集合スキーマには、同じクラスを共有するスキーマによってキャプチャされたフィールドのみを含めることができます。 スキーマの関係はここで生じます。 B2Bスキーマに関係を実装することで、これらのビジネスエンティティが相互にどのように関係するかを説明し、ダウンストリームセグメント化の使用例で複数のクラスの属性を含めることができます。

次の図は、基本的な実装で様々なB2Bクラスを相互に関連付ける方法の例を示しています。

![B2Bクラスの関係](../images/tutorials/relationship-b2b/classes.png)

このチュートリアルでは、リアルタイムCDP B2Bエディションで2つのスキーマ間の多対1の関係を定義する手順を説明します。

>[!NOTE]
>
>このチュートリアルでは、Platform UIでB2Bスキーマ間の関係を手動で確立する方法に焦点を当てます。 B2Bソース接続からデータを取り込む場合は、自動生成ユーティリティを使用して、必要なスキーマ、ID、関係を代わりに作成できます。 自動生成ユーティリティ](../../sources/connectors/adobe-applications/marketo/marketo-namespaces.md)の使用について詳しくは、B2Bの名前空間とスキーマに関するソースのドキュメントを参照してください。[

## はじめに

このチュートリアルでは、[!DNL XDM System]と[!DNL Experience Platform] UIのスキーマエディターに関する十分な知識が必要です。 このチュートリアルを始める前に、次のドキュメントを確認してください。

* [Experience PlatformのXDMシステム](../home.md):XDMと、での実装の概要で [!DNL Experience Platform]す。
* [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [ [!DNL Schema Editor]](create-schema-ui.md)を使用してスキーマを作成します。UIでのスキーマの作成および編集方法の基本を説明するチュートリアルです。

## ソースと宛先のスキーマの定義

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。このチュートリアルでは、デモ目的で、ビジネスオポチュニティ（「[!DNL Opportunities]」スキーマで定義）と、関連するビジネスアカウント（「[!DNL Accounts]」スキーマで定義）との間に関係を作成します。

スキーマの関係は、**宛先スキーマ**&#x200B;のプライマリIDフィールドを参照する&#x200B;**ソーススキーマ**&#x200B;内の専用フィールドで表されます。 次の手順では、「[!DNL Opportunities]」がソーススキーマとして機能し、「[!DNL Accounts]」が宛先スキーマとして機能します。

### B2Bの関係のIDについて

関係を確立するには、両方のスキーマでプライマリIDが定義され、[!DNL Real-time Customer Profile]に対して有効になっている必要があります。 B2BエンティティのプライマリIDを設定する場合、文字列ベースのエンティティIDが異なるシステムや場所で収集されると、Platform内のデータの競合を引き起こす可能性があるので、重複する可能性があることに注意してください。

これを考慮するために、すべての標準B2Bクラスに、[[!UICONTROL B2Bソース]データ型](../data-types/b2b-source.md)に準拠する「キー」フィールドが含まれています。 このデータ型は、B2Bエンティティの文字列識別子のフィールドと、識別子のソースに関するその他のコンテキスト情報を提供します。 これらのフィールドの1つ`sourceKey`は、データ型内の他のフィールドの値を連結して、エンティティの完全に一意の識別子を生成します。 このフィールドは、常にB2BエンティティスキーマのプライマリIDとして使用する必要があります。

![sourceKeyフィールド](../images/tutorials/relationship-b2b/sourcekey.png)

>[!NOTE]
>
>[XDMフィールドをID](../ui/fields/identity.md)として設定する場合、IDを定義するID名前空間を指定する必要があります。 これは、Adobeが提供する標準の名前空間または組織が定義したカスタムの名前空間です。 実際には、名前空間はコンテキスト文字列で、組織がIDタイプを分類するうえで意味を持つ場合に、好きな値に設定できます。 詳しくは、[ID名前空間](../../identity-service/namespaces.md)の概要を参照してください。

以下の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。プライマリIDがスキーマ構造内で定義されている場所と、それらが使用するカスタム名前空間に注意します。

### [!DNL Opportunities] schema

ソーススキーマ「[!DNL Opportunities]」は、[!UICONTROL XDM Business Opportunity]クラスに基づいています。 クラスで提供されるフィールドの1つ`opportunityKey`は、スキーマの識別子として機能します。 特に、`opportunityKey`オブジェクトの下の`sourceKey`フィールドは、[!DNL B2B Opportunity]という名前のカスタム名前空間の下で、スキーマのプライマリIDとして設定されます。
**[!UICONTROL スキーマのプロパティ]**&#x200B;で見たように、このスキーマは[!DNL Real-time Customer Profile]で使用できるようになっています。

![オポチュニティスキーマ](../images/tutorials/relationship-b2b/opportunities.png)

### [!DNL Accounts] スキーマ

宛先スキーマ「[!DNL Accounts]」は、[!UICONTROL XDMアカウント]クラスに基づいています。 ルートレベルの`accountKey`フィールドには、[!DNL B2B Account]という名前のカスタム名前空間の下でプライマリIDとして機能する`sourceKey`が含まれます。 このスキーマは、プロファイルでも有効になっています。

![アカウントスキーマ](../images/tutorials/relationship-b2b/accounts.png)

## ソーススキーマでの関係フィールドの定義 {#relationship-field}

2つのスキーマ間の関係を定義するには、ソーススキーマに、宛先スキーマのプライマリIDを参照する専用のフィールドが必要です。 標準のB2Bクラスには、一般的に関連するビジネスエンティティ用の専用のソースキーフィールドが含まれます。 例えば、[!UICONTROL XDM Business Opportunity]クラスには、関連するアカウント(`accountKey`)および関連するキャンペーン(`campaignKey`)のソースキーフィールドが含まれます。 ただし、デフォルトのコンポーネント以外が必要な場合は、カスタムフィールドグループを使用して、他の[!UICONTROL B2B Source]フィールドをスキーマに追加することもできます。

>[!NOTE]
>
>現在、ソーススキーマから宛先スキーマへは、多対1の関係のみを定義できます。 1対多の関係の場合、「多」を表すスキーマ内に関係フィールドを定義する必要があります。

関係フィールドを設定するには、キャンバス内の該当するフィールドの横にある矢印アイコン（![矢印アイコン](../images/tutorials/relationship-b2b/arrow.png)）を選択します。 [!DNL Opportunities]スキーマの場合、これは`accountKey.sourceKey`フィールドです。これは、目標はアカウントとの多対1の関係を確立することなのです。

![関係ボタン](../images/tutorials/relationship-b2b/relationship-button.png)

関係の詳細を指定できるダイアログが表示されます。 関係タイプは自動的に&#x200B;**[!UICONTROL Many-to-one]**&#x200B;に設定されます。

![関係ダイアログ](../images/tutorials/relationship-b2b/relationship-dialog.png)

**[!UICONTROL 参照スキーマ]**&#x200B;の下で、検索バーを使用して宛先スキーマの名前を検索します。 宛先スキーマの名前をハイライト表示すると、「**[!UICONTROL 参照ID名前空間]**」フィールドが、スキーマのプライマリIDの名前空間に対して自動的に更新されます。

![参照スキーマ](../images/tutorials/relationship-b2b/reference-schema.png)

「**[!UICONTROL 現在のスキーマからの関係名]**」と「**[!UICONTROL 参照スキーマからの関係名]**」の下で、ソーススキーマと宛先スキーマのコンテキストで、それぞれ関係のわかりやすい名前を指定します。 終了したら、「**[!UICONTROL 保存]**」を選択して変更を適用し、スキーマを保存します。

![関係名](../images/tutorials/relationship-b2b/relationship-name.png)

キャンバスが再び表示され、関係フィールドに、前に指定したわかりやすい名前がマークされます。 また、参照しやすいように、左側のパネルの下に関係名も表示されます。

![関係の適用](../images/tutorials/relationship-b2b/relationship-applied.png)

宛先スキーマの構造を表示している場合、関係マーカーは、スキーマのプライマリIDフィールドの横と左側のレールに表示されます。

![宛先スキーマ関係マーカー](../images/tutorials/relationship-b2b/destination-relationship.png)

## 次の手順

このチュートリアルでは、[!DNL Schema Editor]を使用して2つのスキーマ間に多対1の関係を作成しました。 これらのスキーマに基づくデータセットを使用してデータを取り込み、そのデータをプロファイルデータストアでアクティブ化したら、両方のスキーマの属性を使用して複数クラスのセグメント化を使用できます。 詳しくは、 Real-time CDP B2B Editionのドキュメントを参照してください。
