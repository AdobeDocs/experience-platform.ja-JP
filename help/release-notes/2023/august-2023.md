---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2023年8月のリリースノート。
source-git-commit: 5181d39e92fbf957f154c3b1dcf4f9af90cfeae9
workflow-type: tm+mt
source-wordcount: '1749'
ht-degree: 42%

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

Experience Platform 上に構築された Real-time Customer Data Platform（[!DNL Real-Time CDP]）は、企業が既知および未知のデータを統合し、カスタマージャーニーを通じてインテリジェントな意思決定により顧客プロファイルをアクティブ化するのに役立ちます。

[!DNL Real-Time CDP] では、複数のエンタープライズデータソースを組み合わせて、リアルタイムで顧客プロファイルを作成します。これらのプロファイルから構築されたセグメントをダウンストリームの宛先に送信し、すべてのチャネルとデバイスにわたって 1 対 1 のパーソナライズされた顧客体験を提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| インテリジェントな再エンゲージメントの使用例ガイド | The [インテリジェントな再エンゲージメント](../../rtcdp/use-case-guides/intelligent-re-engagement/intelligent-re-engagement.md) インテリジェントで責任のある方法でコンバージョンを完了する前に、コンバージョンを放棄した顧客を再び関与させる方法の詳細を、使用例ガイドで説明します。 このガイドでは、次のジャーニーの例を使用して、顧客を再び関与させます。 <ul><li>再エンゲージメントジャーニー — 製品の閲覧を中止した顧客をターゲティングします。</li><li>カート放棄のジャーニー — 買い物かごに製品を入れたが購入を完了していない顧客をターゲティングします。</li><li>注文確認のジャーニー — 製品購入に重点を置く</li></ul> 詳細なフィードバックオプションリンクは、 [インテリジェントな再エンゲージメントの使用例ガイド](../../rtcdp/use-case-guides/intelligent-re-engagement/intelligent-re-engagement.md) フィードバックを提供する。 |
| パートナーデータサポート | 新しい顧客にリーチし、ファーストパーティデータをエンリッチメントするために、パートナーソースの見込み客プロファイルとパートナー ID を使用して、Real-Time CDPで上位ファネルマーケティングを実行します。 <ul><li>顧客の獲得とアドレス可能性：選択したデータパートナーからの cookie にも対応できない識別子とハッシュ化された PII を活用して、新しい顧客にリーチし、サードパーティ cookie への依存を減らします。</li><li>単一システムでの完全なファネルマーケティング：セルフサービスのセグメント化、オーディエンスのキュレーション、単一システムでの見込み客と既知の顧客に対するネイティブアクティベーション。</li><li>信頼の基盤：特許取得済みのデータの使用、ラベル付け、アクセス制御などを使用して、パートナーのデータとプロファイルを責任を持って市場に提供します。 詳しくは、次の使用例ガイドを参照してください。今後の見込みの使用例ガイドが利用できます。 見込み客の使用例を通じて新しい顧客を惹きつけ、獲得する方法については、見込み客の使用例ガイドを参照してください。<ul><li>[見込み](../../rtcdp/partner-data/prospecting.md)</li><li>[オンサイトのパーソナライズ機能](../../rtcdp/partner-data/onsite-personalization.md)</li><li>[ファーストパーティプロファイルを補完](../../rtcdp/partner-data/supplement-first-party-profiles.md)</li><li>[見込み客オーディエンスの有効化](../../destinations/ui/activate-prospect-audiences.md)</li></ul> |

{style="table-layout:auto"}

詳しくは、 [Real-Time CDPの概要](../../rtcdp/overview.md).

## 属性ベースのアクセス制御 {#abac}

属性ベースのアクセス制御は、プライバシーを重視するブランドが、ユーザーアクセスをより柔軟に管理できるようにする、Adobe Experience Platform の機能です。ユーザーの役割に、スキーマフィールドやセグメントなどの個々のオブジェクトを割り当てることができます。 この機能を使用すると、組織内の特定の Platform ユーザーに対する個々のオブジェクトへのアクセスを許可または取り消すことができます。

属性ベースのアクセス制御により、組織の管理者は、すべての Platform ワークフローとリソースにわたって、機密性の高い個人データ（SPD）、個人を特定できる情報（PII）、およびその他のカスタマイズされた種類のデータへのユーザーのアクセスを制御できます。管理者は、特定のフィールドと、それらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 権限ポリシーのサンドボックス設定 | 新しい [権限ポリシーサンドボックスの設定](../../access-control/abac/ui/policies.md) 機能を使用すると、ニーズと要件に応じて、すべてのサンドボックスまたは選択した数のサンドボックスに対して、属性ベースのアクセス制御ポリシーを適用できます。 |

{style="table-layout:auto"}

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](../../access-control/abac/overview.md)を参照してください。属性ベースのアクセス制御ワークフローの包括的なガイドについては、[属性ベースのアクセス制御エンドツーエンドガイド](../../access-control/abac/end-to-end-guide.md)を参照してください。

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを確認できる複数のダッシュボードを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 同意分析およびトラッキングの使用例 | を使用して、Real-Time CDPデータの様々なマーケティングユースケースに対する同意ダッシュボードを作成する方法を説明します。 [同意分析および追跡ドキュメント](../../dashboards/insights-use-cases/consent-analysis.md). ビジネスニーズに適した属性でオーディエンスを作成し、Adobe Experience Platform UI で事前設定済みのウィジェットを使用してインサイトを利用する方法について詳しく説明します。 また、ユーザー定義のダッシュボード機能を使用して独自のカスタムウィジェットを作成する方法についても説明します。 このドキュメントでは、同意トレンドと同意重複の使用例について説明します。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| タイプ | 機能 | 説明 |
| --- | --- | --- |
| タグとイベント転送 | [Experience Platformタグ（中国）](/help/tags/ui/publishing/premium-cdn.md) | 新しいExperience Platformタグ（中国）機能により、Web サイトの信頼性と遅延が向上し、中国の Web サイトにタグをデプロイするお客様の応答時間が速くなります。 お客様は、中国で Web サイトを実装する際に、タグライブラリの JavaScript コードを利用できるようになりました。 この機能は、統合プロビジョニングプロトコル (UPP) にも追加され、購入後に製品のデプロイメントを自動化できます。 |

{style="table-layout:auto"}

詳しくは、 [データコレクションの概要](../../tags/home.md).

## データ取得 {#data-ingestion}

Adobe Experience Platform は、あらゆる種類および遅延のデータを取得するための豊富な機能セットを提供します。取得は、Batch API または Streaming API、アドビの組み込みソース、データ統合パートナー、Adobe Experience Platform UI を使用して行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| データ取り込みワークフローの変更 | 指定したデータ型より大きい値を含むデータの行（例えば、整数データ型として渡された長いデータ）は、拒否され、エラーメッセージがレポートされるようになりました。 以前は、これらの行は警告なしで拒否されていました。 |

詳しくは、 [データ取得の概要](../../ingestion/home.md).

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| セカンダリ ID のフィルタリングのサポート | Data Prep を使用して、AAID や AACUSTOMID など、Adobe Analyticsからの ID を除外できるようになりました。 除外した場合、これらの ID はリアルタイム顧客プロファイルに取り込まれません。 フィルターを適用していないデータは、引き続きデータレイクに取り込まれます。 |

{style="table-layout:auto"}

詳しくは、 [データ準備の概要](../../data-prep/home.md).

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能** {#destinations-new-updated-functionality}

- 次の操作を実行できます。 [見込み客オーディエンスの有効化](../../destinations/ui/activate-prospect-audiences.md) をファイルベースの宛先に追加する。
- 将軍 [活性化ガードレール](../../destinations/guardrails.md#general-activation-guardrails) サンドボックスあたり最大 100 件の宛先が、 _ハードリミット_.

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新しい XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明 |
| --- | --- | --- |
| クラス | [[!UICONTROL XDM 個人見込み客プロファイル]](https://github.com/adobe/xdm/pull/1758/files) | このクラスを使用して、データベンダーのファネルトップの顧客獲得の使用例から提供される見込み客プロファイルを取り込みます。 詳しくは、 [[!UICONTROL XDM 個別見込み客プロファイル]](../../xdm/classes/prospect.md) 例を参照し、詳細を確認するドキュメントです。 |

{style="table-layout:auto"}

**更新された XDM コンポーネント**

| コンポーネントのタイプ | 名前 | 説明のアップデート |
| --- | --- | --- |
| 拡張子 ([!UICONTROL Adobe Analytics ExperienceEvent Full 拡張機能]) | [[!UICONTROL コンテキストデータ]](https://github.com/adobe/xdm/pull/1761/files) | [!UICONTROL コンテキストデータ] マップオブジェクトを [!UICONTROL Adobe Analytics ExperienceEvent Full 拡張機能] Adobe Analyticsのコンテキストデータを提供する。 |
| フィールドグループ | 複数 | 複数のフィールドが [[!UICONTROL 強化されたイベントセグメントの詳細]](https://github.com/adobe/xdm/pull/1760/files). |

{style="table-layout:auto"}

詳しくは、 [XDM システムの概要](../../xdm/home.md).

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| ID グラフの制限の変更 | 9 月末までに、ID グラフはグラフごとに 50 個の ID に変更され、最新の ID が取り込まれます。 その結果、取り込みタイムスタンプと ID タイプに基づいて最も古い ID が削除され、Cookie の ID タイプが最初に削除されます。 現在、ID グラフには、グラフあたり 150 個の ID という制限があり、この制限に達すると、グラフは更新されなくなります。 実稼働用サンドボックスに次の情報が含まれている場合は、アカウント担当者に連絡して、ID タイプの変更をリクエストしてください。 <ul><li>ユーザー識別子（CRM ID など）を cookie/デバイス id タイプとして設定するカスタム名前空間。</li><li>cookie/device 識別子がクロスデバイス id タイプとして設定されるカスタム名前空間。</li></ul> Adobeエンジニアリングがこれらのリクエストを手動で処理します。 詳しくは、 [ID サービスデータのガードレール](../../identity-service/guardrails.md). |

詳しくは、 [ID サービスの概要](../../identity-service/home.md).

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] では、次に保存されているデータをセグメント化できます。 [!DNL Experience Platform] オーディエンスに組み込まれた個人（顧客、見込み客、ユーザー、組織など）に関連する オーディエンスは、セグメント定義や、 [!DNL Real-Time Customer Profile] データ。 これらのオーディエンスは、 [!DNL Platform]を使用し、任意のAdobeソリューションから簡単にアクセスできます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 類似オーディエンス（限定的に利用可能） | 類似オーディエンスは、機械学習ベースのインサイトを活用して、価値の高い顧客を特定しマーケティングキャンペーンでターゲット設定し、各オーディエンスに関するインテリジェントなインサイトを提供します。 類似オーディエンスを使用すると、パフォーマンスの高いオーディエンスに類似した顧客をターゲットにしたり、以前にコンバージョンされたオーディエンスに類似した顧客をターゲットにしたりする、拡張オーディエンスを作成できます。 類似オーディエンスの詳細については、 [類似オーディエンスの概要](../../segmentation/ui/lookalike-audiences.md). |

{style="table-layout:auto"}

詳しくは、 [セグメント化の概要](../../segmentation/home.md).

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| の一般公開 [!DNL SugarCRM] | [!DNL SugarCRM] ソースが使用できるようになりました。 [!DNL SugarCRM Accounts & Contacts] および [!DNL SugarCRM Events] ソースを使用して、[!DNL SugarCRM] アカウントから Experience Platform にデータを取り込みます。詳しくは、[[!DNL SugarCRM] 概要](../../sources/connectors/crm/sugarcrm.md)を参照してください。 |
| UI でのソースデータフローのオンデマンド取り込みのサポート | UI で、既存のソースのデータフローに対して、フロー実行をオンデマンドで作成できるようになりました。 詳しくは、 [UI を使用したソースに対するオンデマンドフロー実行の作成](../../sources/tutorials/ui/on-demand-ingestion.md). |
| 新規のサポート `correlationID` Adobe Analyticsのフィールド | The `_experience.decisioning.propositions.scopeDetails.correlationID` フィールドがAdobe Analyticsソースコネクタスキーマで使用できるようになりました。 このフィールドは、A4T 分類のサポートに使用され、2023 年 9 月以降に入力されます。 |

{style="table-layout:auto"}

詳しくは、 [ソースの概要](../../sources/home.md).
