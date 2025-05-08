---
title: Adobe Experience Platform リリースノート 2025年4月
description: Adobe Experience Platform の 2025年4月のリリースノート。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: a79efc5d64862850d17cff0fd0633c73fd08207d
workflow-type: tm+mt
source-wordcount: '2197'
ht-degree: 23%

---

# Adobe Experience Platform リリースノート

>[!TIP]
>
>その他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/releases/latest)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/ja/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2025年4月29日**

Adobe Experience Platformの既存の機能およびドキュメントのアップデート：

- [Experience League](#experience-league)
- [データ収集](#data-collection)
- [宛先](#destinations)
- [エクスペリエンスデータモデル](#xdm)
- [ID サービス](#identity)
- [クエリサービス](#query-service)
- [リアルタイム顧客プロファイル](#profile)
- [サンドボックス](#sandboxes)
- [ソース](#sources)
- [ユースケースプレイブック](#use-case-playbooks)

## Experience League {#experience-league}

Experience Leagueは、Adobe製品のスキルアップに役立つ包括的なラーニングプラットフォームです。 コース、ドキュメント、コミュニティページ、イベント、認定制度へのアクセスなど、様々なリソースが用意されています。

| 機能 | 説明 |
| --- | --- |
| パーソナライズされたホームページ | パーソナライズされたホームページに [Experience League](https://experienceleague.adobe.com/ja/home#) でアクセスしてカスタマイズします。 Adobeの資格情報を使用してログインし、上部のメニューで「**[!UICONTROL Experience League]**」を選択して、学習体験の最適化を開始します。 <ul><li>**ブックマーク**:[!UICONTROL &#x200B; ブックマーク &#x200B;] 機能を使用して、お気に入りのリソースを 1 か所に保存および収集します。 プレイリスト、記事、チュートリアルなど、様々なコンテンツを保存できます。</li><li>**ラーニングのカスタマイズ**：お客様のニーズに最適な役割、業界、商品、エクスペリエンスレベルでExperience League プロファイルを更新することで、ラーニングエクスペリエンスを強化できます。</li><li>**Recommendations**：最近のアクティビティに基づいて推奨された学習コンテンツを表示します。</li><li>**最近表示された項目**:「[!UICONTROL &#x200B; 最近表示された項目 &#x200B;] セクションを使用すると、ドキュメントやビデオなど、最近表示されたコンテンツにすばやく戻ることができます。</li><li>**学習リソース**:[!UICONTROL &#x200B; すべての学習リソース &#x200B;] パネルを使用して、チュートリアル、ドキュメント、コミュニティ、イベント、認定制度に移動します。</li><li>**新機能**:Experience Leagueの最新コンテンツのストリームについては、[!UICONTROL &#x200B; 新機能 &#x200B;] の節をご覧ください。</li><li>**オンデマンドで過去のイベントを視聴**:[!UICONTROL &#x200B; オンデマンドで過去のイベントを視聴 &#x200B;] セクションで、製品スポットライト、ユースケースおよびチュートリアルに関する過去のライブストリームを視聴します。</li></ul><br> ![Experience Leagueのパーソナライズされたホームページ。](../2025/assets/april/personalized-home-page.png "Experience Leagueのパーソナライズされたホームページ。"){width="250" align="center" zoomable="yes"} |

{style="table-layout:auto"}

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Adform] 拡張機能 | [!DNL Adform] のサーバーサイド拡張機能を使用すると、企業は ECID を使用してオフサイトでオーディエンスを簡単にリターゲティングできます。 このサーバーサイド拡張機能は、サードパーティ cookie や cookie 代替 ID には依存しません。 さらに、これは完全にサーバーサイドで行われるので、追加のピクセルや他のクライアントサイドの変更は必要ありません。 詳しくは、[Adform 拡張機能の概要 ](/help/tags/extensions/server/adform/overview.md) を参照してください。 |
| [!DNL Amazon] Web イベント API 拡張機能 | [!DNL Amazon] Conversions API 拡張機能を使用すると、広告主は web サイトとのやり取りを [!DNL Amazon] と直接共有でき、アトリビューション、データ信頼性およびキャンペーンの最適化が向上します。 この拡張機能はイベント転送をサポートしており、正確なレポートを実現する適切な重複排除を確保しながら、購入、買い物かごへの追加など、コンバージョンイベントを送信できます。 詳しくは、[Amazon拡張機能の概要 ](/help/tags/extensions/server/amazon/overview.md) を参照してください。 |

{style="table-layout:auto"}

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先** {#new-updated-destinations}

| 宛先 | 説明 |
| --- | --- |
| [Marketo Engage人物同期 ](/help/destinations/catalog/adobe/marketo-engage-person-sync.md) | Adobeは、id マップに複数のメールが存在する場合にお客様に影響を与える問題を修正するために、[!DNL Marketo Engage Person Sync] の宛先を更新しました。 |
| [ （V2） Pega CDH リアルタイムオーディエンス接続 ](/help/destinations/catalog/personalization/pega-v2.md) | Pega アカウントに複数の Pega Customer Decision Hub アプリケーションが設定されている場合は、Adobe Experience Platformの [!DNL (V2) Pega Customer Decision Hub Realtime Audience] 宛先を使用して、プロファイル属性とオーディエンスメンバーシップデータを Pega Customer Decision Hub に送信し、次善のアクションの意思決定を行います。 |

**新機能または更新された機能** {#destinations-new-updated-functionality}

| 機能 | 説明 |
| --- | --- |
| 完全ファイル書き出しの **毎週** および **毎月** スケジュールオプション | クラウドストレージファイルベースの宛先に対してアクティブ化する際に、人物および見込み客オーディエンスの完全なファイル書き出しを毎週または毎月スケジュールできるようになりました。 スケジュールオプションについて [&#128279;](/help/destinations/ui/activate-batch-profile-destinations.md#export-full-files) 詳細情報 。 |

{style="table-layout:auto"}

**修正、機能強化およびその他のお知らせ** {#destinations-fixes-and-enhancements}

- **データセット書き出しの終了日の実施が 2025 年 9 月 1 日（PT）に延期**\
  [2024 年 9 月リリース ](/help/release-notes/2024/september-2024.md#destinations-new-updated-functionality) の一環として、Adobeでは、（そのリリースより前に *作成されたデータセット書き出しデータフローに対して、デフォルトの終了日を 2025 年 5 月 1 日（PT* と設定しました。 Adobeでは、この実施期限を 2025 年 9 月 1 日 **まで延長し、お客様がスケジュールを更新するための時間を追加できるように** りました。 データセット書き出しデータフローの終了日を編集する方法については、[ データセットの書き出しチュートリアル ](../../destinations/ui/export-datasets.md#schedule-dataset-export) のスケジュールの節を参照してください。

- **LiveRamp オンボーディングの失敗した SFTP 転送の処理を改善**\
  Adobeでは、SFTP を使用した [LiveRamp Onboarding](/help/destinations/catalog/advertising/liveramp-onboarding.md) 宛先へのファイル書き出しに影響する問題の修正を実装しました。 一時的なサーバーサイドの問題が原因でファイル転送が失敗したり、失敗した試行からの一時ファイルがサーバーに残ったりすることがあります。 これらの削除不可能なファイルは、Adobeが上書きする権限を持っていなかったので、以降の再試行をブロックしました。\
  この問題を修正すると、再試行で一時ファイルを削除できない場合、Adobeは、再試行が正常に完了するように、サフィックス ,`attempt2` が付いた新しいファイルを生成します。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Adobe Experience Platform に取り込むデータの共通構造および定義（スキーマ）を提供するオープンソース仕様です。XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**更新された XDM コンポーネント**

| 機能 | 説明 |
| --- | --- |
| 文字列フィールドの値が 1 以上になる | 新しい文字列フィールドの長さは、デフォルトで最小 1 になります。 必須以外のフィールドには、引き続き null 値を使用できます。 ベストプラクティスについて詳しくは、[ データモデリングのベストプラクティス ](../../xdm/schema/best-practices.md#data-integrity-tips) に関するガイドを参照してください |

{style="table-layout:auto"}

Experience Platformの XDM について詳しくは、「[XDM システムの概要 ](../../xdm/home.md)」を参照してください。

## ID サービス {#identity}

Adobe Experience Platform ID サービスを使用すると、デバイスやシステム間で ID を紐付けることで、顧客とその行動の全体像を把握し、インパクトのある、個人的なデジタルエクスペリエンスをリアルタイムで提供できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE &#x200B; 限定提供 &#x200B;]{type=Informative} [!DNL Identity Graph Linking Rules] | ID グラフリンクルールは限定提供になり、開発用サンドボックスのすべてのお客様がアクセスできます。 <ul><li>**アクティベーション要件**：この機能は、[!DNL Identity Settings] ールを設定して保存するまで、非アクティブのままになります。 この設定がない場合、システムは引き続き正常に動作し、動作は変更されません。</li><li>**重要なメモ**：この限定提供フェーズでは、Edgeのセグメント化によって、予期しないセグメントメンバーシップの結果が生じる可能性があります。 ただし、ストリーミングおよびバッチセグメント化は期待どおりに機能します。</li><li>**次の手順**：実稼動サンドボックスでこの機能を有効にする方法については、Adobe アカウントチームにお問い合わせください。</li></ul> |

{style="table-layout:auto"}

詳しくは、[[!DNL Identity Graph Linking Rules]  ドキュメント ](../../identity-service/identity-graph-linking-rules/overview.md) を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用した標準 SQL を使用して、Adobe Experience Platform Data Lake でデータをクエリします。 データセットをシームレスに組み合わせ、クエリ結果から新しいデータセットを生成してレポートを強化したり、データサイエンスワークフローを有効にしたり、リアルタイム顧客プロファイルへの取り込みを容易にしたりします。 例えば、顧客トランザクションデータを行動データと結合して、ターゲットマーケティングキャンペーンの高価値オーディエンスを特定できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| SQL オーディエンスの上書き | 既存のプロファイルを新しい SQL クエリの結果で上書きすることで、オーディエンスメンバーシップを更新します。 これにより、古いレコードを削除し、更新されたレコードを 1 回の操作で挿入することで、動的オーディエンスをより効率的に管理できます。 詳しくは、[SQL オーディエンス拡張機能ガイド ](../../query-service/data-distiller-audiences/overview.md#replace-audience) を参照してください。 |
| クエリ結果のダウンロードとコピー | [ クエリ結果を CSV、XLSX、JSON ファイルなどのクエリエディターから直接ダウンロードする ](../../query-service/ui/overview.md#download-query-results) または [ 結果をクリップボードにコピーする ](../../query-service/ui/overview.md#copy-results) コンマ区切り値（CSV）としてダウンロードし、Excel などのスプレッドシートアプリケーションですばやく使用できます。 これらの機能強化により、オフラインでの分析、レポート作成およびデータ検証のワークフローが合理化されます。 |
| クエリ結果を全画面で表示する | [ クエリのプレビュー結果は全画面ダイアログで表示されます ](../../query-service/ui/overview.md#view-results) 読みやすさを向上したり、大きなデータセットを簡単にスキャンしたり、コピーする行を選択したりできます。 フルスクリーンビューは、サイズ変更可能なグリッドレイアウトを提供し、幅の広いテーブルや詳細な出力をより効率的にレビューするのに役立ちます。 |
| モデル予測での列選択の強化 | 特定の列を選択し、拡張 `model_predict` 構文を使用してエイリアスを適用します。 特性ベクトルや確率スコアなどの中間予測結果を取得します。 拡張選択には、機能フラグのアクティブ化が必要です。 構文例および機能フラグの詳細については、[ モデルライフサイクルドキュメント ](../../query-service/advanced-statistics/models.md#select-specific-output-fields) を参照してください。 |
| CREATE TABLE と INSERT INTO を使用したモデル予測出力の保存 | [CREATE TABLE AS SELECT を使用して、選択した予測出力を新しいテーブルに保存するか、または INSERT INTO SELECT を使用して、既存のテーブルに挿入 ](../../query-service/advanced-statistics/models.md#predict)。 拡張列選択を有効にすると、特性ベクトルや確率などの中間結果も最終的な予測と共に保持できます。 使用例については、[SQL 構文ドキュメント ](../../query-service/sql/syntax.md#create-table-as-select) を参照してください。 |

[!DNL Query Service] について詳しくは、[[!DNL Query Service] 概要](../../query-service/home.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| 偽名プロファイルデータの有効期限 | プロファイルダッシュボードで偽名プロファイルデータの有効期限を管理します。 この機能と偽名プロファイルについて詳しくは、[偽名プロファイルデータの有効期限ガイド](../../profile/pseudonymous-profiles.md)を参照してください。 |

{style="table-layout:auto"}

リアルタイム顧客プロファイルについて詳しくは、[ プロファイルの概要 ](../../profile/home.md) を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに対応するために、Experience Platformには、1 つのExperience Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つサンドボックスが用意されています。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックスツールプラグインサポートの拡張 | サンドボックスツールでジャーニーオブジェクトを複製する際、カスタムアクションを依存オブジェクトとしてコピーできるようになりました。 さらに、既存のアクションを選択して、ターゲットサンドボックスで再利用できます。 また、個別にパッケージに追加することもできます。 サポートされるAdobe Journey Optimizer オブジェクトについて詳しくは、[ サンドボックスツール ](../../sandboxes/ui/sandbox-tooling.md#adobe-journey-optimizer-objects) ガイドを参照してください。 |

{style="table-layout:auto"}

サンドボックスについて詳しくは、[ サンドボックスの概要 ](../../sandboxes/home.md) を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

Experience Platform のソースを使用して、Adobe アプリケーションまたはサードパーティのデータソースからデータを取り込みます。

**新しいソース**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} [!DNL Algolia User Profiles] | [[!DNL Algolia User Profiles]](../../sources/connectors/data-partners/algolia-user-profiles.md) ソースを使用できるようになりました。 このソースを使用して、[!DNL Algolia] ユーザープロファイルのアフィニティ データをExperience Platformに取り込みます。 その後、このデータを使用して、web サイト、e コマースプラットフォーム、アプリケーションに高性能の検索ソリューションを提供することで、ユーザーエンゲージメント、コンバージョン率および全体的な顧客体験を向上させることができます。 詳しくは、Experience Platformへのデータの取り込み [ 方法に関するガイドを参照し  [!DNL Algolia User Profiles]  ください ](../../sources/tutorials/ui/create/data-partners/algolia-user-profiles.md)。 |
| [!DNL Azure Databricks] の [!BADGE Beta]{type=Informative} API サポート | [!DNL Azure Databricks] ソースを API で使用できるようになりました。 [!DNL Flow Service] API を使用して [!DNL Databricks] アカウントを接続し、[!DNL Databricks] データをExperience Platformに取り込みます。 詳しくは、[[!DNL Azure Databricks]](../../sources/connectors/databases/databricks.md) のドキュメントを参照してください。 |

{style="table-layout:auto"}

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| ストリーミングメディアデータをExperience Platformに取り込むための XDM フィールドを更新しました。 | 新しい XDM フィールドグループ `mediaReporting` を使用して、Adobe Analytics ソースを介してストリーミングメディアデータをExperience Platformに取り込めるようになりました。 このフィールドは、`media.mediaTimed` フィールドを置き換えます。</br> <br>3 か月の移行期間中、`media.mediaTimed` フィールドへのデータ取り込みは続行されます。 ただし、2025 年 7 月末で、`media.mediaTimed` フィールドは完全に非推奨となり、Experience Platform スキーマ UI に表示されなくなります。また、データは `mediaReporting` フィールドを使用してのみ送信されます。</br><br>2025 年 4 月 22 日（PT）より前にストリーミングメディアデータを Platform に収集する Analytics ソースを実装している場合は、新しいフィールドグループを使用してデータを送信するように既存の設定を移行する必要があります。 この移行は、2025 年 7 月末までに完了する必要があります。 移行サポートについては、Adobe アカウントチームにお問い合わせください。 |
| [!DNL MariaDB] および [!DNL PostgreSQL] の新しい認証タイプ | 基本認証を使用して、Experience Platformで [!DNL MariaDB] および [!DNL PostgreSQL] ソースを認証できるようになりました。 詳しくは、次のドキュメントを参照してください。 <ul><li>[[!DNL MariaDB]](../../sources/connectors/databases/mariadb.md)</li><li>[[!DNL PostgreSQL]](../../sources/connectors/databases/postgres.md) |
| [!DNL Amazon Redshift] の行レベルのフィルタリングのサポート | Experience Platform上の [!DNL Amazon Redshift] データに対して、行レベルのフィルタリング機能を使用できます。 詳しくは、[API でのソースの行レベルデータのフィルタリング ](../../sources/tutorials/api/filter.md) に関するガイドを参照してください。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../../sources/home.md)を参照してください。

## ユースケースプレイブック {#use-case-playbooks}

ユースケースプレイブックは、もともと、Real-Time Customer Data PlatformまたはAdobe Journey Optimizerを使い始める際の課題を克服するように設計されていました。 これらは継続的に進化し、現在では、主要なマーケティングのユースケースを素早く開始し、テストして実稼動環境に移行するためのインスピレーションと事前定義済みのアセットを提供できます。

ユースケースプレイブックは、検出ツールから共同作業フレームワークに移行しました。 様々な組織で独自のプレイブックを作成、管理、共有するのに役立ちます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!BADGE Beta]{type=Informative} 独自のプレイブックを作成して共有する | 新しいプレイブックオーサリングフレームワークを使用すると、独自のユースケースプレイブックを作成、管理、共有できます。 これには、主要なメタデータのキャプチャ、ジャーニーマップの編集、関連する技術アセットの関連付けのサポートが含まれます。 組織全体でプレイブックを共有して、マーケティングアプローチを標準化し、一貫性を維持できます。 |

{style="table-layout:auto"}

独自のプレイブックを作成して共有する方法については、[ 独自のプレイブックの作成と共有 ](/help/use-case-playbooks/playbooks/author.md) ドキュメントを参照してください。

詳しくは、[ ユースケースプレイブックの概要 ](/help/use-case-playbooks/playbooks/overview.md) を参照してください。プレイブックの機能の概要、目的、エンドツーエンドのデモ（インスタンスを作成する方法や、生成されたアセットを他のサンドボックス環境に読み込む方法など）が示されています。
