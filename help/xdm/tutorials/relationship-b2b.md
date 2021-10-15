---
title: Real-time Customer Data Platform B2B Edition での 2 つのスキーマ間の関係の定義
description: Real-time Customer Data Platform B2B Edition で 2 つのスキーマ間の多対 1 の関係を定義する方法を説明します。
exl-id: 14032754-c7f5-46b6-90e6-c6e99af1efba
source-git-commit: 2ad20a4c7a9d1cc71fc4e589de90d7eabf8c87b7
workflow-type: tm+mt
source-wordcount: '1221'
ht-degree: 7%

---

# Real-time Customer Data Platform B2B Edition（ベータ版）で 2 つのスキーマ間の関係を定義する

>[!IMPORTANT]
>
>Real-time Customer Data Platform B2B Edition は現在ベータ版です。 ドキュメントと機能は変更される場合があります。

>[!NOTE]
>
>Real-time Customer Data Platform B2B Edition を使用していない場合は、[B2B 以外の関係の作成 ](./relationship-ui.md) に関するガイドを参照してください。

Real-time Customer Data Platform B2B Edition は、基本的な B2B データエンティティ（[ アカウント ](../classes/b2b/business-account.md)、[ オポチュニティ ](../classes/b2b/business-opportunity.md)、[ キャンペーン ](../classes/b2b/business-campaign.md) など）をキャプチャする、複数のエクスペリエンスデータモデル (XDM) クラスを提供します。 これらのクラスに基づいてスキーマを作成し、[ リアルタイム顧客プロファイル ](../../profile/home.md) で使用できるようにすることで、異なるソースのデータを和集合スキーマと呼ばれる統合表現に結合できます。

ただし、和集合スキーマには、同じクラスを共有するスキーマによってキャプチャされたフィールドのみを含めることができます。 スキーマの関係はここで生じます。 B2B スキーマに関係を実装すると、これらのビジネスエンティティが相互にどのように関係するかを説明し、ダウンストリームセグメント化の使用例で複数のクラスの属性を含めることができます。

次の図は、基本的な実装で様々な B2B クラスを相互に関連付ける方法の例を示しています。

![B2B クラスの関係](../images/tutorials/relationship-b2b/classes.png)

このチュートリアルでは、リアルタイム CDP B2B エディションで 2 つのスキーマ間の多対 1 の関係を定義する手順を説明します。

>[!NOTE]
>
>このチュートリアルでは、Platform UI で B2B スキーマ間の関係を手動で確立する方法に焦点を当てます。 B2B ソース接続からデータを取り込む場合は、自動生成ユーティリティを使用して、必要なスキーマ、ID、関係を代わりに作成できます。 [ 自動生成ユーティリティ ](../../sources/connectors/adobe-applications/marketo/marketo-namespaces.md) の使用について詳しくは、B2B の名前空間とスキーマに関するソースのドキュメントを参照してください。

## はじめに

このチュートリアルでは、[!DNL XDM System] と [!DNL Experience Platform] UI のスキーマエディターに関する十分な知識が必要です。 このチュートリアルを始める前に、次のドキュメントを確認してください。

* [XDM システムのExperience Platform](../home.md):XDM と、での実装の概要で [!DNL Experience Platform]す。
* [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [ [!DNL Schema Editor]](create-schema-ui.md)を使用したスキーマの作成：UI でのスキーマの作成および編集方法の基本について説明するチュートリアルです。

## ソースと宛先のスキーマの定義

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。このチュートリアルでは、デモ目的で、ビジネスオポチュニティ（「[!DNL Opportunities]」スキーマで定義）と、関連するビジネスアカウント（「[!DNL Accounts]」スキーマで定義）との間に関係を作成します。

スキーマの関係は、**宛先スキーマ** のプライマリ ID フィールドを参照する **ソーススキーマ** 内の専用のフィールドで表されます。 次の手順では、「[!DNL Opportunities]」がソーススキーマとして機能し、「[!DNL Accounts]」が宛先スキーマとして機能します。

### B2B 関係の ID について

関係を確立するには、両方のスキーマでプライマリ ID が定義され、[!DNL Real-time Customer Profile] に対して有効になっている必要があります。 B2B エンティティのプライマリ ID を設定する場合、文字列ベースのエンティティ ID が異なるシステムや場所で収集されると重複する可能性があり、Platform でデータの競合が発生する可能性があることに注意してください。

これを考慮するために、すべての標準 B2B クラスには、[[!UICONTROL B2B Source] データ型 ](../data-types/b2b-source.md) に準拠する「key」フィールドが含まれています。 このデータ型は、B2B エンティティの文字列識別子のフィールドと、識別子のソースに関するその他のコンテキスト情報を提供します。 これらのフィールドの 1 つ `sourceKey` は、データ型の他のフィールドの値を連結して、エンティティの完全な一意の識別子を生成します。 このフィールドは、常に B2B エンティティスキーマのプライマリ ID として使用する必要があります。

![sourceKey フィールド](../images/tutorials/relationship-b2b/sourcekey.png)

>[!NOTE]
>
>[XDM フィールドを ID](../ui/fields/identity.md) として設定する場合、ID を定義する ID 名前空間を指定する必要があります。 これは、Adobeが提供する標準名前空間、または組織が定義したカスタム名前空間です。 実際には、名前空間は単にコンテキスト文字列で、組織が ID タイプを分類するうえで意味を持つ場合に、好きな値に設定できます。 詳しくは、[ID 名前空間 ](../../identity-service/namespaces.md) の概要を参照してください。

以下の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。プライマリ ID がスキーマ構造内で定義された場所と、それらが使用するカスタム名前空間に注意します。

### [!DNL Opportunities] schema

ソーススキーマ「[!DNL Opportunities]」は、[!UICONTROL XDM Business Opportunity] クラスに基づいています。 クラスで提供されるフィールドの 1 つ `opportunityKey` は、スキーマの識別子として機能します。 特に、`opportunityKey` オブジェクトの下の `sourceKey` フィールドは、[!DNL B2B Opportunity] という名前のカスタム名前空間の下で、スキーマのプライマリ ID として設定されます。
**[!UICONTROL スキーマのプロパティ]** で見たように、このスキーマは [!DNL Real-time Customer Profile] で使用できるようになっています。

![オポチュニティスキーマ](../images/tutorials/relationship-b2b/opportunities.png)

### [!DNL Accounts] スキーマ

宛先スキーマ「[!DNL Accounts]」は、[!UICONTROL XDM アカウント ] クラスに基づいています。 ルートレベルの `accountKey` フィールドには、[!DNL B2B Account] という名前のカスタム名前空間の下でプライマリ ID として機能する `sourceKey` が含まれています。 このスキーマは、プロファイルでも使用できるようになっています。

![アカウントスキーマ](../images/tutorials/relationship-b2b/accounts.png)

## ソーススキーマでの関係フィールドの定義 {#relationship-field}

2 つのスキーマ間の関係を定義するには、ソーススキーマに、宛先スキーマのプライマリ ID を参照する専用のフィールドが必要です。 標準 B2B クラスには、一般的に関連するビジネスエンティティ用の専用のソースキーフィールドが含まれています。 例えば、[!UICONTROL XDM Business Opportunity] クラスには、関連するアカウント (`accountKey`) と関連するキャンペーン (`campaignKey`) のソースキーフィールドが含まれます。 ただし、デフォルトのコンポーネント以外が必要な場合は、カスタムフィールドグループを使用して、他の [!UICONTROL B2B Source] フィールドをスキーマに追加することもできます。

>[!NOTE]
>
>現在、ソーススキーマから宛先スキーマへは、多対一の関係のみを定義できます。 1 対多の関係の場合、「多」を表すスキーマ内で関係フィールドを定義する必要があります。

関係フィールドを設定するには、キャンバス内の該当するフィールドの横にある矢印アイコン（![ 矢印アイコン ](../images/tutorials/relationship-b2b/arrow.png)）を選択します。 [!DNL Opportunities] スキーマの場合、これは `accountKey.sourceKey` フィールドです。これは、目標はアカウントとの多対 1 の関係を確立することです。

![関係ボタン](../images/tutorials/relationship-b2b/relationship-button.png)

関係の詳細を指定できるダイアログが表示されます。 関係タイプは自動的に **[!UICONTROL 多対 1]** に設定されます。

![関係ダイアログ](../images/tutorials/relationship-b2b/relationship-dialog.png)

**[!UICONTROL 参照スキーマ]** の下で、検索バーを使用して宛先スキーマの名前を検索します。 宛先スキーマの名前をハイライトすると、「**[!UICONTROL 参照 ID 名前空間]**」フィールドが、スキーマのプライマリ ID の名前空間に対して自動的に更新されます。

![参照スキーマ](../images/tutorials/relationship-b2b/reference-schema.png)

「**[!UICONTROL 現在のスキーマからの関係名]**」と「**[!UICONTROL 参照スキーマからの関係名]**」で、それぞれソーススキーマと宛先スキーマのコンテキストで、関係のわかりやすい名前を指定します。 終了したら、「**[!UICONTROL 保存]**」を選択して変更を適用し、スキーマを保存します。

![関係名](../images/tutorials/relationship-b2b/relationship-name.png)

キャンバスが再び表示され、関係フィールドに前に指定したわかりやすい名前が付けられます。 また、参照しやすいように、関係名も左側のパネルの下に表示されます。

![関係の適用](../images/tutorials/relationship-b2b/relationship-applied.png)

宛先スキーマの構造を表示している場合、関係マーカーは、スキーマのプライマリ ID フィールドの横と左側のパネルに表示されます。

![宛先スキーマ関係マーカー](../images/tutorials/relationship-b2b/destination-relationship.png)

## 次の手順

このチュートリアルでは、[!DNL Schema Editor] を使用して 2 つのスキーマ間に多対 1 の関係を作成しました。 これらのスキーマに基づくデータセットを使用してデータを取り込み、データをプロファイルデータストアでアクティブ化したら、両方のスキーマの属性を使用して複数クラスのセグメント化を使用できます。 詳しくは、「 Real-time CDP B2B Edition 」のドキュメントを参照してください。
