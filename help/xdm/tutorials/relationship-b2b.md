---
title: Real-time Customer Data Platform B2B Edition での 2 つのスキーマ間の関係の定義
description: Adobe Real-time Customer Data Platform B2B Edition で 2 つのスキーマ間に多対 1 の関係を定義する方法を説明します。
exl-id: 14032754-c7f5-46b6-90e6-c6e99af1efba
source-git-commit: ba39f62cd77acedb7bfc0081dbb5f59906c9b287
workflow-type: tm+mt
source-wordcount: '1363'
ht-degree: 18%

---

# Real-time Customer Data Platform B2B エディションでの 2 つのスキーマ間における多対 1 の関係を定義 {#relationship-b2b}

>[!CONTEXTUALHELP]
>id="platform_xdm_b2b_reference_schema"
>title="参照スキーマ"
>abstract="関係を確立するスキーマを選択します。スキーマのクラスによっては、B2B コンテキストの他のエンティティとの既存の関係を持つこともあります。B2B スキーマクラスがお互いにどのように関連しているかについて詳しくは、ドキュメントを参照してください。"

Adobe Real-time Customer Data Platform B2B Edition には、[ アカウント ](../classes/b2b/business-account.md)、[ 商談 ](../classes/b2b/business-opportunity.md)、[ キャンペーン ](../classes/b2b/business-campaign.md) など、基本的な B2B データエンティティをキャプチャするエクスペリエンスデータモデル （XDM） クラスがいくつか用意されています。 これらのクラスに基づいてスキーマを構築し、それらを [ リアルタイム顧客プロファイル ](../../profile/home.md) で使用できるようにすると、異なるソースのデータを結合スキーマと呼ばれる統合表現に統合できます。

ただし、和集合スキーマには、同じクラスを共有するスキーマによってキャプチャされたフィールドのみを含めることができます。 スキーマの関係は、ここに格納されます。 B2B スキーマで関係を実装することで、これらのビジネスエンティティの相互関係を記述し、ダウンストリームセグメント化のユースケースに複数のクラスの属性を含めることができます。

次の図は、基本的な実装において、様々な B2B クラスがどのように相互に関連付けられるかを示す例を示しています。

![B2B クラスの関係 ](../images/tutorials/relationship-b2b/classes.png)

このチュートリアルでは、Real-Time CDP B2B Edition の 2 つのスキーマ間に多対 1 の関係を定義する手順を説明します。

>[!NOTE]
>
>Real-time Customer Data Platform B2B Edition を使用していない場合や、1 対 1 の関係を作成する場合は、代わりに [1 対 1 の関係の作成 ](./relationship-ui.md) に関するガイドを参照してください。
>
>このチュートリアルでは、Platform UI で B2B スキーマ間の関係を手動で確立する方法に重点を置いています。 B2B ソース接続からデータを取り込む場合は、自動生成ユーティリティを使用して、必要なスキーマ、ID および関係を作成します。 [ 自動生成ユーティリティの使用 ](../../sources/connectors/adobe-applications/marketo/marketo-namespaces.md) について詳しくは、B2B 名前空間とスキーマに関するソースドキュメントを参照してください。

## はじめに

このチュートリアルでは、[!DNL Experience Platform] UI の [!DNL XDM System] とスキーマエディターに関する十分な知識が必要です。 このチュートリアルを始める前に、次のドキュメントを確認してください。

* [Experience Platformにおける XDM システム ](../home.md):XDM と [!DNL Experience Platform] での実装の概要です。
* [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [ 次を使用してスキーマを作成  [!DNL Schema Editor]](create-schema-ui.md):UI でスキーマを作成および編集する方法の基本を説明するチュートリアル。

## ソースおよび参照スキーマの定義

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。デモ目的で、このチュートリアルは、（「[!DNL Opportunities]」スキーマで定義された）ビジネスオポチュニティと、関連する（「[!DNL Accounts]」スキーマで定義された）ビジネスアカウントとの関係を作成します。

スキーマ関係は、**参照スキーマ** のプライマリ ID フィールドを参照する **ソーススキーマ** 内の専用フィールドで表されます。 以降の手順では、「[!DNL Opportunities]」はソーススキーマとして機能し、「[!DNL Accounts]」は参照スキーマとして機能します。

### B2B の関係での ID について

>[!CONTEXTUALHELP]
>id="platform_xdm_b2b_identity_namespace"
>title="参照 ID 名前空間"
>abstract="参照スキーマのプライマリ ID フィールドの名前空間 (タイプ)。参照スキーマは、関係に参加するために、確立されたプライマリ ID フィールドが必要です。B2B 関係の ID について詳しくは、ドキュメントを参照してください。"

関係を確立するには、参照スキーマに定義済みのプライマリ ID が必要です。 B2B エンティティのプライマリ ID を設定する場合は、様々なシステムや場所にわたって文字列ベースのエンティティ ID を収集すると、重複する可能性があり、Platform でデータの競合が発生する可能性があることに注意してください。

これに対処するために、すべての標準 B2B クラスには、[[!UICONTROL B2B Source] データタイプ ](../data-types/b2b-source.md) に準拠する「キー」フィールドが含まれています。 このデータタイプは、B2B エンティティの文字列識別子のフィールドと、識別子のソースに関するその他のコンテキスト情報を提供します。 これらのフィールドの 1 つ（`sourceKey`）は、データタイプ内の他のフィールドの値を連結して、エンティティの完全に一意の ID を生成します。 このフィールドは、B2B エンティティスキーマのプライマリ ID として常に使用する必要があります。

![sourceKey フィールド ](../images/tutorials/relationship-b2b/sourcekey.png)

>[!NOTE]
>
>[XDM フィールドを ID として設定 ](../ui/fields/identity.md) する場合、の下で ID を定義する ID 名前空間を指定する必要があります。 これは、組織が提供する標準の名前空間、またはAdobeが定義するカスタム名前空間にできます。 実際には、名前空間は単なるコンテキスト文字列であり、ID タイプを分類する組織にとって意味がある場合は、任意の値に設定できます。 詳しくは、[ID 名前空間 ](../../identity-service/features/namespaces.md) の概要を参照してください。

参照目的で、次の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。 プライマリ ID が定義されているスキーマ構造と、それらが使用するカスタム名前空間に注意してください。

### [!DNL Opportunities] スキーマ

ソーススキーマ「[!DNL Opportunities]」は、[!UICONTROL XDM Business Opportunity] クラスに基づいています。 クラス `opportunityKey` が提供するフィールドの 1 つは、スキーマの識別子として機能します。 特に、`opportunityKey` オブジェクトの `sourceKey` フィールドは、[!DNL B2B Opportunity] というカスタム名前空間でスキーマのプライマリ ID として設定されます。

**[!UICONTROL スキーマプロパティ]** に示すように、このスキーマは [!DNL Real-Time Customer Profile] での使用が有効になっています。

![ 商談スキーマ ](../images/tutorials/relationship-b2b/opportunities.png)

### [!DNL Accounts] スキーマ

参照スキーマ「[!DNL Accounts]」は、[!UICONTROL XDM Account] クラスに基づいています。 ルートレベルの `accountKey` フィールドには、[!DNL B2B Account] と呼ばれるカスタム名前空間の下でプライマリ ID として機能する `sourceKey` が含まれています。 このスキーマは、プロファイルでも使用できるようになりました。

![ アカウントスキーマ ](../images/tutorials/relationship-b2b/accounts.png)

## ソーススキーマで関係フィールドを定義 {#relationship-field}

>[!CONTEXTUALHELP]
>id="platform_xdm_b2b_relationship_name_current"
>title="現在のスキーマからの関係名"
>abstract="現在のスキーマから参照スキーマへの関係を説明するラベル (例：「関連するアカウント」)。このラベルは、関連する B2B エンティティからのデータにコンテキストを付与するために、プロファイルとセグメント化で使用されます。B2B スキーマの関係の構築について詳しくは、ドキュメントを参照してください。"

>[!CONTEXTUALHELP]
>id="platform_xdm_b2b_relationship_name_reference"
>title="参照スキーマからの関係名"
>abstract="参照スキーマから現在のスキーマへの関係を説明するラベル (例：「関連するオポチュニティ」)。このラベルは、関連する B2B エンティティからのデータにコンテキストを付与するために、プロファイルとセグメント化で使用されます。B2B スキーマの関係の構築について詳しくは、ドキュメントを参照してください。"

2 つのスキーマ間の関係を定義するには、ソーススキーマに、参照スキーマのプライマリ ID を示す専用のフィールドが必要です。 標準 B2B クラスには、一般的に関連するビジネスエンティティ用の専用のソースキーフィールドが含まれています。 例えば、[!UICONTROL XDM Business Opportunity] クラスには、関連するアカウント（`accountKey`）と関連するキャンペーン（`campaignKey`）のソースキーフィールドが含まれています。 ただし、デフォルトのコンポーネント以上が必要な場合は、カスタムフィールドグループを使用して、他の [!UICONTROL B2B Source] フィールドをスキーマに追加することもできます。

>[!NOTE]
>
>現在、ソーススキーマから参照スキーマに定義できるのは、多対 1 および 1 対 1 の関係のみです。 1 対多の関係の場合、「多」を表すスキーマで関係フィールドを定義する必要があります。

関係フィールドを設定するには、キャンバス内で該当するフィールドの横にある矢印アイコン（![ 矢印アイコン ](../images/tutorials/relationship-b2b/arrow.png)）を選択します。 [!DNL Opportunities] スキーマの場合、目標はアカウントとの多対 1 の関係を確立することなので、これは `accountKey.sourceKey` のフィールドになります。

![ 関係ボタン ](../images/tutorials/relationship-b2b/relationship-button.png)

関係の詳細を指定できるダイアログが表示されます。 関係タイプは自動的に **[!UICONTROL 多対 1]** に設定されます。

![ 関係ダイアログ ](../images/tutorials/relationship-b2b/relationship-dialog.png)

**[!UICONTROL 参照スキーマ]** の下で、検索バーを使用して参照スキーマの名前を見つけます。 参照スキーマの名前をハイライト表示すると、「**[!UICONTROL 参照 ID 名前空間]** フィールドが、スキーマのプライマリ ID の名前空間に自動的に更新されます。

![ 参照スキーマ ](../images/tutorials/relationship-b2b/reference-schema.png)

**[!UICONTROL 現在のスキーマからの関係名]** および **[!UICONTROL 参照スキーマからの関係名]** で、それぞれソーススキーマおよび参照スキーマのコンテキストの関係にわかりやすい名前を指定します。 終了したら、「**[!UICONTROL 保存]**」を選択して変更を適用し、スキーマを保存します。

![ 関係名 ](../images/tutorials/relationship-b2b/relationship-name.png)

キャンバスが再び表示され、関係フィールドが、以前に指定したわかりやすい名前でマークされます。 関係名も左側のパネルの下に表示され、参照しやすくなっています。

![ 適用された関係 ](../images/tutorials/relationship-b2b/relationship-applied.png)

参照スキーマの構造を表示すると、スキーマのプライマリ ID フィールドの横にある左側のパネルに関係マーカーが表示されます。

![ 宛先スキーマの関係マーカー ](../images/tutorials/relationship-b2b/destination-relationship.png)

## 次の手順

このチュートリアルでは、[!DNL Schema Editor] を使用して 2 つのスキーマ間に多対 1 の関係を正常に作成しました。 これらのスキーマに基づくデータセットを使用してデータが取り込まれ、そのデータがプロファイルデータストアでアクティブ化されると、[ 複数クラスのセグメント化のユースケース ](../../rtcdp/segmentation/b2b.md) に対して両方のスキーマの属性を使用できます。
