---
title: Adobe Experience Platformの AI アシスタント
description: AI Assistant を使用してExperience PlatformとReal-time Customer Data Platformの概念をナビゲートおよび理解し、オブジェクトに関する使用情報を学びます。
badge: アルファ版
hide: true
hidefromtoc: true
exl-id: 8be1c222-3ccd-4a41-978e-33ac9b730f8c
source-git-commit: b1f2d85f5a1cf6bb38344c87496488a919800029
workflow-type: tm+mt
source-wordcount: '2604'
ht-degree: 1%

---

# Adobe Experience Platformの AI アシスタント

>[!NOTE]
>
> Adobe Experience Platformの AI アシスタントは現在Alpha中です。 機能とドキュメントは変更される場合があります。

AI Assistant は、Adobe Experience PlatformとReal-time Customer Data Platformの概念や、オブジェクトに関する使用情報をナビゲートおよび理解するために使用できる UI 機能です。

AI Assistant に次のような情報を問い合わせることができます。

* データとオーディエンスに関するタスクの実行方法に関するガイダンス。
* 組織内の既存のデータオブジェクトのステータスと指標。
* 使用例とニュアンスを参照して、属性、データフロー、データセット、宛先、スキーマ、セグメント、ソースを含むデータオブジェクトをより深く理解してください。

AI Assistant を使用してExperience PlatformとReal-Time CDPのワークフローをナビゲートおよび理解する方法については、以下のガイドをお読みください。

>[!BEGINSHADEBOX]

**AI アシスタントの仕組み**

AI Assistant は、データベースに問い合わせ、データベースのデータを人間が読み取れる回答に変換することで、送信された質問に対して応答します。

基礎となるデータのこの内部表現は、ナレッジグラフとも呼ばれます。ナレッジグラフは、特定の回答に関する概念、データ、メタデータの包括的な Web です。

ナレッジ・グラフは、クエリが送信されるたびに参照されるサブグラフで構成されます。

* 顧客使用状況データ。
* 様々なメタストアにわたる顧客使用状況データ。
* Experience Leagueドキュメント。

AI Assistant に問い合わせる前に考慮すべき質問は 2 つあります。

* **概念に関する質問**：概念に関する質問は、データやオーディエンスに関するAdobeの概念に関する質問です。 概念に関する質問の例を次に示します。
   * バッチとストリーミングのセグメント化の違いは何ですか？
   * 業界のデータモデルはありますか？また、それらをどのように使用すればよいですか？
   * Real-Time CDPは何に最も適していますか？
* **使用に関する質問**：使用に関する質問は、組織内のデータオブジェクトに関する質問です。 使用に関する質問の例を次に示します。
   * データセットはいくつありますか？
   * これまでに使用されたことのないスキーマ属性の数
   * アクティブ化されたセグメントはどれですか？

>[!ENDSHADEBOX]

## AI Assistant で達成できる目標

AI Assistant は、次のような目的に使用できます。

| 目的 | 説明 |
| --- | --- |
| Experience PlatformとReal-Time CDPの概念の学習 | AI Assistant の概念的な質問をして、Experience PlatformとReal-Time CDPに自分をオンボーディングできます。 また、AI アシスタントを使用して、慣れていないオブジェクトや行動を学ぶこともできます。 |
| サンドボックスでのデータの清潔さの確保 | AI Assistant を使用して、重複や未使用のオブジェクトを特定し、サンドボックスを効率的に清潔に保つことができます。 |
| 値の分析の統合 | AI Assistant を使用して、最も使用頻度の高いオブジェクトを特定し、パフォーマンス指標を評価したり、最も価値の高いデータを見つけたりできます。 |
| 影響分析の理解 | AI Assistant を使用して、特定のワークフローで使用されたオブジェクトを特定し、変更の影響を評価できます。 |
| データの監視 | AI Assistant を使用して、データフロー、取り込みまたは評価のジョブを監視し、進行状況に関する不一致やレポートを表示できます。 |

## Experience PlatformUI の AI アシスタントにアクセス

AI Assistant を起動するには、 **[!UICONTROL AI アシスタントアイコン]** をExperience PlatformUI の上部のヘッダーから。

![Experience Platformのホームページ（「AI アシスタント」アイコンが選択され、「AI アシスタント」インターフェイスが開いている状態）。](./images/ai-assistant/ai-assistant.png)

AI Assistant インターフェイスが表示され、すぐに使い始めるための情報が提供されます。 以下に示すオプションを使用できます。 [!UICONTROL 開始するアイデア] 次のような質問やコマンドに答えるには：

* [!UICONTROL アクティブ化されているセグメントはどれですか？]
* [!UICONTROL スキーマとは]
* [!UICONTROL Real-Time CDPの一般的な使用例]

![AI アシスタントの「始めるべきアイデア」の節。](./images/ai-assistant/ideas-to-get-started.png)

AI Assistant を操作するには、入力ボックスを使用してクエリやコマンドを入力します。 また、(**`+`**) 記号を使用してオートコンプリート機能とブックマークアイコンを使用し、ブックマークされたクエリやコマンドにアクセスできます。

![AI アシスタントの入力ボックスがハイライト表示されています。](./images/ai-assistant/interact.png)

## 使用例：AI Assistant を使用して、スキーマの作成プロセスを迅速に進めます。

>[!NOTE]
>
>次のワークフローは、エクスペリエンスイベントスキーマの作成プロセスを使用した例で、Experience PlatformUI を使用する際の AI Assistant の使用方法を示しています。

例えば、 **イベントスキーマでのデバイスの取引**. エクスペリエンスイベントスキーマの作成プロセスで、 `eventType` フィールドに入力します。 「この時点で、ワークフローを終了し、 [スキーマ構成の基本](../xdm/schema/composition.md) ドキュメントを参照するか、AI Assistant を使用して質問への回答を取得し、AI Assistant が推奨するドキュメントリンクを通じて追加のリソースを見つけることができます。」

まず、表示されるテキストボックスに質問を入力します。 次の例では、AI Assistant に次の質問が含まれています。**ExperienceEvent スキーマの eventType フィールドとは何ですか。**&quot;

![次の質問を含むExperience Platform用 AI Assistant です。「ExperienceEvent スキーマの eventType フィールドとは何ですか。](./images/ai-assistant/question.png)

AI アシスタントはナレッジベースに問い合わせ、回答を計算します。 しばらくすると、AI Assistant は回答と関連する提案を返します。これをフォローアッププロンプトとして使用できます。

![前のクエリへの回答を含むExperience Platform用 AI アシスタント。](./images/ai-assistant/answer.png)

AI アシスタントからの応答を受け取ったら、様々なオプションから選択して、続行する方法を決定できます。

### クエリを保存 {#save-your-query}

+++クエリの保存方法の例を表示するには、「 」を選択します

クエリを保存するには、質問の横にあるブックマークアイコンを選択します。

![選択したブックマークのスクリーンショット。](./images/ai-assistant/save-your-query.png)

保存したクエリにアクセスするには、入力ボックスの下のブックマークアイコンを選択し、実行するクエリを選択します。

![ブックマークアイコンのスクリーンショットと保存済みクエリのリスト。](./images/ai-assistant/bookmarks.png)

+++

### サンドボックスでのデータの表示 {#view-data-in-your-sandbox}

+++選択して例を表示

クエリに応じて、AI アシスタントはサンドボックス内のデータに関する追加情報を提供します。 クエリへの応答がサンドボックスにどのように適用されるかを表示するには、「 」を選択します。 **[!UICONTROL サンドボックス内].**

この手順の間、AI アシスタントは、該当する特定のオブジェクトの UI ページへの直接リンクを提供する場合があります。 以下の例では、AI アシスタントは、 [!UICONTROL スキーマ] および [!UICONTROL セグメント] UI ページ。

![「サンドボックス内」オプションのスクリーンショット。](./images/ai-assistant/in-your-sandbox.png)

+++

### 応答を検証 {#verify-the-response}

+++選択すると、ソースの表示方法の例が表示されます

引用を表示し、AI アシスタントの応答を検証するには、 **[!UICONTROL ソースを表示]**. AI Assistant は、応答を裏付けるドキュメントへのリンクを提供します。 AI Assistant が提供するクエリを使用することもできます。 [!UICONTROL 関連する提案] を参照して、元のクエリに関連するトピックをさらに詳しく調べます。

![「ソースを表示」のスクリーンショット。](./images/ai-assistant/show-sources.png)

+++

### データの使用とビジュアライゼーション {#data-usage-and-visualization}

+++「 」を選択すると、データ使用に関する質問とデータの視覚化の例が表示されます

AI Assistant が組織内のデータの使用状況に関するクエリに応答するには、アクティブなサンドボックス内にいる必要があります。

以下の例では、AI Assistant に次のクエリが用意されています。 **「1,000 を超えるプロファイルを持ち、アクティベーションステータスを含むセグメント定義を表示する」** 次に、AI Assistant はセグメントとプロファイルデータを視覚化したグラフで応答します。

![データの使用に関する質問に従います。](./images/ai-assistant/data-usage-question.png)

個々のバーにマウスポインターを置くと、特定のデータを表示できます。 また、展開アイコンを選択すると、グラフの大きな表示を確認できます。

![データのビジュアライゼーションを示す質問に従います。](./images/ai-assistant/data-visualization.png)

ビジュアライゼーションの展開ビューが表示されます。 展開されたモーダルを使用して、データをさらに検査できます。これは、多数の列を持つビジュアライゼーションが返される場合に特に便利です。

![展開されたグラフ。](./images/ai-assistant/chart-expanded.png)

データ使用に関する質問のメッセージが表示されたら、AI Assistant は、その質問に対する回答の計算方法を説明します。 以下の例では、AI アシスタントが、1,000 件を超えるプロファイルとそれぞれのアクティベーションステータスを持つセグメント定義を表示するために実行した手順の概要を説明します。

![AI アシスタントによる回答の計算方法を示すセグメントに関する質問に従います。](./images/ai-assistant/results-explained.png)

また、クエリにフィルターを指定したり、変更を加えたりできます。また、AI Assistant に対して、含めるフィルターに基づいて結果をレンダリングするよう指示することもできます。 例えば、AI Assistant に対して、作成日の順にカウントセグメント定義のトレンドを表示するように依頼したり、合計プロファイル数がゼロのセグメント定義を削除したり、データの表示時には整数ではなく月名を使用したりできます。

+++

### オートコンプリートを使用 {#use-auto-complete}

+++オートコンプリートの例を表示するには、「 」を選択します

オートコンプリート関数を使用して、サンドボックスに存在するデータオブジェクトのリストを受け取ることができます。 オートコンプリートの推奨事項は、セグメント、スキーマ、データセット、ソース、宛先の各ドメインで使用できます。

オートコンプリートを使用するには、プラス記号 (**`+`**) をクエリ内でクリックします。 別の方法として、プラス記号 (**`+`**) をクリックします。 ウィンドウが開き、サンドボックス内の推奨データオブジェクトのリストが表示されます。

![オートコンプリートの例](./images/ai-assistant/auto-complete-one.png)

次に、クエリを実行して質問を完了し、質問を送信するデータオブジェクトを選択します。

![質問と回答を含むオートコンプリートの例](./images/ai-assistant/auto-complete-two.png)

+++

### マルチターンを使用 {#use-multi-turn}

+++選択すると、マルチターンの例が表示されます

AI Assistant の複数回転機能を使用して、体験中により自然な会話をすることができます。 AI アシスタントは、与えられたフォローアップの質問に答えることができます。 このコンテキストは、以前のインタラクションから推論できます。

次の例では、AI Assistant に現在の組織内のデータフローの合計数を尋ねられます。

![マルチターンの例](./images/ai-assistant/multi-turn-one.png)

次に、AI アシスタントが別のフォローアップリクエストを受け取ります。 今回は、AI アシスタントが、組織内に現在存在するデータフローをリストして応答します。

![質問と回答を含むマルチターンの例](./images/ai-assistant/multi-turn-two.png)

+++

## ドキュメント {#documentation}

現在、ドキュメントインデックスはAdobe Experience Platform(Real-Time CDPおよび Audiences) を対象としています。 インデックスは定期的に更新されます。

ドキュメント取得モデルは、Experience Platform(Real-Time CDPおよびオーディエンス ) に基づいてトレーニングされます。 Adobe Experience Platform以外のAdobe(Adobe TargetやCreative Cloudスイートなどの他の製品に関する質問には回答できません )。

## データ使用状況 {#data-usage}

また、次のドメインで、AI Assistant にデータの使用に関する質問をすることもできます。

* 属性
* データフロー
* データセット
* 宛先 _（現時点では、アカウントに関する質問と、データフローに関する質問には回答できません）。_
* スキーマ _（現時点では、フィールドグループに関する質問には回答できません。）_
* セグメント
* ソース _（現時点ではアカウントに関する質問に回答できません。）_

使用状況データクエリの場合、回答に UI の現在の状態が反映されていない可能性があります。 これらの質問を裏付けるデータは、24 時間に 1 度更新されます。 例えば、昼間にReal-Time CDPでユーザーが行った変更は、夜間のデータストアと同期され、朝にユーザーからの質問に利用できるようになります。 質問の形式を「いつセグメントがタイトルを持つか」に設定する必要が生じる場合があります。 {TITLE} 作成済み」 その代わりに、「いつが {TITLE} セグメントが作成されました。」

スキーマ、データセット、属性、宛先、セグメントなどのオブジェクトに関連する特定のデータについて問い合わせるには、サンドボックスにログインする必要があります。

### データ使用に関する質問の例 {#example-data-usage-questions}

+++「 」を選択すると、データ使用に関する質問の例のリストが表示されます

| 質問タイプ | 説明 | 例 |
| --- | --- | --- | 
| データ系列 | 1 つまたは複数のオブジェクトの使用状況を他のExperience Platform・オブジェクト間で追跡 | <ul><li>使用するデータセット {SCHEMA_NAME} スキーマ？</li><li>同じスキーマを使用して取り込まれたデータセットの数</li><li>アクティブ化されたセグメントで使用されたデータセットはどれですか？</li><li>アクティブ化されたセグメントで使用される属性を持つスキーマをリストします。</li><li>アクティブ化されたセグメントを表示する {DESTINATION_ACCOUNT_NAME} およびには 1000 を超えるプロファイルがあります。</li><li>2023 年 1 月以降に変更された、アクティブ化されたセグメントで使用されている属性を表示します。</li><li>を介して取り込まれるデータセット {SOURCE_NAME}?</li><li>関連付けられているデータフロー {DATAFLOW_NAME}</li><li>アクティブ化されたセグメントに関連し、過去 1 年間に作成されたスキーマをリストします。</li></ul> |
| 配分と集計 | Experience Platformオブジェクトの使用に関する概要ベースの質問 | <ul><li>アクティブ化されたセグメントの割合はどれくらいですか？</li><li>セグメント化で使用されるフィールドの数</li><li>どのセグメントが最も多くの宛先に対してアクティブ化されていますか？</li><li>重複したセグメントをリストします。</li><li>アクティブ化されたセグメントを表示する {DESTINATION_ACCOUNT_NAME} プロファイルサイズでランク付けします。</li><li>アクティブ化されていないが 100 を超えるプロファイルを持つセグメントの割合。 名前を見せて。</li><li>データセットにデータを取り込む 3 つのソースコネクタをリストします。</li><li>アクティブ化されたセグメントで使用される、発生した項目に基づく上位 5 つの属性を示します。</li></ul> |
| オブジェクト参照 | Experience Platformオブジェクトまたはそのプロパティを取得またはアクセスします。 | <ul><li>関連付けられたスキーマを持たないデータセット</li><li>使用する属性のリスト {SEGMENT_NAME}?</li><li>プロファイルが有効で、作成後に変更されていないスキーマのリストを教えてください。</li><li>先週変更されたセグメントは何ですか。</li><li>同じセグメント定義を持つセグメントと作成日をリストします。</li><li>どのデータセットが有効か、および各データセットから作成されたセグメント数も含まれます。</li><li>データセット XYZ に関連付けられているソースアカウントは何ですか。</li><li>セグメント定義と変更日を表示する {SEGMENT_NAME}.</li></ul> |

+++

## フィードバックの提供 {#feedback}

>[!BEGINSHADEBOX]

**フィードバックがリクエストされました**

このAlpha段階では、AI アシスタントから受け取った応答に関するフィードバックを提供するように招待されます。 AI Assistant のエクスペリエンスを向上させ続けるために、すべての回答と送信済みのフィードバックがレビューされます。

フィードバックを提供するには、AI Assistant からの応答を受け取った後で上親指または下親指を選択し、表示されるテキストボックスにフィードバックを入力します。 次に、「 **[!UICONTROL フィードバックを送信]** を送信します。

>[!ENDSHADEBOX]

+++フィードバックの提供

>[!BEGINTABS]

>[!TAB 親指を上に]

上サムネールアイコンを選択して、AI アシスタントでのエクスペリエンスの効果に関するフィードバックを提供します。

![肯定的なフィードバックウィンドウ。](./images/ai-assistant/thumbs-up.png)

>[!TAB 親指を下に]

下のサムネールアイコンを選択し、AI Assistant の使用経験に基づいて改善できる点に関するフィードバックを提供します。 この手順の間に、エクスペリエンスに関する特定のコメントを入力することもできます。 コメントで提供されたフィードバックは毎日レビューされます。

![否定的なフィードバックウィンドウ。](./images/ai-assistant/thumbs-down.png)

>[!TAB フラグ]

フラグアイコンを選択すると、AI Assistant を使用したエクスペリエンスに関するさらなるレポートが表示されます。

![レポート結果ウィンドウ。](./images/ai-assistant/flag.png)

>[!ENDTABS]

+++

## 追加情報 {#additional-information}

Experience Platformの AI アシスタントの詳細については、この節を参照してください。

### 注意事項と制限事項 {#caveats-and-limitations}

次の節では、AI Assistant を使用する際に考慮するべき現在の注意事項と制限事項について説明します。
<!-- 
#### Conversational experience

You must consider several nuances regarding the conversational experience when querying the AI Assistant.

>[!NOTE]
>
>These limitations are temporary and are being improved upon throughout the course of the alpha.

>[!BEGINTABS]

>[!TAB Unable to infer context from prior discussion]

The AI Assistant currently cannot reference prior discussions as context for a given question. See the table below for examples:

| Ambiguous question | Clear question | Note |
| --- | --- | --- |
| <ul><li>First question: "What is a segment?"</li><li>Follow up question: "Are there different types of them?"</li></ul>| <ul><li>First question: "What is a segment?"</li><li>Follow up question: "Are there different types of **segments**?"</li></ul> | The AI Assistant cannot infer what "them" means. |
| <ul><li>First question: "What is a segment?"</li><li>Follow up question: "Can you elaborate more?"</li></ul> | <ul><li>First question: "What is a segment?"</li><li>Follow up question: "Explain what a segment is in depth"</li></ul> | The AI Assistant cannot intelligently reference documentation based on "more". |
| <ul><li>First question: "What is a segment?"</li><li>Follow up question: "Can you give me an example of one?"</li></ul> | <ul><li>First question: "What is a segment?"</li><li>Follow up question: "Can you give me an example of a segment?"</li></ul> | The AI Assistant cannot infer what you want an example of.|
| <ul><li>First question: "What is a batch segment?"</li><li>Follow up question: "How does it compare to a streaming segment?"</li></ul> | <ul><li>First question: "What is a batch segment?"</li><li>Follow up question: "Can you compare a streaming segment to a batch segment?"</li></ul> | The AI Assistant cannot infer what "it" is referring to and thus cannot compare the streaming segment. |
| <ul><li>First question: "How many segments do I have?"</li><li>Follow up question: "How many of them use Facebook as a destination?"</li></ul> | <ul><li>First question: "How many segments do I have?"</li><li>Follow up question: "How many of the segments that I have are using Facebook as a destination?"</li></ul> | The AI Assistant is cannot infer what "them" is referring to. |

{style="table-layout:auto"}

>[!TAB Unable to infer context from a page]

When asking the AI Assistant about a particular element of the Experience Platform UI page that you are on, you must clearly define the specific element within your question. 

| Ambiguous question | Clear question | Note |
| --- | --- | --- |
| "What does this do?" | "What does {PAGE_NAME} do? | The AI Assistant cannot infer what "this" is referring to. You must provide the specific page element that you are querying about. |
| "Why won't it save?" | "Why can't I save a new sandbox called {NAME}?" | The AI Assistant cannot infer what "it" is referring to and cannot know that you are having issues with an entity. |

{style="table-layout:auto"}

Furthermore, the AI Assistant can only answer questions regarding error messages, given that the error is documented in Experience League.

>[!TAB Ambiguity]

You must phrase your questions clearly and scope them within a product, application, or domain, as the AI Assistant currently cannot disambiguate questions.

| Ambiguous question | Clear question | Note |
| --- | --- | --- |
| "How do I create a filter? | How do I create a filter in Profile Query Language? | You must specify the feature that which you are filtering for because a variety of Experience Platform features support filtering. |
| "How do I get started? | How do I get started using destinations? | You must provide clarity on your goals and use case because overly broad concepts may result in generic or unnecessarily specific answers. |

{style="table-layout:auto"}

>[!ENDTABS] -->

#### 限られた小さな話

AI アシスタントとの小さな話し合いは可能ですが、現在、この処理能力は制限されています。

#### 機能に関する質問

AI アシスタントは、何ができるかを不正確な印象を与える可能性があります。 次のタイプの質問に対して、間違って回答する場合があります。

| 質問の例 | 注意 |
| --- | --- |
| 「～に関する質問に答えていただけますか？ {ENTITY}?」 | AI アシスタントがインデックス内の特定のエンティティを参照する単一のページを見つけられる限り、はいと応答します。 |
| 「知ってる？ **x** 言語？」 | AI アシスタントは現在、英語のみをサポートしていますが、基盤となるモデルが英語をサポートできるので、「はい」と答える場合があります。 |
| 「できるか…?」 | AI アシスタントは、できない場合でも「はい」と答える場合があります。 |

### ヒント {#tips}

次の節では、AI Assistant を使用する際に考慮すべきヒントと回避策の概要を説明します。

#### 間違った情報源を使って質問に答えることができます

使用状況データに関する質問が、ドキュメントに基づく回答になる場合があります。 これは、AI アシスタントが誤って質問を誤った情報ソースにルーティングする可能性があるためです。 これを防ぐには、次の方法があります。

* より多くの SQL に似た言語を使用するために質問を書き換える
* 使用する情報ソースを明示的に呼び出す。

以下の表に例を示します。

| 悪い質問 | 良い質問 | メモ |
| --- | --- | --- |
| 最大のセグメントは何ですか？ | 最大のセグメントは何ですか？ データを使用しています。 | データに基づいて回答を行うことを AI アシスタントに明示的に伝えます。 |
| 最大のセグメントは何ですか？ | 最大のセグメントをリストします。 | 「なに…」という質問がドキュメントベースの質問と間違える場合があります。 「list」のようなコマンドを使用すると、コンテキスト内のデータを使用して質問していることを示す強力なインジケーターが表示されます。 |
| データセットはいくつありますか？ | データセットをカウントします。 | 元の質問はセグメントで機能しますが、データセットでは機能しない場合があります。 |
