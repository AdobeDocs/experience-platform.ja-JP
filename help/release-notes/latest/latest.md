---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の 2024年1月のリリースノートです。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: fc7183cbc1ca3e27999d0ddd64c83ee19ccb1200
workflow-type: tm+mt
source-wordcount: '1036'
ht-degree: 38%

---

# Adobe Experience Platform リリースノート

**リリース日：2024年1月30日**

Adobe Experience Platform の新機能：

- [ユースケースプレイブック](#use-case-playbooks)

Experience Platformの既存の機能の更新：

- [ダッシュボード](#dashboards)
- [データ準備](#data-prep)
- [宛先](#destinations)
- [Real-Time Customer Data Platform](#rtcdp)
- [リアルタイム顧客プロファイル](#profile)
- [ソース](#sources)

## ユースケースプレイブック {#use-case-playbooks}

The [!UICONTROL ユースケースプレイブック] の機能は、すべてのReal-Time CDPおよびAdobe Journey Optimizerのユーザーに対して一般に利用できるようになりました。 [!UICONTROL ユースケースプレイブック] は、Real-time Customer Data PlatformまたはAdobe Journey Optimizerを使い始める際に、課題の克服に役立つように設計されています。 開始する場所や目的の使用例に適したアセットの作成方法が不明な場合は、ユースケースプレイブックにインスピレーションが得られ、準備が整ったら実稼動環境にテストして読み込むための様々なアセットを作成できます。

を使い始めるには、以下を実行します。 [!UICONTROL ユースケースプレイブック]を参照し、次のドキュメントページをお読みください。

- 詳しくは、 [概要ページ](/help/use-case-playbooks/playbooks/overview.md) 目的、可用性情報を理解し、再生ブックの検出からインスタンスの作成、生成されたアセットの他のサンドボックス環境への読み込みに至るまで、どのように機能するかをエンドツーエンドでデモンストレーションできます。
- すべての [利用可能なプレイブック](/help/use-case-playbooks/playbooks/playbooks-list.md)、製品別にグループ化 (Real-Time CDPまたはJourney Optimizer)
- すべての [必要な権限](/help/use-case-playbooks/playbooks/get-started.md#grant-your-team-the-required-access-permissions) プレイブックとプレイブックが生成したアセットを使用する。
- について [データ認識機能](/help/use-case-playbooks/playbooks/data-awareness.md) ：生成されたアセットを他のサンドボックス環境にコピーできます。
- 取得 [トラブルシューティングのヒント](/help/use-case-playbooks/playbooks/troubleshooting.md) ユースケースプレイブックの使用に関するエラーや問題が発生した場合。

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
| SQL を表示 | プロファイル、オーディエンス、宛先、カスタマイズされたインサイトの背後に SQL を表示し、クエリエディターを使用してクエリをオンデマンドで実行できるようになりました。 40 を超える既存のインサイトの SQL からインスピレーションを得て、ビジネスニーズに基づいて Platform データから一意のインサイトを引き出す新しいクエリを作成します。 詳しくは、 [insight SQL の表示](../../dashboards/view-sql.md). |

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

## Real-Time Customer Data Platform {#rtcdp}

Experience Platform 上に構築された Real-time Customer Data Platform（[!DNL Real-Time CDP]）は、企業が既知および未知のデータを統合し、カスタマージャーニーを通じてインテリジェントな意思決定により顧客プロファイルをアクティブ化するのに役立ちます。[!DNL Real-Time CDP] では、複数のエンタープライズデータソースを組み合わせて、リアルタイムで顧客プロファイルを作成します。これらのプロファイルから構築されたセグメントをダウンストリームの宛先に送信し、すべてのチャネルとデバイスにわたって 1 対 1 のパーソナライズされた顧客体験を提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| の更新 [Real-Time CDPホームページ](https://experience.adobe.com) | <ul><li>**プロファイルウィジェット**：プロファイルウィジェットを使用してプロファイルの概要ページに移動し、組織のプロファイル指標を表示できるようになりました。</li><li>**「プロファイル指標」カード**：ホームページダッシュボードの「プロファイル指標」カードに、それぞれの結合ポリシーに応じて、組織内のプロファイルの合計数が表示されるようになりました。</li><li>**スキーマウィジェット**：スキーマウィジェットを使用して、UI でスキーマ作成ワークフローに移動できるようになりました。</li></ul> |

Real-Time CDPの詳細については、 [Real-Time CDPの概要](../../rtcdp/overview.md).

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| プロファイルビューアのデフォルトのダッシュボードカードのローカライズの改善 | デフォルトのプロファイルカードで、動的にローカライズされた名前が使用されるようになりました。 カスタムプロファイルカードには、引き続き編集可能なカスタム名を付けることができます。 |

{style="table-layout:auto"}

リアルタイム顧客プロファイルの詳細については、 [プロファイルの概要](../../profile/home.md)

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