---
title: Adobe Experience Platform リリースノート（2022年1月）
description: Adobe Experience Platform の 2022年1月のリリースノートです。
exl-id: 734ce1b3-e270-4c37-958c-88bcc39fbf20
source-git-commit: 668b2624b7a23b570a3869f87245009379e8257c
workflow-type: tm+mt
source-wordcount: '1342'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年1月26日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [アラート](#alerts)
- [[!DNL Dashboards]](#dashboards)
- [[!DNL Data Prep]](#data-prep)
- [[!DNL Destinations]](#destinations)
- [クエリサービス](#query-service)
- [サンドボックス](#sandboxes)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## アラート {#alerts}

Experience Platform では、様々な Platform アクティビティに関するイベントベースのアラートを登録できます。Platform ユーザーインターフェイスの「[!UICONTROL アラート]」タブを使用して、様々なアラートルールを購読し、UI 内または電子メール通知を通じてアラートメッセージを受け取るように選択できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しいアラートルール | データの取り込み、ID、プロファイル、セグメント化、アクティブ化に関連するワークフローで、いくつかの新しいアラートルールが使用できるようになりました。更新されたアラートタイプのリストの概要については、[アラートルール](../../observability/alerts/rules.md)を参照してください。 |
| ソースデータフローのコンテキスト内アラート | 登録して、取り込みワークフロー中のデータフローのステータスに関するアラートメッセージを受け取れるようになりました。詳しくは、[UI でのソースアラートの購読](../../sources/tutorials/ui/alerts.md)でのガイドを参照してください。 |

Platform のアラートについて詳しくは、[アラートの概要](../../observability/alerts/overview.md)を参照してください。

## [!DNL Dashboards] {#dashboards}

Adobe Experience Platform では、毎日のスナップショットで得られた、組織のデータに関する重要なインサイトを表示できる複数のダッシュボードを提供しています。

| 機能 | 説明 |
| --- | --- |
| インテリジェントキャプション | 機械学習アルゴリズムは、プロファイルとオーディエンスデータに関するインサイトを自動的に提供し、30 ～ 90 日（12 か月）のパターンとトレンドを示します。キャプションには、以下に関する情報が含まれます。 <ul><li>全体的な形状と統計</li><li>トレンドと急激な変化</li><li>季節的パターン</li><li>予期しない異常値</li></ul> 詳しくは、[プロファイルダッシュボード](../../dashboards/guides/profiles.md#profiles-count-trend)および[セグメントダッシュボード](../../dashboards/guides/segments.md#audience-size-trend)ドキュメントで確認することができます。 |
| ダッシュボードインベントリ | プロファイル、セグメント、宛先ダッシュボード（PowerBI などのインストール済みの統合を含む）の事前設定済みレポートに一箇所でアクセスします。 詳しくは、[[!DNL Dashboards] インベントリのドキュメント](../../dashboards/inventory.md)を参照してください。 |
| PowerBI レポートテンプレート | 新しい PowerBI グラフを使用して、プロファイル、セグメントおよび宛先レポートのデータモデルから指標を作成、カスタマイズ、拡張します。 自動インストールワークフローを使用すると、PowerBI 環境内から組織全体でマーケティングインサイトを共有できます。詳しくは、[PowerBI レポートテンプレートドキュメント](../../dashboards/integrations/power-bi.md)を参照してください。 |

[!DNL Dashboards] について詳しくは、[[!DNL Dashboards] 概要](../../dashboards/home.md)を参照してください。

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータをマッピング、変換および検証できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 統合マッピングエクスペリエンス | Platform UI の新しいマッピングインターフェイスでは、一貫したマッピングエクスペリエンスを利用して、インテリジェントマッピングレコメンデーションの活用、マッピングルールの手動設定、マッピングセットに発生したエラーのデバッグができます。詳しくは、[[!DNL Data Prep]  UI ガイド](../../data-prep/ui/mapping.md) を参照してください。 |

[!DNL Data Prep] について詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

## [!DNL Destinations] {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ----------- | ----------- |
| 同じページと次のページのパーソナライゼーション | この[同じページと次のページのパーソナライゼーション機能](../../destinations/ui/configure-personalization-destinations.md)は、マーケティングチャネルと顧客チャネルの一貫性を保つために、Experience Edge 上のアプリケーションのユーザーを共有し、ターゲティング可能なビューで表示します。 このパーソナライゼーションは、[Adobe Target 接続](../../destinations/catalog/personalization/adobe-target-connection.md)そして[カスタムパーソナライゼーション接続](../../destinations/catalog/personalization/custom-personalization.md)を通じて使用することができます。同じページまたは次のページのパーソナライゼーションキャンペーンを設定するには、[専用チュートリアル](../../destinations/ui/configure-personalization-destinations.md)を参照してください。 |
| バッチ宛先の監視とセグメントレベルの指標 | 宛先の監視機能がストリーミングの宛先から拡張され、アクティベーションデータフローのバッチ宛先とセグメントレベルの指標も含まれるようになりました。 詳しくは、[宛先ダッシュボードの監視](/help/dataflows/ui/monitor-destinations.md#monitoring-destinations-dashboard)、[セグメントジョブダッシュボードの監視](/help/dataflows/ui/monitor-destinations.md#monitoring-segment-jobs-dashboard)および[セグメントレベル表示](/help/dataflows/ui/monitor-destinations.md#segment-level-view)を参照してください。 |
| UI での既存のバッチアクティベーションデータフロー編集のスケジュール設定 | このリリースでは、既存のアクティベーションデータフローのスケジュールをバッチ保存先に編集するオプションが導入されています。 詳しくは、[プロファイルの宛先を一括でアクティブ化する](/help/destinations/ui/activate-batch-profile-destinations.md)を参照してください。 |
| Marketo 宛先の強化機能 | Marketo Engage を使用する Experience Platform ユーザーは、[Marketo 宛先コネクタ](/help/destinations/catalog/adobe/marketo-engage.md)を介して新規担当者レコードを Experience Platform から Marketo Engage にプッシュする新機能を使用して、Marketo データベースを最大化できます。<br> オーディエンスセグメントを Experience Platform から Marketo Engage に送信する際に、Marketo Engage データベースにまだ存在しないセグメント内の人物をデータベースに自動的に追加できます。 詳しくは、[Adobe Experience Platform セグメントを Marketo 静的リストにプッシュ](https://experienceleague.adobe.com/docs/marketo/using/product-docs/core-marketo-concepts/smart-lists-and-static-lists/static-lists/push-an-adobe-experience-platform-segment-to-a-marketo-static-list.html?lang=ja)を参照してください（チュートリアルの手順 9 では、新規担当者レコードを Marketo にプッシュする方法を説明します）。 |

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [Adobe Target 接続](../../destinations/catalog/personalization/adobe-target-connection.md) | Adobe Target は、web サイトやモバイルアプリなどをまたいだすべてのインバウンド顧客のインタラクションで、AI を利用したリアルタイムのパーソナライゼーションと実験を提供するアプリケーションです。Adobe Target は、Adobe Experience Platform のパーソナライゼーション接続です。 |
| [カスタムパーソナライゼーション接続](../../destinations/catalog/personalization/custom-personalization.md) | このパーソナライゼーション接続を使用すると、Adobe Experience Platform から外部のパーソナライゼーションプラットフォーム、コンテンツ管理システム、広告サーバー、および顧客の web サイト上で実行されているその他のアプリケーションにセグメント情報を取得できます。 |

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## クエリサービス {#query-service}

[!DNL Query Service] では、標準 SQL を使用して Adobe Experience Platform でデータに対してクエリを実行できます [!DNL Data Lake]。[!DNL Data Lake] の任意のデータセットを結合したり、クエリ結果を新しいデータセットとして取得したりすることで、それらのデータセットをレポートやデータサイエンスワークスペースで使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 匿名ブロック | 匿名ブロック SQL 構成を使用すると、クエリサービス内の大規模なデータ準備ジョブを小さなタスクに分類し、それらを再利用して順に実行することで増分データの読み込みを行うことができます。 詳しくは、[匿名ブロックドキュメントのクエリ例](../../query-service/essential-concepts/anonymous-block.md)を参照してください。 |
| データセットの構成 | サンドボックス内でデータアセットの量の増加に合わせ、クエリサービスで使用するデータアセットを整理するための論理的で一貫性のあるデータ構造を提供します。 詳しくは、[データアセットの整理に関するドキュメント](../../query-service/best-practices/organize-data-assets.md)を参照してください。 |

[!DNL Query Service] について詳しくは、[[!DNL Query Service]  の概要](../../query-service/home.md)を参照してください。

## サンドボックス {#sandboxes}

Adobe Experience Platform は、デジタルエクスペリエンスアプリケーションをグローバルな規模で強化するように設計されています。企業ではしばしば複数のデジタルエクスペリエンスアプリケーションを並行して運用し、運用コンプライアンスを確保しながら、アプリケーションの開発、テスト、導入に注力する必要があります。このニーズに応えるために、Experience Platform にはサンドボックスが用意されています。サンドボックスでは、単一の Platform インスタンスを別々の仮想環境に分割するので、デジタルエクスペリエンスアプリケーションの開発と発展に役立ちます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| サンドボックス UI の強化 | サンドボックスインジケーターが、すべての Platform UI アプリケーションのヘッダー内に統合されるようになりました。サンドボックスインジケーターは、サンドボックスの名前、地域、タイプを表示し、ドロップダウンメニューにアクセスしてサンドボックス間を切り替えることもできます。詳しくは、 [サンドボックス UI ガイド](../../sandboxes/ui/user-guide.md) を参照してください。 |

サンドボックスについて詳しくは、 [サンドボックスの概要](../../sandboxes/home.md) を参照してください。

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| Segment Match | Segment Match は、2 人以上の Platform ユーザーが、共通の識別子に基づいて、安全で管理され、プライバシーに優しい方法でデータを交換できるデータコラボレーションサービスです。Segment Match は、Platform のプライバシー基準と個人識別子（ハッシュ化された電子メール、ハッシュ化された電話番号、IDFA や GAID などのデバイス識別子など）を使用します。詳しくは、 [Segment Match の概要](../../segmentation/ui/segment-match/overview.md) を参照してください。 |

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

| 機能 | 説明 |
| --- | --- |
| ベータ版ソースの一般公開（GA） | 以下のソースがベータ版から一般公開（GA）に昇格しました。 <ul><li>[[!DNL Snowflake]](../../sources/connectors/databases/snowflake.md)</li><li>[[!DNL Veeva CRM]](../../sources/connectors/crm/veeva.md)</li></ul> |
| [!DNL Event Hubs] ソースの強化機能 | この [!DNL Event Hubs] ソースは、ソース接続を接続および作成するため、非ルート SAS キータイプ認証をサポートするようになりました。 詳しくは、[[!DNL Event Hubs] 概要](../../sources/connectors/cloud-storage/eventhub.md)を参照してください。 |
| [!DNL SFTP] ソースの強化機能 | この [!DNL SFTP] ソースでは、データフローが SFTP サーバーへの接続に使用できる最大同時接続数の設定を確立できるようになりました。詳しくは、[[!DNL SFTP] 概要](../../sources/connectors/cloud-storage/sftp.md)を参照してください。 |
