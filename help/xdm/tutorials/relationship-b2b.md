---
title: Real-time Customer Data Platform B2B Edition での 2 つのスキーマ間の関係の定義
description: Adobe Real-time Customer Data Platform B2B Edition で 2 つのスキーマ間に多対 1 の関係を定義する方法を説明します。
exl-id: 14032754-c7f5-46b6-90e6-c6e99af1efba
source-git-commit: 14e3eff3ea2469023823a35ee1112568f5b5f4f7
workflow-type: tm+mt
source-wordcount: '1403'
ht-degree: 5%

---

# Real-time Customer Data Platform B2B Edition で 2 つのスキーマ間に多対 1 の関係を定義する {#relationship-b2b}

>[!CONTEXTUALHELP]
>id="platform_xdm_b2b_reference_schema"
>title="参照スキーマ"
>abstract="関係を確立するスキーマを選択します。 スキーマのクラスに応じて、B2B コンテキスト内の他のエンティティとの既存の関係も存在する場合があります。 B2B スキーマクラスが相互にどのように関係しているかについては、ドキュメントを参照してください。"

Adobe Real-time Customer Data Platform B2B Edition は、を含む、基本的な B2B データエンティティをキャプチャする、複数の Experience Data Model(XDM) クラスを提供します [アカウント](../classes/b2b/business-account.md), [商談](../classes/b2b/business-opportunity.md), [campaigns](../classes/b2b/business-campaign.md)など。 これらのクラスに基づいてスキーマを構築し、で使用できるようにする。 [リアルタイム顧客プロファイル](../../profile/home.md)を使用すると、異なるソースのデータを、和集合スキーマと呼ばれる統合表現に結合できます。

ただし、和集合スキーマには、同じクラスを共有するスキーマによってキャプチャされるフィールドのみを含めることができます。 スキーマの関係はここで生じます。 B2B スキーマに関係を実装することで、これらのビジネスエンティティが相互にどのように関係しているかを説明し、ダウンストリームセグメント化の使用例で複数のクラスの属性を含めることができます。

次の図は、基本的な実装で様々な B2B クラスを相互に関連付ける方法の例を示しています。

![B2B クラスの関係](../images/tutorials/relationship-b2b/classes.png)

このチュートリアルでは、Real-Time CDP B2B Edition で 2 つのスキーマ間に多対 1 の関係を定義する手順を説明します。

>[!NOTE]
>
>Real-time Customer Data Platform B2B Edition を使用していない場合や、1 対 1 の関係を作成する場合は、 [1 対 1 の関係の作成](./relationship-ui.md) 代わりに、
>
>このチュートリアルでは、Platform UI で B2B スキーマ間の関係を手動で確立する方法に焦点を当てます。 B2B ソース接続からデータを取り込む場合は、自動生成ユーティリティを使用して、必要なスキーマ、ID、関係を代わりに作成できます。 詳しくは、B2B 名前空間とスキーマに関するソースのドキュメントを参照してください。 [自動生成ユーティリティの使用](../../sources/connectors/adobe-applications/marketo/marketo-namespaces.md).

## はじめに

このチュートリアルでは、 [!DNL XDM System] と、 [!DNL Experience Platform] UI このチュートリアルを始める前に、次のドキュメントを確認してください。

* [XDM システムのExperience Platform](../home.md):XDM と、 [!DNL Experience Platform].
* [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [を使用してスキーマを作成する [!DNL Schema Editor]](create-schema-ui.md):UI でスキーマを構築および編集する方法の基本を説明するチュートリアルです。

## ソースと宛先のスキーマの定義

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。このチュートリアルでは、デモ目的で、ビジネスオポチュニティ ([!DNL Opportunities]「 」スキーマ ) および関連するビジネスアカウント（「 」で定義）[!DNL Accounts]&quot;スキーマ ) です。

スキーマの関係は、 **ソーススキーマ** が **宛先スキーマ**. 次の手順で、「[!DNL Opportunities]」はソーススキーマとして機能し、「[!DNL Accounts]「 」は宛先スキーマとして機能します。

### B2B の関係で ID を理解する

>[!CONTEXTUALHELP]
>id="platform_xdm_b2b_identity_namespace"
>title="参照 ID 名前空間"
>abstract="参照スキーマのプライマリ ID フィールドの名前空間（タイプ）。 関係に参加するには、参照スキーマに確立されたプライマリ ID フィールドが必要です。 B2B の関係で ID について詳しくは、ドキュメントを参照してください。"

関係を確立するには、宛先スキーマに定義済みのプライマリ ID が必要です。 B2B エンティティのプライマリ ID を設定する場合、文字列ベースのエンティティ ID が異なるシステムや場所で収集されると、Platform 内のデータの競合を引き起こす可能性があるので、重複する可能性があることに注意してください。

これを考慮するために、すべての標準 B2B クラスには、 [[!UICONTROL B2B ソース] データタイプ](../data-types/b2b-source.md). このデータ型は、B2B エンティティの文字列識別子のフィールドと、識別子のソースに関するその他のコンテキスト情報を提供します。 この一つのフィールドは `sourceKey`がデータ型の他のフィールドの値を連結して、エンティティの完全に一意の識別子を生成します。 このフィールドは、常に B2B エンティティスキーマのプライマリ ID として使用する必要があります。

![sourceKey フィールド](../images/tutorials/relationship-b2b/sourcekey.png)

>[!NOTE]
>
>条件 [ID としての XDM フィールドの設定](../ui/fields/identity.md)に値を入力する場合は、id を定義する id 名前空間を指定する必要があります。 これは、Adobeが提供する標準の名前空間か、組織が定義したカスタム名前空間にすることができます。 実際には、名前空間は単にコンテキスト文字列で、組織が ID タイプを分類するうえで意味を持つ場合に、好きな値に設定できます。 概要については、 [ID 名前空間](../../identity-service/namespaces.md) を参照してください。

以下の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。スキーマ構造内でプライマリ ID が定義された場所と、それらが使用するカスタム名前空間に注意してください。

### [!DNL Opportunities] schema

ソーススキーマ「 」[!DNL Opportunities]」が [!UICONTROL XDM ビジネスオポチュニティ] クラス。 クラスが提供するフィールドの 1 つ。 `opportunityKey`は、スキーマの識別子として機能します。 特に、 `sourceKey` 下のフィールド `opportunityKey` オブジェクトは、 [!DNL B2B Opportunity].

以下に示すように **[!UICONTROL スキーマのプロパティ]**&#x200B;の場合、このスキーマはでの使用に対して有効になっています [!DNL Real-time Customer Profile].

![商談スキーマ](../images/tutorials/relationship-b2b/opportunities.png)

### [!DNL Accounts] スキーマ

宛先スキーマ「 」[!DNL Accounts]」が [!UICONTROL XDM アカウント] クラス。 ルートレベル `accountKey` フィールドに `sourceKey` と呼ばれるカスタム名前空間の下でプライマリ ID として機能する [!DNL B2B Account]. このスキーマは、プロファイルでも使用できるようになっています。

![アカウントスキーマ](../images/tutorials/relationship-b2b/accounts.png)

## ソーススキーマでの関係フィールドの定義 {#relationship-field}

>[!CONTEXTUALHELP]
>id="platform_xdm_b2b_relationship_name_current"
>title="現在のスキーマからの関係名"
>abstract="現在のスキーマから参照スキーマへの関係を示すラベル（「関連アカウント」など）。 このラベルは、関連する B2B エンティティからのデータにコンテキストを与えるために、プロファイルとセグメント化で使用されます。 B2B スキーマの関係の構築について詳しくは、ドキュメントを参照してください。"

>[!CONTEXTUALHELP]
>id="platform_xdm_b2b_relationship_name_reference"
>title="参照スキーマからの関係名"
>abstract="参照スキーマから現在のスキーマへの関係（「関連オポチュニティ」など）を示すラベル。 このラベルは、関連する B2B エンティティからのデータにコンテキストを与えるために、プロファイルとセグメント化で使用されます。 B2B スキーマの関係の構築について詳しくは、ドキュメントを参照してください。"

2 つのスキーマ間の関係を定義するには、ソーススキーマに、宛先スキーマのプライマリ ID を参照する専用のフィールドが必要です。 標準 B2B クラスには、一般的に関連するビジネスエンティティ用の専用のソースキーフィールドが含まれています。 例えば、 [!UICONTROL XDM ビジネスオポチュニティ] クラスには、関連するアカウントのソースキーフィールドが含まれます (`accountKey`) および関連するキャンペーン (`campaignKey`) をクリックします。 ただし、他の [!UICONTROL B2B ソース] デフォルトのコンポーネント以外が必要な場合は、カスタムフィールドグループを使用してスキーマにフィールドを追加します。

>[!NOTE]
>
>現在、ソーススキーマから宛先スキーマへの多対 1 および 1 対 1 の関係のみを定義できます。 1 対多の関係の場合、「多数」を表すスキーマで関係フィールドを定義する必要があります。

関係フィールドを設定するには、矢印アイコン (![矢印アイコン](../images/tutorials/relationship-b2b/arrow.png)) をクリックします。 の場合、 [!DNL Opportunities] スキーマ、これは `accountKey.sourceKey` フィールドを変更する必要があります。

![関係ボタン](../images/tutorials/relationship-b2b/relationship-button.png)

関係の詳細を指定できるダイアログが表示されます。 関係タイプは自動的ににに設定されます。 **[!UICONTROL 多対 1]**.

![関係ダイアログ](../images/tutorials/relationship-b2b/relationship-dialog.png)

の下 **[!UICONTROL 参照スキーマ]**&#x200B;を検索する場合は、検索バーを使用して宛先スキーマの名前を検索します。 宛先スキーマの名前をハイライトすると、 **[!UICONTROL 参照 ID 名前空間]** フィールドは、スキーマのプライマリ id の名前空間に対して自動的に更新されます。

![参照スキーマ](../images/tutorials/relationship-b2b/reference-schema.png)

の下 **[!UICONTROL 現在のスキーマからの関係名]** および **[!UICONTROL 参照スキーマからの関係名]**&#x200B;では、それぞれ、ソーススキーマと宛先スキーマのコンテキストで、関係のわかりやすい名前を指定します。 終了したら、「 」を選択します。 **[!UICONTROL 保存]** 変更を適用し、スキーマを保存します。

![関係名](../images/tutorials/relationship-b2b/relationship-name.png)

キャンバスが再び表示され、関係フィールドが、前に指定したわかりやすい名前でマークされます。 関係名は、参照しやすくするために左側のパネルの下にも表示されます。

![適用された関係](../images/tutorials/relationship-b2b/relationship-applied.png)

宛先スキーマの構造を表示している場合、関係マーカーは、スキーマのプライマリ ID フィールドの横の、左側のパネルに表示されます。

![宛先スキーマ関係マーカー](../images/tutorials/relationship-b2b/destination-relationship.png)

## 次の手順

このチュートリアルでは、 [!DNL Schema Editor]. これらのスキーマに基づくデータセットを使用してデータを取り込み、そのデータをプロファイルデータストアでアクティブ化したら、両方のスキーマの属性を複数クラスのセグメント化の使用例で使用できます。 詳しくは、 Real-Time CDP B2B Edition のドキュメントを参照してください。
