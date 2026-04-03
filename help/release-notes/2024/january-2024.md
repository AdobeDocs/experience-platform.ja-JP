---
title: Adobe Experience Platform リリースノート（2024年1月）
description: Adobe Experience Platform の 2024年1月のリリースノートです。
exl-id: d4b3c5b2-3adb-41fd-91ad-f4c0f21d2325
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '1649'
ht-degree: 37%

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

[!UICONTROL Use Case Playbooks]機能は、すべてのReal-Time CDPおよびAdobe Journey Optimizerのお客様が一般に利用できるようになりました。 [!UICONTROL Use Case Playbooks]は、Real-Time Customer Data PlatformまたはAdobe Journey Optimizerを使い始める際の課題を解決できるように設計されています。 どこから始めればよいのか、望ましいユースケースに適したアセットを作成する方法がわからない場合は、ユースケースプレイブックを参考に、様々なアセットを作成してテストし、準備ができたら本番環境にインポートすることができます。

[!UICONTROL Use Case Playbooks]を使い始めるには、次のドキュメントページを参照してください。

- [概要ページ ](/help/use-case-playbooks/playbooks/overview.md)を読んで、目的、可用性に関する情報を理解し、プレイブックの機能を検出からインスタンスの作成、生成されたアセットの他のサンドボックス環境への読み込みまで、エンドツーエンドのデモをご確認ください。
- 利用可能なすべての[ プレイブック ](/help/use-case-playbooks/playbooks/playbooks-list.md)のリストを、製品別にグループ化します（Real-Time CDPまたはJourney Optimizer）
- プレイブックと、プレイブックによって生成されたアセットを使用するために必要なすべての[権限](/help/use-case-playbooks/playbooks/get-started.md#grant-your-team-the-required-access-permissions)に関する情報を取得します。
- 生成されたアセットを他のサンドボックス環境にコピーできる[ データ認識機能](/help/use-case-playbooks/playbooks/data-awareness.md)について説明します
- ユースケースプレイブックを使用する際にエラーや問題が発生した場合は、[ トラブルシューティングのヒント ](/help/use-case-playbooks/playbooks/troubleshooting.md)を入手してください。

## 属性ベースのアクセス制御 {#abac}

属性ベースのアクセス制御は、プライバシーを重視するブランドが、ユーザーアクセスをより柔軟に管理できるようにする、Adobe Experience Platform の機能です。ユーザーの役割に、スキーマフィールドやセグメントなどの個々のオブジェクトを割り当てることができます。 この機能を使用すると、組織内の特定のExperience Platform ユーザーに対する個々のオブジェクトへのアクセス権を付与または取り消すことができます。

属性ベースのアクセス制御により、Experience Platformの管理者は、あらゆるワークフローとリソースをまたいで、ユーザーによる個人情報（SPD）、個人情報（PII）、その他のカスタマイズされた種類のデータへのアクセスを制御できます。 管理者は、特定のフィールドと、それらのフィールドに対応するデータにのみアクセスできるユーザーの役割を定義できます。

**新規ドキュメントまたは更新されたドキュメント**

| ドキュメントの更新 | 説明 |
| --- | --- |
| 属性ベースのアクセス制御用にドキュメント化された新しいAPI エンドポイント | [ アクセス制御API リファレンス ドキュメント ](https://developer.adobe.com/experience-platform-apis/references/access-control/)に、属性ベースのアクセス制御APIの役割、ポリシー、製品エンドポイントが含まれるようになりました。 これらのエンドポイントを使用して、指定したサンドボックス内の特定のリソースに対するユーザーの関連する役割、ポリシー、製品を取得できます。 |

{style="table-layout:auto"}

属性ベースのアクセス制御の詳細については、[属性ベースのアクセス制御の概要](../../access-control/abac/overview.md)を参照してください。属性ベースのアクセス制御ワークフローの包括的なガイドについては、[属性ベースのアクセス制御エンドツーエンドガイド](../../access-control/abac/end-to-end-guide.md)を参照してください。

## データ準備 {#data-prep}

データ準備を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換および検証を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しいマッパー関数 | <ul><li>`object_to_map`: `object_to_map`関数を使用して、マップ データ型を作成します。 この関数は、複数の異なる構文をサポートしています。 詳しくは、階層の[関数 – オブジェクト ](../../data-prep/functions.md#objects)に関するガイドを参照してください。 </li><li>`to_map`: `to_map`関数を使用して、オブジェクトを使用して、特定のフィールド名と値のペアを持つマップを作成します。 詳しくは、階層の[関数 – マップ ](../../data-prep/functions.md#map)に関するガイドを参照してください。 </li><li>`array_to_map`: `array_to_map`関数を使用して、オブジェクト配列を使用して、特定のフィールド名と値のペアを持つマップを作成します。 詳しくは、階層の[関数 – マップ ](../../data-prep/functions.md#map)に関するガイドを参照してください。 |

{style="table-layout:auto"}

データ準備について詳しくは、[ データ準備の概要](../../data-prep/home.md)を参照してください。

## ダッシュボード {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを確認できる複数のダッシュボードを提供しています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| SQL を表示 | 「SQLを表示」切替スイッチを使用して、プロファイル、オーディエンス、宛先、カスタマイズされたインサイトの背後にあるSQLを表示し、クエリエディターを使用してオンデマンドでクエリを実行できるようになりました。 Adobe Real-Time CDPのインサイトを強化するSQLにアクセスすることで、データモデル分析の背後にあるロジックを理解できます。 この透明性により、Adobe Real-Time CDP データをよりアクセスしやすく、理解しやすく、意思決定に影響を与えることができます。<br>40以上の既存のインサイトのSQLからインスピレーションを得て、ビジネスニーズに基づいてExperience Platform データから独自のインサイトを導き出す新しいクエリを作成します。 SQLは、Experience League ドキュメントの[ プロファイル ](../../dashboards/insights/profiles.md)、[ オーディエンス ](../../dashboards/insights/audiences.md)、および[宛先](../../dashboards/insights/destinations.md) インサイトでも使用できます。 これらのドキュメントでは、標準的なインサイトで回答できるビジネスのユースケースを取り上げます。 詳しくは、[insight SQLの表示](../../dashboards/view-sql.md)に関するガイドを参照してください。 |

{style="table-layout:auto"}

アクセス権限の付与方法やカスタムウィジェットの作成方法など、ダッシュボードの詳細については、まず[ダッシュボードの概要](../../dashboards/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先** {#new-destinations}

| 宛先 | 説明 |
| ----------- | ----------- |
| [公開の接続](../../destinations/catalog/advertising/pubmatic.md) | この宛先を使用して、オーディエンスデータを[!DNL PubMatic Connect] プラットフォームに送信します。 |

{style="table-layout:auto"}

**新しい機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| ----------- | ----------- |
| Amazon S3宛先の新しい&#x200B;**想定される役割**&#x200B;認証タイプ | アカウントキーと秘密鍵をExperience Platformと共有したくない場合は、Experience PlatformをAmazon S3 バケットに接続する際に、新しい想定ロール認証タイプを使用します。 新しい認証方法について詳しくは、Amazon S3 ドキュメントの[認証セクション ](/help/destinations/catalog/cloud-storage/amazon-s3.md#assumed-role-authentication)を参照してください。 |

{style="table-layout:auto"}

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## ID サービス {#identity-service}

Adobe Experience Platform ID サービスを利用すると、デバイスやシステム間で ID を橋渡しすることで、顧客とその行動を包括的に把握し、インパクトのある個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**新規ドキュメントまたは更新されたドキュメント**

| ドキュメントの更新 | 説明 |
| --- | --- |
| ドキュメントの再構築 | Identity Service ドキュメントが再構築され、Identity Service内の概念のプレゼンテーションと明瞭性が向上しました。<ul><li>拡張された用語ガイド、一般的なカスタマージャーニーの詳細を示す使用例、Identity ServiceがIDをリンクする方法の内訳、Identity ServiceがExperience Platform エコシステム内で行う役割の概要については、[Identity Serviceの概要ページ ](../../identity-service/home.md)をご覧ください。</li><li>ID サービスとリアルタイム顧客プロファイル [の関係を理解する](../../identity-service/identity-and-profile.md)に関するガイドを参照して、2つのサービスの連携の仕組みと、目的、プロセス、入力、出力の違いについて詳しく説明してください。</li><li>異なるシナリオやタイムスタンプを指定した場合のID グラフの動作の説明と視覚化については、[ID サービス リンク ロジック ガイド ](../../identity-service/features/identity-linking-logic.md)を参照してください。</li></ul> |

{style="table-layout:auto"}

ID サービスについて詳しくは、[ID サービスの概要](../../identity-service/home.md)を参照してください。

## Real-Time Customer Data Platform {#rtcdp}

Experience Platform 上に構築された Real-time Customer Data Platform（[!DNL Real-Time CDP]）は、企業が既知および未知のデータを統合し、カスタマージャーニーを通じてインテリジェントな意思決定により顧客プロファイルをアクティブ化するのに役立ちます。[!DNL Real-Time CDP] では、複数のエンタープライズデータソースを組み合わせて、リアルタイムで顧客プロファイルを作成します。これらのプロファイルから構築されたセグメントをダウンストリームの宛先に送信し、すべてのチャネルとデバイスにわたって 1 対 1 のパーソナライズされた顧客体験を提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [Real-Time CDP ホームページ ](https://experience.adobe.com)の更新 | <ul><li>**プロファイルウィジェット**: プロファイルウィジェットを使用して、プロファイル概要ページに移動し、組織のプロファイル指標を表示できるようになりました。</li><li>**プロファイル指標カード**: ホームページ ダッシュボードのプロファイル指標カードに、それぞれの結合ポリシーに応じて、組織内のプロファイルの合計数が表示されるようになりました。</li><li>**スキーマウィジェット**: スキーマウィジェットを使用して、UIのスキーマ作成ワークフローに移動できるようになりました。</li></ul> |

{style="table-layout:auto"}

**新規ドキュメントまたは更新されたドキュメント**

| ドキュメントの更新 | 説明 |
| --- | --- |
| 新しいReal-Time CDP ドキュメントホームページ | 製品、ガードレール、サンプルユースケースなどの使い方を一目で確認するには、[new Real-Time CDPのドキュメントのホームページ ](/help/rtcdp/home.md)をご覧ください。 |
| Adobe Real-Time CDPのユースケースの概要 | Real-Time CDPで実現できるサンプルユースケースのコレクションについては、[新しいサンプルユースケースの概要ページ ](/help/rtcdp/use-case-guides/overview.md)を参照してください。 |

{style="table-layout:auto"}

Real-Time CDPについて詳しくは、[Real-Time CDPの概要](../../rtcdp/overview.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| プロファイルビューアのデフォルトダッシュボードカードのローカライズの改善 | デフォルトのプロファイルカードには、動的にローカライズされた名前が付けられます。 カスタムプロファイルカードは、引き続き編集可能なカスタム名を持つことができます。 |

{style="table-layout:auto"}

リアルタイム顧客プロファイルについて詳しくは、[ プロファイルの概要](../../profile/home.md)を参照してください

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 外部で生成されたオーディエンスのアップロード | 列の最大数が&#x200B;**25**&#x200B;に増加しました。 |
| セグメントビルダーの見積もり | 推定プロファイルと適格プロファイルがオーディエンスプロパティセクションに表示されるようになりました。 この変更について詳しくは、[ セグメントビルダーUI ガイド ](../../segmentation/ui/segment-builder.md)を参照してください。 |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} [!DNL Oracle NetSuite] ソース | ソースカタログの[!DNL Oracle NetSuite]統合機能を使用して、[[!DNL Oracle NetSuite Activities]](../../sources/tutorials/ui/create/marketing-automation/oracle-netsuite-activities.md)および[[!DNL Oracle NetSuite Entities]](../../sources/tutorials/ui/create/marketing-automation/oracle-netsuite-entities.md) アカウントからExperience Platformにデータを取り込みます。 |
| [!BADGE Beta]{type=Informative} [!DNL Braze Currents] ソース | ソースカタログの[[!DNL Braze Currents]](../../sources/tutorials/ui/create/marketing-automation/braze.md)統合を使用して、[!DNL Braze] アカウントからExperience Platformにデータを取り込みます。 |
| [!DNL Snowflake] バッチソースのキーペア認証のサポート | バッチデータの新しい[!DNL Snowflake] アカウントを作成する際に、キーペア認証を使用できるようになりました。 詳しくは、[APIを使用した [!DNL Snowflake]  アカウントの作成](../../sources/tutorials/api/create/databases/snowflake.md)に関するガイドまたは[UIを使用した [!DNL Snowflake]  アカウントの作成](../../sources/tutorials/ui/create/databases/snowflake.md)に関するガイドを参照してください。 |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
