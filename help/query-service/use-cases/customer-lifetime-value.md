---
title: データシグナルを追跡して、顧客のライフタイムバリューを生成する
description: このガイドでは、Data Distillerとユーザー定義ダッシュボードをReal-time Customer Data Platformで使用して、顧客のライフタイム値を測定および視覚化する方法のエンドツーエンドのデモを示します。
exl-id: c74b5bff-feb2-4e21-9ee4-1e0973192570
source-git-commit: ddf886052aedc025ff125c03ab63877cb049583d
workflow-type: tm+mt
source-wordcount: '1263'
ht-degree: 7%

---

# データシグナルを追跡して、顧客のライフタイムバリューを生成する

Real-time Customer Data Platformを使用して、顧客のライフタイム値（CLV）を追跡し、その指標をユーザー定義のダッシュボードで視覚化できます。 Data Distillerとユーザー定義のダッシュボードを使用すると、関係全体にわたって顧客の価値を測定できます。 CLV を知ることで、既存の顧客を維持し、利益率を維持しながら、新規顧客を獲得するためのビジネス戦略を開発するのに役立ちます。

次のインフォグラフィックは、マーケティングキャンペーンを改善するための高パフォーマンスのデータを生成する、データ収集、操作、分析、操作のサイクルを示しています。

![ 観測から解析、行動までのデータを示した往復のインフォグラフィック ](../images/use-cases/infographic-use-case-cycle.png)

このエンドツーエンドのユースケースは、顧客のライフタイム値から得られた属性を計算するために、データ信号を取得および変更する方法を示しています。 これらの派生データセットはその後、Real-Time CDP プロファイルデータに適用でき、ユーザー定義のダッシュボードで使用して insight analysis のダッシュボードを作成できます。 Data Distillerを通じて、Real-Time CDP insights データモデルを拡張し、CLV 派生データセットとダッシュボードインサイトを使用して、新しいオーディエンスを作成し、目的の宛先に対してアクティブ化できます。 これらの高パフォーマンスのオーディエンスは、次のマーケティングキャンペーンの強化に使用できます。

このガイドは、CLV を推進する主要なタッチポイントをまたいでデータ信号を測定し、環境に類似のユースケースを実装することで、カスタマーエクスペリエンスをより深く理解できるように設計されています。 プロセス全体の概要を以下の画像に示します。

![ 顧客のライフタイムバリューを利用するために必要な幅広い手順のインフォグラフィック ](../images/use-cases/implementation-steps.png)

## はじめに {#getting-started}

このガイドでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [ クエリサービス ](../home.md):SQL クエリを使用してデータを分析および強化できるユーザーインターフェイスと RESTful API を提供します。
* [ セグメント化サービス ](../../segmentation/home.md)：リアルタイム顧客プロファイルデータからオーディエンスを生成できます。

## 前提条件

このガイドでは、パッケージの一部として [Data Distiller](../data-distiller/overview.md) SKU が必要です。 不明な点がある場合は、Adobe サービスの担当者にお問い合わせください。

## 派生データセットの作成 {#create-derived-dataset}

CLV を確立する最初の手順は、ユーザーの操作から取得したデータ信号から派生データセットを作成することです。 この特定のユースケースは、航空会社のロイヤルティスキームに関する別のドキュメントで取り上げています。 [ クエリサービスを使用して、プロファイルデータで使用するデシルベースの派生データセットを作成する ](./deciles-use-case.md) 方法については、ガイドを参照してください。 完全な例と説明は、次の手順を説明するドキュメントに記載されています。

* デシルバケット化を許可するスキーマを作成します。
* クエリサービスを使用してデシルを作成します。
* デシルデータセットを生成します。
* リアルタイム顧客プロファイルで使用するスキーマを有効にします。
* ID 名前空間を作成し、プライマリ識別子としてマークします。
* ルックバック期間のデシルを計算するクエリを作成します。

## インサイトデータモデルの拡張と更新のスケジュール設定 {#extend-data-model-and-set-refresh-schedule}

次に、カスタムデータモデルを作成するか、既存のAdobe Real-Time CDP データモデルを拡張して、CLV レポートインサイトに関与する必要があります。 [ クエリサービスを通じてレポートインサイトデータモデルを作成し、高速化ストアデータとユーザー定義ダッシュボードで使用する ](../data-distiller/sql-insights/reporting-insights-data-model.md#build-a-reporting-insights-data-model) 方法については、ドキュメントを参照してください。 このチュートリアルでは、次の手順について説明します。

* Data Distillerを使用して、レポートインサイト用のモデルを作成します。
* テーブル、関係の作成およびデータの入力
* レポートインサイトデータモデルをクエリします。
* Real-Time CDP インサイトデータモデルを使用したデータモデルの拡張。
* レポートインサイトモデルを拡張するディメンションテーブルを作成します。
* 拡張した高速ストアレポートインサイトデータモデルのクエリ

[SQL クエリテンプレートをカスタマイズして、マーケティングおよび主要業績評価指標（KPI）のユースケースに関する Real-Time CDP レポートを作成する](../../dashboards/data-models/cdp-insights-data-model-b2c.md)方法については、 Real-time Customer Data Platform インサイトデータモデルのドキュメントを参照してください。

カスタムデータモデルを更新するスケジュールを定期的に設定してください。 これにより、必要に応じて取り込みパイプラインの一部としてデータが戻り、ユーザー定義のダッシュボードに入力されます。 スケジュールの設定方法については、[ スケジュールクエリガイド ](../ui/query-schedules.md#create-schedule) を参照してください。

## インサイトを得るためのダッシュボードの作成 {#build-a-custom-dashboard}

これで、カスタムデータモデルが作成されたので、カスタムクエリおよびユーザー定義ダッシュボードを使用してデータを視覚化する準備が整いました。 [ カスタムダッシュボードの作成 ](../../dashboards/standard-dashboards.md) 方法に関する完全なガイダンスについては、ユーザー定義ダッシュボードの概要を参照してください。 UI ガイドには、次の項目の詳細が含まれます。

* ウィジェットの作成方法。
* ウィジェットコンポーザーの使用方法。

デシルバケットを使用するカスタム CLV ウィジェットの例を以下に示します。

![ カスタムデシルベースの CLTV ウィジェットのコレクション。](../images/use-cases/deciles-user-defined-dashboard.png)

## 高パフォーマンスのオーディエンスを作成してアクティブ化する {#create-and-activate-audiences}

次の手順では、セグメント定義を作成し、リアルタイム顧客プロファイルデータからオーディエンスを生成します。 [Platform でオーディエンスを作成およびアクティブ化 ](../../segmentation/ui/segment-builder.md) する方法については、セグメントビルダー UI ガイドを参照してください。 このガイドには、次の方法に関する節が用意されています。

* 属性、イベントおよび既存のオーディエンスの組み合わせを構成要素として使用して、セグメント定義を作成する。
* ルールビルダーキャンバスとコンテナを使用して、セグメント化ルールの実行順序を制御します。
* 見込みオーディエンスの予測値を表示する。必要に応じてセグメント定義を調整できます。
* スケジュールに沿ったセグメント化に対してすべてのセグメント定義を有効にする。
* ストリーミングによるセグメント化に対して、特定のセグメント定義を有効にする。

または、詳細については、[ セグメントビルダービデオチュートリアル ](https://experienceleague.adobe.com/docs/platform-learn/tutorials/audiences/create-segments.html) も参照できます。

## メールキャンペーンのオーディエンスのアクティブ化 {#activate-audience-for-campaign}

オーディエンスを作成したら、宛先に対してアクティブ化する準備が整います。 Platform は、プロモーションメールキャンペーンの送信など、メールマーケティング活動を管理できる様々なメールサービスプロバイダー（ESP）をサポートしています。

[ メールマーケティングの宛先の概要 ](../../destinations/catalog/email-marketing/overview.md#connect-destination) で、データの書き出し先としてサポートされている宛先のリスト（[OracleEloqua](../../destinations/catalog/email-marketing/oracle-eloqua-api.md) ページなど）を確認します。

## キャンペーンから返された分析データを確認します {#post-campaign-data-analysis}

高速化データストア内のデータモデルに対してスケジュールされた更新の一環として、ソースのデータを [ 増分的に処理 ](../key-concepts/incremental-load.md) できるようになりました。 顧客からの任意の応答イベントは、発生時またはバッチでAdobe Experience Platformに取り込むことができます。 データモデルは、設定やソースコネクタに応じて、1 日に 1 回または複数回に更新できます。 詳しくは、[ バッチ取得 API の概要 ](../../ingestion/batch-ingestion/api-overview.md) または [ ストリーミング取得の概要 ](../../ingestion/streaming-ingestion/overview.md) を参照してください。

データモデルが更新されると、カスタムダッシュボードウィジェットは、顧客のライフタイム値を測定および視覚化できる意味のあるシグナルを提供します。

![ オーディエンスとメールキャンペーンに応じて開封されたメールの数を表示するカスタムウィジェット。](../images/use-cases/post-activation-and-email-response-kpis.png)

カスタム分析用に様々なビジュアライゼーションオプションが用意されています。

![ キャンペーンバケット別に開かれたメール ](../images/use-cases/email-opened-by-campaign-buckets.png)

これらのインサイトは、後続のキャンペーンのビジネス戦略を策定するのに役立ちます。

![ メールキャンペーンの結果の詳細を示す、4 つのカスタマイズされたウィジェットのコレクション。](../images/use-cases/example-widgets.png)

## 次の手順

このドキュメントでは、Real-time Customer Data Platformを使用して顧客のライフタイム値（CLV）指標を追跡および視覚化する方法について、より深く理解します。 クエリサービスとExperience Platformを通じて提供される多くのビジネスユースケースについて詳しくは、次のドキュメントを参照することをお勧めします。

* [クエリサービスの汎用性と利点を示す、放棄されたブラウザユースケースのエンドツーエンドの例です。](./abandoned-browse.md)
* [クエリサービスと機械学習を使用して、正規のオンライン web サイト訪問者トラフィックからボットアクティビティを特定し、フィルタリングする方法](./bot-filtering.md)
* [選択した文字列にほぼ一致することで、複数のデータセットの結果を組み合わせて Platform データを照合する方法。](./fuzzy-match.md)

<!-- "Data signals are actions taken by consumers while online that offer clues about intent that can be acted upon. This includes anything from visiting a website to filling out a change of address or clicking an ad."  -->

<!-- "Customer touchpoints are your brand's points of customer contact, from start to finish." -->
