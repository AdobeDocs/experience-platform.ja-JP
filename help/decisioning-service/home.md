---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: 判定サービス
topic: overview
translation-type: tm+mt
source-git-commit: bd9884a24c5301121f30090946ab24d9c394db1b
workflow-type: tm+mt
source-wordcount: '1592'
ht-degree: 77%

---


# 判定サービスの概要

[!DNL Decisioning Service] は、Adobe Experience Platform上で実行されるアプリケーションで、パーソナライズされた、最適化された、調整されたエクスペリエンスを作成する機能を提供します。 Using [!DNL Decisioning Service], you can determine the best *option* from a set of available choices. これらのオプションは代替オプションとも呼ばれ、オファー、お勧め製品、web エクスペリエンスのコンテンツコンポーネント、会話スクリプト、実行するアクションなどがあります。現在、*オファー判定*&#x200B;の使用事例と領域がサポートされています。ここでは、判定オプションは特にオファーとしてモデル化されています。また、今後はほかの使用事例もサポートされる予定です。

With [!DNL Decisioning Service], customers can reuse business logic as well as share a catalog of options across channels and applications. アプリケーション内で判定オプション（およびそれを選択するための戦略）を徹底的に管理する代わりに、顧客は、エンドユーザーが取引先や組織とやり取りするタイミング、方法、チャネルに関係なく、判定オプションを活用できるようになりました。

判定戦略では、多くのチャネルやアプリケーションにおいて、顧客が実行してきた多数のインタラクションを考慮することができます。例えば、コールセンターへの申請アクティビティがあった場合、マーケティングメッセージを有効にしたり、苦情の後はしばらくの間マーケティングメッセージを無効にしたりすることができます。メッセージ自体は、顧客による購入や投稿レビューの内容に基づいて作成できます。

[!DNL Decisioning Service] エクスペリエンスのパーソナライゼーションを促進します。

| エクスペリエンス判定前 | エクスペリエンス判定の後 |
| --- | --- |
| 単一のチャネル内または一部のエクスペリエンスタッチポイント内で、ユーザーのエクスペリエンスがパーソナライズされ最適化されます。 | エクスペリエンス（応答）は、インタラクションを通して調整されます。 |
| 最適化は、エンドユーザーのジャーニーの 1 つの（通常は）短い段階に重点を置いておこなわれます。 | 判定は、過去から最新の状況において検出されたビヘイビアーのインタラクション履歴全体に基づいておこなわれます。 |
| オプションと、顧客のエクスペリエンス中に提示するオプションを選択するための戦略は、通常、アプリケーションの内部でコード化されます。 | 最適なオプションを選択するための戦略は、チャネル固有のアプリケーション外で定義され、再利用可能になります。 |
| 顧客体験は、単純な目標に応じてパーソナライズされ、最適化されます。例えば、web ページでのチェックアウトの成功数を増加させる、担当者とのインタラクションで提示されるオファーの受け入れ数を延ばすといった目標です。 | 顧客体験は、顧客の現在のニーズを総合的に理解した上で最適化され、ユーザーのすべてのエクスペリエンス（良い、悪い）に合わせて調整されます。例えば、マーケティングキャンペーンは、製品やサービスについて最近苦情を申し立てた顧客には不適切な場合があります。 |

[!DNL Decisioning Service] エクスペリエンスのパーソナライズ機能を単一のチャネルでのターゲティングから移行し、チャネルに依存しないブランドに対する顧客のエンゲージメントのライフサイクル全体的な段階を決定します。 ライフサイクルのステージは、セグメントメンバーシップよりもはるかに複雑で、ほとんどの場合は複雑なイベントストリーム、ビジネスルールおよび予測属性に基づいています。

同様の使用事例を提供するために、製品やサービスで使用されるそのほかの用語は次のとおりです。

- Real-time Interaction Management（RTIM）
- ジャーニー管理
- オムニチャネルマーケティングおよびパーソナライゼーション
- リアルタイム判定

## How does [!DNL Decisioning Service] work?

Experiences can be customized using [!DNL Decisioning Service] in real time, as your customer engages with your brand via an inbound channel, such as your site or mobile app. また、判定サービスは、電子メールやプッシュ通知といった、アウトバウンドチャネルを介したメッセージをカスタマイズする場合にも使用できます。

判定は、様々な方法でおこなうことができます。1 つは、オプションが残り 1 つになるまで、またはオプションの一部のみが残るか、減らされたオプションの中から 1 つのオプションがランダムに選択されるまで、オプションを徐々に減らしていく方法です。これに似た方法に、計算式にしたがって 1 つのオプションを選択するという方法があります。ランキングに沿ってオプションを選択するには、関数を使用します。オファー判定の場合、関数ではコスト（ビジネスに対するオファーの価値）を計算し、事前に決定した、エンドユーザーがオファーを受け入れる確率を使用できます。結果のスコアを使用して、オファーを順に並べることができます。

または、同様のオプションが提示された類似の顧客とのこれまでのインタラクションから収集された結果に基づいて、戦略を作成することもできます。この戦略では、優先度の値を計算した関数が学習されます。最適な結果値は、アクティビティの目標に関連付けられます。また、予測のパフォーマンスインジケーターは、オプションの提示後に結果に達した頻度を示します。

### 判定戦略

判定戦略は、「_アクティビティ_」と呼ばれるオブジェクトを介して設定します。各判定戦略は基本的に、N 個のオプション {o1,o2,...oN} を入力値として取り、オプションの順序リスト（o1, o2,…oK）を生成するアルゴリズムまたは関数です。最適化条件にしたがって、リストの最初のオプションが最適なオプションと見なされ、リストの 2 番目のオプションが 2 番目に適切なオプションと見なされます。

顧客のジャーニーの任意の時点で、特定のアクティビティに最適なオプションが、最新のコンテキスト変数、ルールおよび制約に基づいて再評価されます。コンテキスト変数には、に保存されているレコードが含まれ [!DNL Real Time Customer Profile]ます。 中央のレコードエンティティは顧客のプロファイルですが、運用ビジネスデータなどの他のエンティティもアクティビティで同様に使用できます。

上位 K 個のオプションのリストを生成するアルゴリズムまたは関数は、使用事例によって異なります。そのアルゴリズムの内部コンポーネントは、使用事例によって異なります。コンポーネントは、設計時にリポジトリーで定義され、使用事例固有の判定戦略の手順に「コンパイル」されます。

![判定の最適化](./images/decisioning-optimization.png)

## Working with [!DNL Decisioning Service]

The [!DNL Decisioning Service], like other [!DNL Platform] services, adopts an API first philosophy. つまり、API は主要インターフェイスで、管理関数を含むすべての関数は API を介して使用可能になります。It also means that other [!DNL Platform] services, Adobe solutions, and 3rd party integrations use the same APIs.

You can use [!DNL Decisioning Service] in a synchronous request-response interaction mode facilitated by a simple HTTP REST API. API 呼び出しは、1 つのプロファイルに対して現在最適なオプションを返します。「現在最適なオプション」の選択肢は、特定のアクティビティで考慮される、すべてのオプションに適用されるルールと制約に応じて変わります。REST API を使用すると、複数のアクティビティにとって次に最適なオプションを一度に取得できます。こうして、複数のチャネルにわたってオプションが提供されます。複数のアクティビティのレスポンスをまとめて取得する場合、追加のルールが適用される場合があります。

![判定 API](./images/decisioning-API.png)

### Integration with other [!DNL Platform] workflows

Use of [!DNL Decisioning Service] is optional and only requires a few steps in addition to the typical steps required to create [!DNL Profile] entities and manage them.

>[!NOTE]
>
>を最大限に活用するため [!DNL Real-time Customer Profile]に、はプロファイルストアと [!DNL Decisioning Service] 直接統合されます。 API 呼び出しでは、特定のプロファイルの ID の 1 つを示す必要があります。

通常は、初めにプロファイルを構築してから、以下の手順を実行します。

- 認証先 [!DNL Experience Platform]。
- プロファイルクラスに基づいてスキーマを定義し、必要な場合はエクスペリエンスイベントクラスに基づいてスキーマを定義します。
- Configure a dataset to upload record and time series data to [!DNL Customer Profile].
- 前の手順で設定したデータセットを介してデータを追加し、パイプラインを介してインスタンスデータをストリーミングします。
- Stream experience events into the [!DNL Platform] to enrich the profile with behavioral data.

Additionally, to use [!DNL Decisioning Service], the following steps:

- Repository API を使用して、判定コンポーネントを定義します。これらは、決定戦略を構成するビジネスロジックエンティティです。The decision components will be automatically compiled into a format used by the [!DNL Decision Service Runtime]. 次の図の左側に、Repository API を示します。
- Runtime API を呼び出して、前の手順で定義したビジネスロジックに従って最適なオプションを取得します。The [!DNL Decision Service Runtime] APIs are illustrated on the right side in the diagram below.

![判定 - API1](./images/decisioning-API1.png)

ビジネスロジックエンティティは、自動的かつ連続的にアクティブ化されます。新しいオプションがリポジトリーに保存され「承認済み」とマークされると、すぐにそのオプションが使用可能なオプションの候補となります。判定ルールが更新されると、すぐにルールセットが再構築され、実行時に向けて準備されます。この自動アクティベーション手順では、ビジネスロジックによって定義された、実行時のコンテキストに依存しない制約が評価されます。The results of this activation step are sent to a cache where they are available to the [!DNL Decisioning Service] runtime. 次の図に、この処理を示します。

![Decisioning判定 - API2](./images/decisioning-API2.png)

Once the option sets, rule sets and constraints are activated, and have been pushed to [!DNL Decisioning Service] nodes, a simple API is used to post a request for a decision. 通常、API は配信サービスによって呼び出され、提案されたオプション（次に最適なアクション、次に最適なオファーなど）を取得し、エクスペリエンスを構築するか、アクションを実行します。提案がオファーの場合、そのオファーを表すコンテンツが検索され、エンドユーザーに提供されるエクスペリエンスに挿入されます。次の図に、この処理を示します。

![判定 - API3](./images/decisioning-API3.png)

[!DNL Delivery Service] デシジョンリクエスト用のデータをアセンブリします。 次に、最適なオプションを判定するプロファイルエンティティの ID を決定します。It also assembles any context data that is not stored in [!DNL Customer Profile] but is potentially used by the decision logic.

判定ロジックはアクティビティ別に整理されます。各アクティビティでは、そのアクティビティで考慮する必要のあるオプションのサブセットのフィルターと、単一のフォールバックオプションが指定されます。

それぞれの判定をおこなうには、最初に制約を適用してオプションの数を減らし、残りのオプションをランク付けします。Although most of the logic is evaluated inside [!DNL Decisioning Service], various adjunct services are used to help with these two aspects. 例えば、キャッピングサービスは、判定におけるオプションの使用可能頻度の上限を管理します。また、別のサービスでは、プロファイルとオプションのスコアの計算に使用される機械学習モデルをホストすることができます。

Repository API の使用について詳しくは、[API を使用した判定エンティティとルールの管理](./tutorials/entities.md)に関するチュートリアルを参照してください。

To learn more about using the [!DNL Decisioning Service] runtime, see the tutorial on [Working with the Decisioning Service runtime using APIs](./tutorials/runtime.md)