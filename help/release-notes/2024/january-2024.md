---
title: Adobe Experience Platform リリースノート（2024年1月）
description: Adobe Experience Platform の 2024年1月のリリースノートです。
source-git-commit: c6d471d7bb8cb9d5e376cc49c9c89c39e663d7f9
workflow-type: tm+mt
source-wordcount: '1655'
ht-degree: 39%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年1月30日**

Adobe Experience Platform の新機能：

- [ユースケースプレイブック](#use-case-playbooks)

Experience Platformの既存の機能の更新：

- [属性ベースのアクセス制御](#abac)
- [データ準備](#data-prep)
- [ダッシュボード](#dashboards)
- [宛先](#destinations)
- [ID サービス](#identity-service)
- [Real-Time Customer Data Platform](#rtcdp)
- [リアルタイム顧客プロファイル](#profile)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## ユースケースプレイブック {#use-case-playbooks}

The [!UICONTROL ユースケースプレイブック] の機能は、すべてのReal-Time CDPおよびAdobe Journey Optimizerのユーザーに対して一般に利用できるようになりました。 [!UICONTROL ユースケースプレイブック] は、Real-time Customer Data PlatformまたはAdobe Journey Optimizerを使い始める際に、課題の克服に役立つように設計されています。 開始する場所や目的の使用例に適したアセットの作成方法が不明な場合は、ユースケースプレイブックにインスピレーションが得られ、準備が整ったら実稼動環境にテストして読み込むための様々なアセットを作成できます。

を使い始めるには、以下を実行します。 [!UICONTROL ユースケースプレイブック]を参照し、次のドキュメントページをお読みください。

- 詳しくは、 [概要ページ](/help/use-case-playbooks/playbooks/overview.md) 目的、可用性情報を理解し、再生ブックの検出からインスタンスの作成、生成されたアセットの他のサンドボックス環境への読み込みに至るまで、どのように機能するかをエンドツーエンドでデモンストレーションできます。
- すべての [利用可能なプレイブック](/help/use-case-playbooks/playbooks/playbooks-list.md)、製品別にグループ化 (Real-Time CDPまたはJourney Optimizer)
- すべての [必要な権限](/help/use-case-playbooks/playbooks/get-started.md#grant-your-team-the-required-access-permissions) プレイブックとプレイブックが生成したアセットを使用する。
- について [データ認識機能](/help/use-case-playbooks/playbooks/data-awareness.md) ：生成されたアセットを他のサンドボックス環境にコピーできます。
- 取得 [トラブルシューティングのヒント](/help/use-case-playbooks/playbooks/troubleshooting.md) ユースケースプレイブックの使用に関するエラーや問題が発生した場合。

## 属性ベースのアクセス制御 {#abac}

属性ベースのアクセス制御は、プライバシーを重視するブランドが、ユーザーアクセスをより柔軟に管理できるようにする、Adobe Experience Platform の機能です。ユーザーの役割に、スキーマフィールドやセグメントなどの個々のオブジェクトを割り当てることができます。 この機能を使用すると、組織内の特定の Platform ユーザーに対する個々のオブジェクトへのアクセスを許可または取り消すことができます。

属性ベースのアクセス制御により、組織の管理者は、すべての Platform ワークフローとリソースにわたって、機密性の高い個人データ（SPD）、個人を特定できる情報（PII）、およびその他のカスタマイズされた種類のデータへのユーザーのアクセスを制御できます。管理者は、特定のフィールドと、それらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義できます。

**新規ドキュメントまたは更新されたドキュメント**

| ドキュメントの更新 | 説明 |
| --- | --- |
| 属性ベースのアクセス制御用に新しい API エンドポイントがドキュメント化されました。 | The [アクセス制御 API リファレンスドキュメント](https://developer.adobe.com/experience-platform-apis/references/access-control/) には、属性ベースのアクセス制御 API の役割、ポリシー、製品エンドポイントが含まれるようになりました。 これらのエンドポイントは、指定したサンドボックス内の特定のリソースに関するユーザーの関連する役割、ポリシーおよび製品を取得するために使用できます。 |

{style="table-layout:auto"}

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](../../access-control/abac/overview.md)を参照してください。属性ベースのアクセス制御ワークフローの包括的なガイドについては、[属性ベースのアクセス制御エンドツーエンドガイド](../../access-control/abac/end-to-end-guide.md)を参照してください。

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しいマッパー関数 | <ul><li>`object_to_map`：を使用します。 `object_to_map` 関数を使用して、マップデータ型を作成します。 この関数は、複数の異なる構文をサポートします。 詳しくは、 [階層の関数 — オブジェクト](../../data-prep/functions.md#objects). </li><li>`to_map`：を使用します。 `to_map` 関数を使用して、指定されたフィールド名と値のペアを持つマップをオブジェクトを使用して作成します。 詳しくは、 [階層の関数 — マップ](../../data-prep/functions.md#map). </li><li>`array_to_map`：を使用します。 `array_to_map` 関数を使用して、オブジェクト配列を使用して、指定されたフィールド名と値のペアを持つマップを作成します。 詳しくは、 [階層の関数 — マップ](../../data-prep/functions.md#map). |

{style="table-layout:auto"}

Data Prep の詳細については、 [データ準備の概要](../../data-prep/home.md).

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを確認できる複数のダッシュボードを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| SQL を表示 | SQL を表示の切り替えで、プロファイル、オーディエンス、宛先、カスタマイズされたインサイトの背後に SQL を表示し、クエリエディターを使用してクエリをオンデマンドで実行できるようになりました。 Real-time Customer Data Platformの洞察を強化する SQL にアクセスすると、データモデルの分析の背後にあるロジックを理解できます。 この透明性により、Adobeのリアルタイム CDP データに対するアクセス性と理解性が向上し、意思決定にとっての影響が大きくなります。<br>40 を超える既存のインサイトの SQL からインスピレーションを得て、ビジネスニーズに基づいて Platform データから一意のインサイトを引き出す新しいクエリを作成します。 SQL は、 [プロファイル](../../dashboards/insights/profiles.md), [オーディエンス](../../dashboards/insights/audiences.md)、および [宛先](../../dashboards/insights/destinations.md) Experience Leagueドキュメントのインサイト。 これらのドキュメントでは、標準的なインサイトを使用して回答できるビジネスユースケースを重点的に説明します。 詳しくは、 [insight SQL の表示](../../dashboards/view-sql.md). |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先** {#new-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| [公開接続](../../destinations/catalog/advertising/pubmatic.md) | この宛先を使用して、オーディエンスデータをに送信します。 [!DNL PubMatic Connect] プラットフォーム。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| 新規 **仮の役割** Amazon S3 の宛先の認証タイプ | アカウントキーと秘密鍵をExperience Platformと共有しない場合は、Experience PlatformをAmazon S3 バケットに接続する際に、新しい想定される役割認証タイプを使用します。 新しい認証方法について詳しくは、 [認証セクション](/help/destinations/catalog/cloud-storage/amazon-s3.md#assumed-role-authentication) Amazon S3 ドキュメントの |

{style="table-layout:auto"}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**新規ドキュメントまたは更新されたドキュメント**

| ドキュメントの更新 | 説明 |
| --- | --- |
| ドキュメントの再構築 | ID サービスのドキュメントが再構成され、ID サービス内の概念の表示と明確性が向上しました。<ul><li>次にアクセス： [ID サービスの概要ページ](../../identity-service/home.md) 用語ガイドの拡張、一般的なカスタマージャーニーの詳細、ID サービスによる id のリンク方法の分類、Experience Platformエコシステム内での ID サービスの役割の概要を説明する使用例です。</li><li>次のガイドを読む： [ID サービスとリアルタイム顧客プロファイルの関係について](../../identity-service/identity-and-profile.md) 2 つのサービスの連携の仕組みと、目的、プロセス、入力、出力の違いに関する詳細な概要。</li><li>詳しくは、 [ID サービスリンクロジックガイド](../../identity-service/features/identity-linking-logic.md) を参照してください。</li></ul> |

{style="table-layout:auto"}

ID サービスの詳細については、 [ID サービスの概要](../../identity-service/home.md).

## Real-Time Customer Data Platform {#rtcdp}

Experience Platform 上に構築された Real-time Customer Data Platform（[!DNL Real-Time CDP]）は、企業が既知および未知のデータを統合し、カスタマージャーニーを通じてインテリジェントな意思決定により顧客プロファイルをアクティブ化するのに役立ちます。[!DNL Real-Time CDP] では、複数のエンタープライズデータソースを組み合わせて、リアルタイムで顧客プロファイルを作成します。これらのプロファイルから構築されたセグメントをダウンストリームの宛先に送信し、すべてのチャネルとデバイスにわたって 1 対 1 のパーソナライズされた顧客体験を提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| の更新 [Real-Time CDPホームページ](https://experience.adobe.com) | <ul><li>**プロファイルウィジェット**：プロファイルウィジェットを使用してプロファイルの概要ページに移動し、組織のプロファイル指標を表示できるようになりました。</li><li>**「プロファイル指標」カード**：ホームページダッシュボードの「プロファイル指標」カードに、それぞれの結合ポリシーに応じて、組織内のプロファイルの合計数が表示されるようになりました。</li><li>**スキーマウィジェット**：スキーマウィジェットを使用して、UI でスキーマ作成ワークフローに移動できるようになりました。</li></ul> |

{style="table-layout:auto"}

**新規ドキュメントまたは更新されたドキュメント**

| ドキュメントの更新 | 説明 |
| --- | --- |
| 新しいReal-Time CDPドキュメントのホームページ | 次にアクセス： [新しいReal-Time CDPドキュメントのホームページ](/help/rtcdp/home.md) を参照して、製品、ガードレール、サンプルユースケースの使用を開始する方法などの概要情報を確認できます。 |
| サンプルReal-Time CDPの使用例の概要 | 次にアクセス： [新しいサンプルユースケースの概要ページ](/help/rtcdp/use-case-guides/overview.md) を参照してください。 |

{style="table-layout:auto"}

Real-Time CDPの詳細については、 [Real-Time CDPの概要](../../rtcdp/overview.md).

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| プロファイルビューアのデフォルトのダッシュボードカードのローカライズの改善 | デフォルトのプロファイルカードで、動的にローカライズされた名前が使用されるようになりました。 カスタムプロファイルカードには、引き続き編集可能なカスタム名を付けることができます。 |

{style="table-layout:auto"}

リアルタイム顧客プロファイルの詳細については、 [プロファイルの概要](../../profile/home.md)

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 外部で生成されたオーディエンスのアップロード | 列の最大数が **25**. |
| セグメントビルダーの予測 | 推定および認定済みプロファイルが、オーディエンスプロパティセクション内に表示されるようになりました。 この変更の詳細については、 [セグメントビルダー UI ガイド](../../segmentation/ui/segment-builder.md). |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative}[!DNL Oracle NetSuite] ソース | 以下を使用します。 [!DNL Oracle NetSuite] ソースカタログ内の統合を使用して [[!DNL Oracle NetSuite Activities]](../../sources/tutorials/ui/create/marketing-automation/oracle-netsuite-activities.md) および [[!DNL Oracle NetSuite Entities]](../../sources/tutorials/ui/create/marketing-automation/oracle-netsuite-entities.md) アカウントからExperience Platformへ。 |
| [!BADGE Beta]{type=Informative}[!DNL Braze Currents] ソース | 以下を使用します。 [[!DNL Braze Currents]](../../sources/tutorials/ui/create/marketing-automation/braze.md) ソースカタログでの統合を参照してください。 [!DNL Braze] アカウントからExperience Platformへ。 |
| のキーペア認証のサポート [!DNL Snowflake] バッチソース | 新しい [!DNL Snowflake] バッチデータのアカウント。 詳しくは、 [作成 [!DNL Snowflake] API を使用するアカウント](../../sources/tutorials/api/create/databases/snowflake.md) または [作成 [!DNL Snowflake] UI を使用するアカウント](../../sources/tutorials/ui/create/databases/snowflake.md). |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。