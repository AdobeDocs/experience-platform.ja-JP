---
title: Adobe Experience Platform リリースノート 2023年8月
description: Adobe Experience Platform の 2023年8月のリリースノート。
exl-id: c67dca3a-eccb-427e-8ab3-b69c51b57938
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1743'
ht-degree: 40%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年8月23日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [Real-Time Customer Data Platform](#rtcdp)
- [属性ベースのアクセス制御](#abac)
- [ダッシュボード](#dashboards)
- [データ収集](#data-collection)
- [データ取得](#data-ingestion)
- [データ準備](#data-prep)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [ID サービス](#identity-service)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## Real-Time Customer Data Platform {#rtcdp}

Experience Platform上に構築されたReal-Time Customer Data Platform（[!DNL Real-Time CDP]）は、企業が既知および未知のデータを統合し、カスタマージャーニーを通じてインテリジェントな意思決定によりカスタマープロファイルをアクティブ化するのに役立ちます。

[!DNL Real-Time CDP] では、複数のエンタープライズデータソースを組み合わせて、リアルタイムで顧客プロファイルを作成します。これらのプロファイルから構築されたセグメントをダウンストリームの宛先に送信し、すべてのチャネルとデバイスにわたって 1 対 1 のパーソナライズされた顧客体験を提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| インテリジェントな再エンゲージメントユースケースのガイド | [&#x200B; インテリジェントな再エンゲージメント &#x200B;](../../rtcdp/use-case-guides/intelligent-re-engagement/intelligent-re-engagement.md) ユースケースガイドでは、コンバージョンを完了する前にコンバージョンを放棄した顧客を、インテリジェントかつ責任ある方法で再エンゲージする方法の詳細を提供します。 このガイドでは、次のジャーニーの例を使用して、顧客を再び引き付けます。 <ul><li>再エンゲージメントジャーニー – 製品閲覧を放棄した顧客をターゲティングします。</li><li>放棄された買い物かごジャーニー – 商品を買い物かごに入れたが、購入をまだ完了していない顧客をターゲティングします。</li><li>注文確認ジャーニー – 製品購入に重点</li></ul> [&#x200B; インテリジェント再エンゲージメントユースケースガイド &#x200B;](../../rtcdp/use-case-guides/intelligent-re-engagement/intelligent-re-engagement.md) の下部にある詳細なフィードバックオプションのリンクを使用して、フィードバックを提供してください。 |
| パートナーデータサポート | Real-Time CDPでパートナーをソースとする見込み客プロファイルとパートナー ID を使用して上位ファネルマーケティングを実施し、新規顧客にリーチしてファーストパーティデータを強化します。 <ul><li>顧客の獲得とアドレス可能性：任意のデータパートナーから提供されるクッキーなしの識別子とハッシュ化された PII を活用して、新規顧客にリーチし、サードパーティ cookie の依存関係を軽減します。</li><li>単一システムでの完全なファネルマーケティング：単一システムでの見込み顧客と既知顧客に対するセルフサービスのセグメント化、オーディエンスキュレーション、ネイティブアクティベーション。</li><li>信頼の基盤：特許取得済みのデータの使用、ラベル付け、アクセス制御などを通じて、パートナーデータおよびプロファイルを管理し、責任を持って市場に提供します。 詳しくは、以下のユースケースガイドを参照してください。見込み客のユースケースガイドが使用できるようになりました。 見込み客のユースケースを通じて新規顧客を獲得およびエンゲージメントする方法については、見込み客のユースケースのガイドを参照してください。<ul><li>[&#x200B; 試掘等 &#x200B;](../../rtcdp/partner-data/prospecting.md)</li><li>[オンサイトのパーソナライズ機能](../../rtcdp/partner-data/onsite-personalization.md)</li><li>[&#x200B; ファーストパーティプロファイルの補足 &#x200B;](../../rtcdp/partner-data/supplement-first-party-profiles.md)</li><li>[&#x200B; 見込み客オーディエンスのアクティブ化 &#x200B;](../../destinations/ui/activate-prospect-audiences.md)</li></ul> |

{style="table-layout:auto"}

詳しくは、[Real-Time CDPの概要 &#x200B;](../../rtcdp/overview.md) を参照してください。

## 属性ベースのアクセス制御 {#abac}

属性ベースのアクセス制御は、プライバシーを重視するブランドが、ユーザーアクセスをより柔軟に管理できるようにする、Adobe Experience Platform の機能です。ユーザーの役割に、スキーマフィールドやセグメントなどの個々のオブジェクトを割り当てることができます。 この機能を使用すると、組織内の特定のExperience Platform ユーザーに対する個々のオブジェクトへのアクセスを許可または取り消すことができます。

属性ベースのアクセス制御により、組織の管理者は、すべてのExperience Platform ワークフローとリソースにわたって、機密性の高い個人データ（SPD）、個人を特定できる情報（PII）、およびその他のカスタマイズされた種類のデータへのユーザーのアクセスを制御できます。 管理者は、特定のフィールドと、それらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 権限ポリシーサンドボックス設定 | 新しい [&#x200B; 権限ポリシーサンドボックス設定 &#x200B;](../../access-control/abac/ui/policies.md) 機能を使用すると、要件に応じて、すべてのサンドボックスまたは選択した数のサンドボックスに属性ベースのアクセス制御ポリシーを適用できます。 |

{style="table-layout:auto"}

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](../../access-control/abac/overview.md)を参照してください。属性ベースのアクセス制御ワークフローの包括的なガイドについては、[属性ベースのアクセス制御エンドツーエンドガイド](../../access-control/abac/end-to-end-guide.md)を参照してください。

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを確認できる複数のダッシュボードを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 同意分析とトラッキングのユースケース | [&#x200B; 同意分析とトラッキングドキュメント &#x200B;](../../dashboards/insights-use-cases/consent-analysis.md) を使用して、Real-Time CDP データの様々なマーケティング使用例に対する同意ダッシュボードを作成する方法を説明します。 ビジネスニーズに適した属性でオーディエンスを作成し、Adobe Experience Platform UI の事前設定済みウィジェットを使用してインサイトを利用する方法について詳しく説明します。 また、ユーザー定義ダッシュボード機能を使用して独自のカスタムウィジェットを作成する方法についても説明します。 このドキュメントでは、同意のトレンドと同意の重複のユースケースについて説明します。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| タグとイベント転送 | [Experience Platform タグ （中国） &#x200B;](/help/tags/ui/publishing/premium-cdn.md) | 新しいExperience Platform Tags （China）機能により、Web サイトの信頼性と待ち時間が向上し、中国で Web サイトにタグをデプロイするお客様の応答時間が短縮されます。 お客様は、中国で web サイトを実装する際に、タグライブラリのJavaScript コードを利用できるようになりました。 この機能は Unified Provisioning Protocol （UPP）にも追加されており、購入後の製品の導入を自動化できます。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; データ収集の概要 &#x200B;](../../tags/home.md) を参照してください。

## データ取得 {#data-ingestion}

Adobe Experience Platform は、あらゆるタイプ、あらゆる待ち時間のデータを取り込める、豊富な機能セットを提供します。取得は、Batch API または Streaming API、アドビの組み込みソース、データ統合パートナー、Adobe Experience Platform UI を使用して行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| データ取り込みワークフローの変更 | 指定されたデータタイプを超える値を含むデータの行（整数データタイプとして渡される long データなど）は拒否され、エラーメッセージがレポートされるようになりました。 以前は、これらの行は警告なしで拒否されていました。 |

詳しくは、[&#x200B; データ取り込みの概要 &#x200B;](../../ingestion/home.md) を参照してください。

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| セカンダリ ID のフィルタリングのサポート | Data Prep を使用して、AAID や AACUSTOMID など、Adobe Analyticsからの ID をフィルタリングできるようになりました。 除外した場合、これらの ID はリアルタイム顧客プロファイルに取り込まれません。 フィルタリングされていないデータは、引き続きデータレイクに取り込まれます。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; データ準備の概要 &#x200B;](../../data-prep/home.md) を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能** {#destinations-new-updated-functionality}

- クラウドストレージの宛先に対して、[&#x200B; 見込み客オーディエンスをアクティブ化 &#x200B;](../../destinations/ui/activate-prospect-audiences.md) できるようになりました。
- サンドボックスごとに最大 100 個の宛先という一般的な [&#x200B; アクティベーションガードレール &#x200B;](../../destinations/guardrails.md#general-activation-guardrails) が、_ハード制限_ に更新されました。

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| クラス | [[!UICONTROL XDM 個人見込み客プロファイル]](https://github.com/adobe/xdm/pull/1758/files) | このクラスを使用すると、データベンダーの最上位層の顧客獲得ユースケースをソースとする見込み客プロファイルを取り込むことができます。 例と詳細については、[[!UICONTROL XDM 個人見込み客プロファイル &#x200B;]](../../xdm/classes/prospect.md) のドキュメントを参照してください。 |

{style="table-layout:auto"}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明のアップデート |
| --- | --- | --- |
| 拡張機能（[!UICONTROL Adobe Analytics ExperienceEvent 完全拡張機能 &#x200B;]） | [[!UICONTROL &#x200B; コンテキストデータ &#x200B;]](https://github.com/adobe/xdm/pull/1761/files) | [!UICONTROL &#x200B; コンテキストデータ &#x200B;]Adobe Analyticsのコンテキストデータを提供するために、[!UICONTROL Adobe Analytics ExperienceEvent 完全拡張機能 &#x200B;] に追加されたマップオブジェクト。 |
| フィールドグループ | 複数 | いくつかのフィールドが [[!UICONTROL &#x200B; エンリッチメントされたイベントセグメントの詳細 &#x200B;]](https://github.com/adobe/xdm/pull/1760/files) に追加されました。 |

{style="table-layout:auto"}

詳しくは、「[XDM システムの概要 &#x200B;](../../xdm/home.md)」を参照してください。

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| ID グラフの制限の変更 | 9 月末までに ID グラフはグラフあたり 50 個の ID に変更され、最新の ID が取り込まれます。 そのため、最も古い ID が取り込みタイムスタンプと ID タイプに基づいて削除され、cookie ID タイプが最初に削除されます。 現在、ID グラフには 1 つのグラフあたり 150 個の ID という制限があり、この制限に達すると、グラフは更新されなくなります。 実稼動サンドボックスに次の ID タイプが含まれる場合は、アカウント担当者に連絡して、ID タイプの変更をリクエストしてください。 <ul><li>ユーザー識別子（CRM ID など）が cookie/デバイス ID タイプとして設定されるカスタム名前空間。</li><li>cookie とデバイスの識別子がクロスデバイス id タイプとして設定されるカスタム名前空間。</li></ul> これらのリクエストは、Adobe エンジニアリングによって手動で処理されます。 詳しくは、[ID サービスデータのガードレール &#x200B;](../../identity-service/guardrails.md) を参照してください。 |

詳しくは、[ID サービスの概要 &#x200B;](../../identity-service/home.md) を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] を使用すると、[!DNL Experience Platform] に保存されている、個人（顧客、見込み客、ユーザー、組織など）に関連するデータをオーディエンスにセグメント化できます。オーディエンスは、セグメント定義または [!DNL Real-Time Customer Profile] データの他のソースを通じて作成できます。これらのオーディエンスは [!DNL Experience Platform] で一元的に設定および管理されており、Adobe ソリューションから簡単にアクセスできます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 類似オーディエンス（限定提供） | 類似オーディエンスは、各オーディエンスに関するインテリジェントなインサイトを提供し、機械学習ベースのインサイトを活用して、マーケティングキャンペーンで高価値顧客を特定してターゲットを設定します。 類似オーディエンスを使用すると、パフォーマンスの高いオーディエンスに類似した顧客をターゲットにしたり、以前にコンバージョンされたオーディエンスに類似した顧客をターゲットにしたりして、拡張オーディエンスを作成できます。 類似オーディエンスについて詳しくは、[&#x200B; 類似オーディエンスの概要 &#x200B;](../../segmentation/types/account-audiences.md) を参照してください。 |

{style="table-layout:auto"}

詳しくは、「セグメント化の概要 [&#x200B; を参照してください &#x200B;](../../segmentation/home.md)。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL SugarCRM] の一般提供 | [!DNL SugarCRM] ソースが利用可能になりました。 [!DNL SugarCRM Accounts & Contacts] および [!DNL SugarCRM Events] ソースを使用して、[!DNL SugarCRM] アカウントから Experience Platform にデータを取り込みます。詳しくは、[[!DNL SugarCRM] 概要](../../sources/connectors/crm/sugarcrm.md)を参照してください。 |
| UI でのソースデータフローのオンデマンド取り込みのサポート | UI で、既存のソースデータフローに対して、オンデマンドでフロー実行を作成できるようになりました。 詳しくは、[UI を使用したソースのオンデマンドフロー実行の作成 &#x200B;](../../sources/tutorials/ui/on-demand-ingestion.md) に関するガイドを参照してください。 |
| Adobe Analyticsの新しい `correlationID` フィールドのサポート | `_experience.decisioning.propositions.scopeDetails.correlationID` フィールドが Adobe Analytics ソースコネクタスキーマで使用できるようになりました。このフィールドは、A4T 分類をサポートするために使用され、2023 年 9 月から設定されます。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; ソースの概要 &#x200B;](../../sources/home.md) を参照してください。
