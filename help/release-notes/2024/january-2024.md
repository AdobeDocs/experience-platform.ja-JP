---
title: Adobe Experience Platform リリースノート（2024年1月）
description: Adobe Experience Platform の 2024年1月のリリースノートです。
exl-id: d4b3c5b2-3adb-41fd-91ad-f4c0f21d2325
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '1659'
ht-degree: 40%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年1月30日**

Adobe Experience Platform の新機能：

- [ユースケースプレイブック](#use-case-playbooks)

Experience Platformの既存の機能に対するアップデート：

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

[!UICONTROL  ユースケースプレイブック ] 機能が、すべてのReal-Time CDPおよびAdobe Journey Optimizerのお客様で一般公開されました。 [!UICONTROL  ユースケースプレイブック ] は、Real-time Customer Data PlatformまたはAdobe Journey Optimizerの使用を開始する際の課題を克服するのを支援するように設計されています。 どこから始めればよいのか、目的のユースケースに合った適切なアセットの作成方法がわからない場合は、ユースケースプレイブックを使用すると、インスピレーションを得て様々なアセットを作成し、準備ができたときにテストして実稼動環境に読み込むことができます。

[!UICONTROL  ユースケースプレイブック ] を使い始めるには、次のドキュメントページを参照してください。

- [ 概要ページ ](/help/use-case-playbooks/playbooks/overview.md) を読んで、目的や可用性に関する情報を理解し、プレイブックが検出からインスタンスの作成、生成されたアセットの他のサンドボックス環境への読み込みにどのように機能するかを示すエンドツーエンドのデモを取得します。
- 製品別にグループ化されたすべての [ 使用可能なプレイブック ](/help/use-case-playbooks/playbooks/playbooks-list.md) のリストを取得します（Real-Time CDPまたはJourney Optimizer）
- プレイブックとプレイブックで生成されたアセットを使用するためのすべての [ 必要な権限 ](/help/use-case-playbooks/playbooks/get-started.md#grant-your-team-the-required-access-permissions) に関する情報を取得します。
- 生成されたアセットを他のサンドボックス環境にコピーできる ](/help/use-case-playbooks/playbooks/data-awareness.md) データ認識機能 [ について説明します
- ユースケースプレイブックの使用でエラーや問題が発生した場合は、[ トラブルシューティングのヒント ](/help/use-case-playbooks/playbooks/troubleshooting.md) を入手します。

## 属性ベースのアクセス制御 {#abac}

属性ベースのアクセス制御は、プライバシーを重視するブランドが、ユーザーアクセスをより柔軟に管理できるようにする、Adobe Experience Platform の機能です。ユーザーの役割に、スキーマフィールドやセグメントなどの個々のオブジェクトを割り当てることができます。 この機能を使用すると、組織内の特定の Platform ユーザーに対する個々のオブジェクトへのアクセスを許可または取り消すことができます。

属性ベースのアクセス制御により、組織の管理者は、すべての Platform ワークフローとリソースにわたって、機密性の高い個人データ（SPD）、個人を特定できる情報（PII）、およびその他のカスタマイズされた種類のデータへのユーザーのアクセスを制御できます。管理者は、特定のフィールドと、それらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義できます。

**新規ドキュメントまたは更新されたドキュメント**

| ドキュメントの更新 | 説明 |
| --- | --- |
| 属性ベースのアクセス制御についてドキュメントに記載された新しい API エンドポイント | [ アクセス制御 API リファレンスドキュメント ](https://developer.adobe.com/experience-platform-apis/references/access-control/) には、属性ベースのアクセス制御 API の役割、ポリシー、製品エンドポイントが含まれるようになりました。 これらのエンドポイントを使用して、指定されたサンドボックス内の特定のリソースでユーザーの関連する役割、ポリシー、製品を取得できます。 |

{style="table-layout:auto"}

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](../../access-control/abac/overview.md)を参照してください。属性ベースのアクセス制御ワークフローの包括的なガイドについては、[属性ベースのアクセス制御エンドツーエンドガイド](../../access-control/abac/end-to-end-guide.md)を参照してください。

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しいマッパー関数 | <ul><li>`object_to_map`: `object_to_map` 関数を使用して、マップ データ タイプを作成します。 この関数は、複数の異なる構文をサポートします。 詳しくは、[ 階層の関数 – オブジェクト ](../../data-prep/functions.md#objects) に関するガイドを参照してください。 </li><li>`to_map`: `to_map` 関数を使用して、オブジェクトを使用して指定されたフィールド名と値のペアでマップを作成します。 詳しくは、[ 階層の関数 – マップ ](../../data-prep/functions.md#map) に関するガイドを参照してください。 </li><li>`array_to_map`: `array_to_map` 関数を使用すると、オブジェクト配列を使用して、指定されたフィールド名と値のペアでマップを作成できます。 詳しくは、[ 階層の関数 – マップ ](../../data-prep/functions.md#map) に関するガイドを参照してください。 |

{style="table-layout:auto"}

データ準備について詳しくは、[ データ準備の概要 ](../../data-prep/home.md) を参照してください。

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを確認できる複数のダッシュボードを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| SQL を表示 | 「SQL を表示」切替スイッチを使用して、プロファイル、オーディエンス、宛先およびカスタマイズされたインサイトの背後にある SQL を表示し、クエリエディターを使用してオンデマンドでクエリを実行できるようになりました。 Real-time Customer Data Platformのインサイトを強化する SQL にアクセスすると、データモデルの分析の背後にあるロジックを理解するのに役立ちます。 この透明性により、Adobeの Real-time CDP データがよりアクセスしやすく、理解しやすく、意思決定に影響を与えやすくなります。<br>40 を超える既存のインサイトの SQL からインスピレーションを得て、ビジネスニーズに基づいて Platform データから独自のインサイトを導き出す新しいクエリを作成します。 また、Experience Leagueドキュメントでは、[ プロファイル ](../../dashboards/insights/profiles.md)、[ オーディエンス ](../../dashboards/insights/audiences.md) および [ 宛先 ](../../dashboards/insights/destinations.md) インサイトに対して SQL を使用することもできます。 これらのドキュメントでは、標準のインサイトを使用して回答できるビジネスユースケースをハイライト表示しています。 詳しくは、[ インサイト SQL の表示 ](../../dashboards/view-sql.md) に関するガイドを参照してください。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先** {#new-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| [ パブリッシュ接続 ](../../destinations/catalog/advertising/pubmatic.md) | オーディエンスデータを [!DNL PubMatic Connect] プラットフォームに送信するには、この宛先を使用します。 |

{style="table-layout:auto"}

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| Amazon S3 の宛先の新しい **想定される役割** 認証タイプ | アカウントキーと秘密鍵をExperience Platformと共有しない場合は、Amazon S3 バケットにExperience Platformを接続する際に、新しく想定されるロール認証タイプを使用します。 新しい認証方法について詳しくは、Amazon S3 ドキュメントの [ 認証の節 ](/help/destinations/catalog/cloud-storage/amazon-s3.md#assumed-role-authentication) を参照してください。 |

{style="table-layout:auto"}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**新規ドキュメントまたは更新されたドキュメント**

| ドキュメントの更新 | 説明 |
| --- | --- |
| ドキュメントの再構築 | ID サービスに関するドキュメントが再構成され、ID サービス内の概念の表示方法と明確さが改善されました。<ul><li>一般的なカスタマージャーニーの詳細なユースケースの例、ID サービスが ID をリンクする方法の分類、Experience Platformエコシステム内で ID サービスが提供するロールの概要については、[ID サービスの概要ページ ](../../identity-service/home.md) を参照してください。</li><li>2 つのサービスの連携方法と目的、プロセス、入力、出力の違いについて詳しくは、[ID サービスとリアルタイム顧客プロファイルの関係について ](../../identity-service/identity-and-profile.md) に関するガイドを参照してください。</li><li>様々なシナリオやタイムスタンプでの ID グラフの動作を示す説明とビジュアライゼーションについては、[ID サービスリンクロジックガイド ](../../identity-service/features/identity-linking-logic.md) を参照してください。</li></ul> |

{style="table-layout:auto"}

ID サービスについて詳しくは、[ID サービスの概要 ](../../identity-service/home.md) を参照してください。

## Real-Time Customer Data Platform {#rtcdp}

Experience Platform 上に構築された Real-time Customer Data Platform（[!DNL Real-Time CDP]）は、企業が既知および未知のデータを統合し、カスタマージャーニーを通じてインテリジェントな意思決定により顧客プロファイルをアクティブ化するのに役立ちます。[!DNL Real-Time CDP] では、複数のエンタープライズデータソースを組み合わせて、リアルタイムで顧客プロファイルを作成します。これらのプロファイルから構築されたセグメントをダウンストリームの宛先に送信し、すべてのチャネルとデバイスにわたって 1 対 1 のパーソナライズされた顧客体験を提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [Real-Time CDP ホームページを更新 ](https://experience.adobe.com) | <ul><li>**プロファイルウィジェット**：プロファイルウィジェットを使用して、プロファイルの概要ページに移動し、組織のプロファイル指標を表示できるようになりました。</li><li>**プロファイル指標カード**：ホームページダッシュボードのプロファイル指標カードに、それぞれの結合ポリシーに応じて、組織内のプロファイルの合計数が表示されるようになりました。</li><li>**スキーマウィジェット**：スキーマウィジェットを使用して、UI のスキーマ作成ワークフローに移動できるようになりました。</li></ul> |

{style="table-layout:auto"}

**新規ドキュメントまたは更新されたドキュメント**

| ドキュメントの更新 | 説明 |
| --- | --- |
| 新しいReal-Time CDP ドキュメントのホームページ | 製品、ガードレール、サンプルのユースケースなどの概要については、[ 新しいReal-Time CDP ドキュメントのホームページ ](/help/rtcdp/home.md) を参照してください。 |
| Real-Time CDPのユースケースの概要 | 組織がReal-Time CDPで実現できるサンプルユースケースのコレクションについては、[ 新しいサンプルユースケースの概要ページ ](/help/rtcdp/use-case-guides/overview.md) を参照してください。 |

{style="table-layout:auto"}

Real-Time CDPについて詳しくは、[Real-Time CDPの概要 ](../../rtcdp/overview.md) を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| プロファイルビューアのデフォルトのダッシュボードカードのローカライズの改善 | デフォルトのプロファイルカードの名前が、動的にローカライズされるようになりました。 カスタムプロファイルカードには、編集可能なカスタム名を引き続き付けることができます。 |

{style="table-layout:auto"}

リアルタイム顧客プロファイルについて詳しくは、[ プロファイルの概要 ](../../profile/home.md) を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 外部で生成されたオーディエンスのアップロード | 列の最大数が **25** に増えました。 |
| セグメントビルダーの予測 | 見積もりプロファイルと認定プロファイルが、オーディエンスプロパティセクションに表示されるようになりました。 この変更について詳しくは、[ セグメントビルダー UI ガイド ](../../segmentation/ui/segment-builder.md) を参照してください。 |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} [!DNL Oracle NetSuite] ソース | ソースカタログの [!DNL Oracle NetSuite] 統合を使用すると、[[!DNL Oracle NetSuite Activities]](../../sources/tutorials/ui/create/marketing-automation/oracle-netsuite-activities.md) および [[!DNL Oracle NetSuite Entities]](../../sources/tutorials/ui/create/marketing-automation/oracle-netsuite-entities.md) アカウントからExperience Platformにデータを取り込むことができます。 |
| [!BADGE Beta]{type=Informative} [!DNL Braze Currents] ソース | ソースカタログの [[!DNL Braze Currents]](../../sources/tutorials/ui/create/marketing-automation/braze.md) 統合を使用して、[!DNL Braze] アカウントからExperience Platformにデータを取り込みます。 |
| バッチソースのキーペア認証 [!DNL Snowflake] サポート | バッチデータ用の新しい [!DNL Snowflake] アカウントを作成する際に、キーペア認証を使用できるようになりました。 詳しくは、[API を使用したアカウントの作成  [!DNL Snowflake]  または [UI を使用したアカウントの作成 ](../../sources/tutorials/api/create/databases/snowflake.md) に関するガイドを参照し  [!DNL Snowflake]  ください ](../../sources/tutorials/ui/create/databases/snowflake.md)。 |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
