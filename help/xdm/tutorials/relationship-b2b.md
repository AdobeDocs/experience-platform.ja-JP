---
title: Real-Time Customer Data Platform B2B editionでの 2 つのスキーマ間の関係の定義
description: Adobe Real-Time Customer Data Platform B2B editionで 2 つのスキーマ間に多対 1 の関係を定義する方法を説明します。
exl-id: 14032754-c7f5-46b6-90e6-c6e99af1efba
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1726'
ht-degree: 14%

---

# Real-time Customer Data Platform B2B エディションでの 2 つのスキーマ間における多対 1 の関係を定義 {#relationship-b2b}

>[!CONTEXTUALHELP]
>id="platform_xdm_b2b_reference_schema"
>title="参照スキーマ"
>abstract="関係を確立するスキーマを選択します。スキーマのクラスによっては、B2B コンテキストの他のエンティティとの既存の関係を持つこともあります。B2B スキーマクラスがお互いにどのように関連しているかについて詳しくは、ドキュメントを参照してください。"

Adobe Systems Real-時間 Customer データ Platform B2B Edition には、[アカウント](../classes/b2b/business-account.md)、[商談](../classes/b2b/business-opportunity.md)[キャンペーン](../classes/b2b/business-campaign.md)など、基本的な B2B データエンティティをキャプチャするいくつかのエクスペリエンスデータモデル(XDM)クラスが用意されています。これらのクラスに基づいてスキーマを構築し、 [Real-時間 Customer プロフィール](../../profile/home.md) で使用できるようにすることで、異なるソースからのデータを和集合スキーマと呼ばれる統一された表現にマージできます。

ただし、和集合スキーマには、同じクラスを共有するスキーマによってキャプチャされたフィールドのみを含めることができます。 これがスキーマ関係の出番です。 B2B スキーマにリレーションシップを実装することで、これらのビジネス エンティティが相互にどのように関連しているかを記述し、ダウンストリーム セグメント化 ユース ケースに複数のクラスの属性を含めることができます。

次の図は、さまざまな B2B クラスが基本的な実装で相互に関連付ける方法の例を示しています。

![B2B クラスの関係 ](../images/tutorials/relationship-b2b/classes.png)

このチュートリアルでは、Real-Time CDP B2B editionで 2 つのスキーマ間に多対 1 の関係を定義する手順を説明します。

>[!NOTE]
>
>Real-Time Customer Data Platform B2B editionを使用していない場合や、1 対 1 の関係を作成する場合は、代わりに [1 対 1 の関係の作成 ](./relationship-ui.md) に関するガイドを参照してください。
>
>このチュートリアルでは、Experience Platform UI で B2B スキーマ間の関係を手動で確立する方法について重点的に説明します。 B2B ソース接続からデータを取り込む場合は、自動生成ユーティリティを使用して、必要なスキーマ、ID および関係を作成します。 [ 自動生成ユーティリティの使用 ](../../sources/connectors/adobe-applications/marketo/marketo-namespaces.md) について詳しくは、B2B 名前空間とスキーマに関するソースドキュメントを参照してください。

## はじめに

このチュートリアルでは、[!DNL XDM System] UI の [!DNL Experience Platform] とスキーマエディターに関する十分な知識が必要です。 このチュートリアルを始める前に、次のドキュメントを確認してください。

* [Experience Platformの XDM システム ](../home.md):XDM と [!DNL Experience Platform] での実装の概要です。
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

リレーションシップを確立するには、参照スキーマに定義済みのプライマリ ID が必要です。 B2Bエンティティのプライマリ ID を設定する場合、文字列ベースのエンティティ ID は、異なるシステムまたは場所にまたがって収集する場合に重複する可能性があり、Experience Platform でのデータの競合リード可能性があることに注意してください。

これアカウントために、すべての標準B2Bクラスには、 [[!UICONTROL B2B Source] データ型](../data-types/b2b-source.md)に準拠する「キー」フィールドが含まれています。 このデータ型は、B2B エンティティの文字列識別子のフィールドと、識別子のソースに関するその他のコンテキスト情報を提供します。 `sourceKey`、これらのフィールドの 1 つは、データ型内の他のフィールドの値を連結して、エンティティの完全に一意の識別子を生成します。このフィールドは、B2B エンティティスキーマのプライマリ ID として常に使用する必要があります。

![sourceKey フィールド ](../images/tutorials/relationship-b2b/sourcekey.png)

>[!NOTE]
>
>[XDM フィールドを ID として設定 ](../ui/fields/identity.md) する場合、の下で ID を定義する ID 名前空間を指定する必要があります。 これは、Adobeが提供する標準の名前空間、または組織が定義するカスタム名前空間にできます。 実際には、名前空間は単なるコンテキスト文字列であり、ID タイプを分類する組織にとって意味がある場合は、任意の値に設定できます。 詳しくは、[ID 名前空間 ](../../identity-service/features/namespaces.md) の概要を参照してください。

参照目的で、次の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。 プライマリ ID が定義されているスキーマ構造と、それらが使用するカスタム名前空間に注意してください。

### 商談スキーマ

ソーススキーマ「[!DNL Opportunities]」は、[!UICONTROL XDM Business Opportunity] クラスに基づいています。 クラス `opportunityKey` が提供するフィールドの 1 つは、スキーマの識別子として機能します。 特に、`sourceKey` オブジェクトの `opportunityKey` フィールドは、[!DNL B2B Opportunity] というカスタム名前空間でスキーマのプライマリ ID として設定されます。

**[!UICONTROL Field Properties]** に示すように、このスキーマは [!DNL Real-Time Customer Profile] での使用が有効になっています。

![opportunityKey オブジェクトと「プロファイルを有効にする」切替スイッチがハイライト表示された、スキーマエディターの商談スキーマ ](../images/tutorials/relationship-b2b/opportunities.png)

### [!DNL Accounts] スキーマ

参照スキーマ「[!DNL Accounts]」は、[!UICONTROL XDM Account] クラスに基づいています。 ルートレベルの `accountKey` フィールドには、`sourceKey` と呼ばれるカスタム名前空間の下でプライマリ ID として機能する [!DNL B2B Account] が含まれています。 このスキーマは、プロファイルでも使用できるようになりました。

![accountKey オブジェクトと「プロファイルで有効にする」切替スイッチがハイライト表示されたスキーマエディターのアカウントスキーマ ](../images/tutorials/relationship-b2b/accounts.png)

## ソーススキーマで関係フィールドを定義 {#relationship-field}

>[!CONTEXTUALHELP]
>id="platform_xdm_b2b_relationship_name_current"
>title="現在のスキーマからの関係名"
>abstract="現在のスキーマから参照スキーマへの関係を説明するラベル (例：「関連するアカウント」)。このラベルは、関連する B2B エンティティからのデータにコンテキストを付与するために、プロファイルとセグメント化で使用されます。B2B スキーマの関係の構築について詳しくは、ドキュメントを参照してください。"

>[!CONTEXTUALHELP]
>id="platform_xdm_b2b_relationship_name_reference"
>title="参照スキーマからの関係名"
>abstract="参照スキーマから現在のスキーマへの関係を説明するラベル (例：「関連するオポチュニティ」)。このラベルは、関連する B2B エンティティからのデータにコンテキストを付与するために、プロファイルとセグメント化で使用されます。B2B スキーマの関係の構築について詳しくは、ドキュメントを参照してください。"

2 つのスキーマ間の関係を定義するには、ソーススキーマに、参照スキーマのプライマリ ID を示す専用のフィールドが必要です。 標準 B2B クラスには、一般的に関連するビジネスエンティティ用の専用のソースキーフィールドが含まれています。 例えば、[!UICONTROL XDM Business Opportunity] クラスには、関連するアカウント（`accountKey`）と関連するキャンペーン（`campaignKey`）のソースキーフィールドが含まれています。 ただし、デフォルトの [!UICONTROL B2B Source] ンポーネント以外が必要な場合は、カスタムフィールドグループを使用して、スキーマに他のカスタムフィールドを追加することもできます。

>[!NOTE]
>
>現在、ソーススキーマから参照スキーマに定義できるのは、多対 1 および 1 対 1 の関係のみです。 1 対多の関係の場合、「多」を表すスキーマで関係フィールドを定義する必要があります。

関係フィールドを設定するには、キャンバス内で該当するフィールドを選択し、**[!UICONTROL Add relationship]** のサイドバーで [!UICONTROL Schema properties] を選択します。 [!DNL Opportunities] スキーマの場合、目標はアカウントとの多対 1 の関係を確立することなので、これは `accountKey.sourceKey` のフィールドになります。

![sourceKey フィールドと「関係を追加」がハイライト表示されたスキーマエディター ](../images/tutorials/relationship-b2b/add-relationship.png)

[!UICONTROL Add relationship] ダイアログが表示されます。 このダイアログを使用して、関係の詳細を指定します。 関係タイプは、デフォルトで **[!UICONTROL Many-to-one]** に設定されます。

![ 多対 1 のスキーマ関係がハイライト表示された関係を追加ダイアログ。](../images/tutorials/relationship-b2b/relationship-dialog.png)

**[!UICONTROL Reference Schema]** の下で、検索バーまたはドロップダウンメニューを使用して、参照スキーマの名前を探します。 参照スキーマの名前をハイライト表示すると、**[!UICONTROL Reference Identity Namespace]** フィールドが参照スキーマのプライマリ ID の名前空間に自動的に更新されます。

>[!NOTE]
>
>使用可能な参照スキーマのリストは、適切なスキーマのみを含むようにフィルターされます。 スキーマは、プライマリ ID が割り当てられており、B2B クラスまたは個別 プロフィール クラスのいずれかである必要があります&#x200B;****。プロスペクトクラススキーマはリレーションを持つことができません。

![[参照スキーマ] フィールドと [参照IDサービス 名前空間 フィールドが強調表示された [リレーションシップ追加] ダイアログ。](../images/tutorials/relationship-b2b/reference-schema.png)

[ **[!UICONTROL Relationship Name From Current Schema]** ] と [ **[!UICONTROL Relationship Name From Reference Schema]**] で、ソース スキーマと参照スキーマのコンテキストでリレーションシップのフレンドリ名をそれぞれ指定します。 完了したら、 [ **[!UICONTROL Apply]** ] を選択して変更を確認し、リレーションシップを保存します。

>[!NOTE]
>
>交際名は 35 文字以内にする必要があります。

![ 関係名フィールドがハイライト表示された関係を追加ダイアログ。](../images/tutorials/relationship-b2b/relationship-name.png)

キャンバスが再び表示され、関係フィールドが、以前に指定したわかりやすい名前でマークされます。 関係名も左側のパネルに表示され、簡単に参照できます。

![ 新しい関係名が適用されたスキーマエディター。](../images/tutorials/relationship-b2b/relationship-applied.png)

参照スキーマの構造を表示すると、スキーマのプライマリ ID フィールドの横にある左側のパネルに関係マーカーが表示されます。

![ スキーマエディターの宛先スキーマで、新しい関係マーカーがハイライト表示されています。](../images/tutorials/relationship-b2b/destination-relationship.png)

## B2B スキーマ関係の編集 {#edit-schema-relationship}

スキーマの関係が確立されたら、ソーススキーマの関係フィールドを選択してから、**[!UICONTROL Edit relationship]** を選択します。

>[!NOTE]
>
>関連するすべての関係を表示するには、参照スキーマのプライマリ ID フィールドを選択してから [!UICONTROL View relationships] を選択します。
>![関係フィールドが選択され、関係を表示がハイライト表示されたスキーマエディター。](../images/tutorials/relationship-b2b/view-relationships.png " 関係フィールドが選択され、関係を表示がハイライト表示されたスキーマエディター。"){width="100" zoomable="yes"}

![ 関係フィールドと「関係を編集」がハイライト表示されたスキーマエディター ](../images/tutorials/relationship-b2b/edit-b2b-relationship.png)

[!UICONTROL Edit relationship] ダイアログが表示されます。 このダイアログから、参照スキーマと関係名を変更したり、関係を削除したりできます。 多対 1 関係タイプは変更できません。

![ 関係を編集ダイアログ ](../images/tutorials/relationship-b2b/edit-b2b-relationship-dialog.png)

データの整合性を維持し、セグメント化やその他のプロセスの中断を避けるために、リンクされたデータセットとのスキーマ関係を管理する際に、次のガイドラインを考慮してください。

* スキーマがデータセットに関連付けられている場合は、セグメント化に悪影響を及ぼす可能性があるので、関係を直接削除しないでください。 代わりに、関係を削除する前に、関連するデータセットを削除します。
* 参照スキーマを変更するには、まず既存のリレーションシップを削除する必要があります。 ただし、関連付けられたデータセットとの関係を削除すると、意図しない結果が生じる可能性があるため、これは注意して行う必要があります。
* 既存のリンクされたデータセットを持つスキーマに新しいリレーションシップを追加すると、意図したとおりに機能せず、潜在的な競合リード可能性があります。

## リレーションシップのフィルターと検索 {#filter-and-search}

スキーマ内の特定のリレーションシップのフィルター処理と検索は、[!UICONTROL Relationships] ワークスペースの[!UICONTROL Schemas] タブから取得できます。この表示を使用すると、リレーションシップをすばやく見つけて管理できます。 フィルターオプションの詳細については、 [スキーマリソースの探索](../ui/explore.md#lookup) ドキュメントを参照してください。

![ スキーマ ワークスペースの「関係」タブ ](../images/tutorials/relationship-b2b/relationship-tab.png)

## 次の手順

このチュートリアルでは、[!DNL Schema Editor] を使用して 2 つのスキーマ間に多対 1 の関係を正常に作成しました。 これらのスキーマに基づくデータセットを使用してデータが取り込まれ、そのデータがプロファイルデータストアでアクティブ化されると、[ 複数クラスのセグメント化のユースケース ](../../rtcdp/segmentation/b2b.md) に対して両方のスキーマの属性を使用できます。
