---
title: Adobe Experience Platform リリースノート 2022年3月
description: Adobe Experience Platform の 2022年3月のリリースノート。
exl-id: 0d499aa6-e25d-4d34-ad32-5e4ab361cba1
source-git-commit: 34e0381d40f884cd92157d08385d889b1739845f
workflow-type: ht
source-wordcount: '1194'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年3月30日（PT）**

Adobe Experience Platform の新機能：

- [監査ログ](#audit-logs)
- [Real-Time CDP B2B Edition の関連するアカウント](#related-accounts)

Adobe Experience Platform の既存の機能に対するアップデート：

- [アラート](#alerts)
- [[!DNL Dashboards]](#dashboards)
- [データ収集](#data-collection)
- [[!DNL Query Service]](#query-service)
- [ソース](#sources)

## 監査ログ {#audit-logs}

Experience Platform を使用すると、様々なサービスおよび機能についてユーザーアクティビティを監査できます。監査ログは、誰がいつ何をしたかに関する情報を提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| データセット、スキーマ、クラス、フィールドグループ、データタイプ、サンドボックス、宛先、セグメント、結合ポリシー、計算属性、製品プロファイルおよびアカウント（アドビ）に関する監査ログ | これらは、監査ログで記録されるリソースです。この機能を有効にすると、アクティビティが発生したときに監査ログが自動的に収集されます。ログ収集を手動で有効にする必要はありません。 |
| 監査ログの書き出し | 監査ログは、`CSV` または `JSON` ファイルとしてダウンロードできます。生成されるファイルは、マシンに直接保存されます。 |

{style=&quot;table-layout:auto&quot;}

Platform の監査ログについて詳しくは、[監査ログの概要](../../landing/governance-privacy-security/audit-logs/overview.md)を参照してください。

## Real-Time CDP B2B Edition の関連するアカウント {#related-accounts}

>[!NOTE]
>
>関連するアカウント機能は、Real-Time CDP B2B Edition のお客様のみ使用できます。

B2B 企業では、多くの場合、顧客情報が複数のシステムに保存されており、それぞれのシステムには、同じ実世界のビジネスエンティティに関するデータの一部のみ、または矛盾するデータが含まれています。そのため、顧客を正確に把握することが難しく、B2B マーケティングや営業活動の効率や効果を低下させるという大きな課題を抱えています。関連するアカウントのリリースにより、[!DNL Real-Time CDP B2B] では、参照中のアカウントに類似したアカウントのリストを表示するようになりました。関連するアカウントをセグメント定義に含めることができるので、リーチを広げたり、セグメントでより広い条件を適用したりできます。

この機能について詳しくは、次のドキュメントページを参照してください。

- [Real-Time CDP B2B Edition の関連するアカウントの概要](../../rtcdp/b2b-ai-ml-services/related-accounts.md)
- [アカウントプロファイル UI ガイドの「関連するアカウント」タブ](../../rtcdp/accounts/account-profile-ui-guide.md#related-accounts-tab)
- [セグメント定義での関連するアカウントの使用方法](../../rtcdp/segmentation/b2b.md#related-accounts)

Real-Time CDP B2B エディションについて詳しくは、[概要](../../rtcdp/overview.md)を参照してください。

## アラート {#alerts}

Experience Platform では、様々な Platform アクティビティに関するイベントベースのアラートを登録できます。Platform ユーザーインターフェイスの「[!UICONTROL アラート]」タブを使用して、様々なアラートルールを購読し、UI 内または電子メール通知を通じてアラートメッセージを受け取るように選択できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しいアラートルール | データ取り込みに関連するソースに対して、2 つの新しいアラートルールが使用できるようになりました。更新されたアラートタイプのリストの概要については、[アラートルール](../../observability/alerts/rules.md)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

Platform のアラートについて詳しくは、[アラートの概要](../../observability/alerts/overview.md)を参照してください。

## ダッシュボード {#dashboards}

Adobe Experience Platform は、毎日のスナップショットで取得した、組織のデータに関する重要な情報を表示できる複数の [!DNL dashboards] を提供しています。

### プロファイルダッシュボード

プロファイルダッシュボードは、組織が Experience Platform のプロファイルストア内に持つ属性（レコード）データのスナップショットを表示します。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| セグメント化されていないプロファイルウィジェット | このウィジェットには、どのセグメントにも属していないすべてのプロファイルの合計数が表示されます。生成される数は、最後のスナップショット時点のもので、組織全体のプロファイルアクティブ化の機会を表しています。詳しくは、[プロファイル標準ウィジェットのドキュメント](../../dashboards/guides/profiles.md#standard-widgets)を参照してください。 |
| セグメント化されていないプロファイルのトレンドウィジェット | このウィジェットには、一定期間内にどのセグメントにも属していないプロファイルの数が折れ線グラフで表示されます。30 日、90 日および 12 か月の期間のトレンドを可視化できます。詳しくは、[プロファイル標準ウィジェットのドキュメント](../../dashboards/guides/profiles.md#standard-widgets)を参照してください。 |
| セグメント化されていないプロファイル（ID 別）ウィジェット | このウィジェットは、セグメント化されていないプロファイルの合計数を、一意の識別子で分類します。データは、棒グラフで可視化されます。詳しくは、[プロファイル標準ウィジェットのドキュメント](../../dashboards/guides/profiles.md#standard-widgets)を参照してください。 |
| 単一の ID プロファイルウィジェット | このウィジェットは、ID を作成する ID タイプが電子メールか ECID のどちらかのみである組織のプロファイルの数を提供します。詳しくは、[プロファイル標準ウィジェットのドキュメント](../../dashboards/guides/profiles.md#standard-widgets)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

プロファイルダッシュボードについて詳しくは、[プロファイルダッシュボードの概要](../../dashboards/guides/profiles.md)を参照してください。

### 宛先ダッシュボード

宛先ダッシュボードは、組織が Experience Platform 内で有効にしている宛先のスナップショットを表示します。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 宛先数ウィジェット | このウィジェットには、システム内でオーディエンスをアクティブにして配信できる、利用可能なエンドポイントの合計数が表示されます。この数には、アクティブな宛先と非アクティブな宛先の両方が含まれます。詳しくは、[宛先標準ウィジェットのドキュメント](../../dashboards/guides/destinations.md#standard-widgets)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

Platform の宛先ダッシュボードについて詳しくは[宛先ダッシュボードの概要](../../dashboards/guides/destinations.md)を参照してください。

## データ収集 {#data-collection}

Platform は、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信します。そこでデータを強化、変換、アドビまたはアドビ以外の宛先への配信を可能にする一連のテクノロジーを提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| グローバルデータストリーム設定 | データストリームを設定する際に、新しいグローバル設定（ジオロケーション、ファーストパーティ ID cookie、サードパーティ ID 同期）を設定できるようになりました。詳しくは、データストリーム UI ガイドの[データストリームの設定](../../edge/datastreams/overview.md#create)の節を参照してください。 |
| [Edge Network Server API](../../server-api/overview.md) | この Server API を使用すると、お客様は、新しい認証済みエンドポイントを使用して Experience Platform Edge Network を操作し、様々なデータ収集、パーソナライゼーション、広告、マーケティングのユースケースを強化できます。 |

Platform のデータ収集について詳しくは、[データ収集概要](../../collection/home.md)を参照してください。

## クエリサービス {#query-service}

[!DNL Query Service] では、標準 SQL を使用して Adobe Experience Platform でデータに対してクエリを実行できます [!DNL Data Lake]。[!DNL Data Lake] の任意のデータセットを結合したり、クエリ結果を新しいデータセットとして取得したりすることで、それらのデータセットをレポートやデータサイエンスワークスペースで使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| `table_exists` | この新機能コマンドは、現在、システム内にテーブルが存在するかどうかを確認するために使用されます。このコマンドは、ブール値を返します（テーブルが存在&#x200B;**する**&#x200B;場合は `true`、テーブルが存在&#x200B;**しない**&#x200B;場合は `false`）。詳しくは、[SQL 構文ドキュメント](../../query-service/sql/syntax.md)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

使用できる機能について詳しくは、[クエリサービスの概要](../../query-service/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得の実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| B2B で使用できる新しいソースを提供開始 | B2B ユースケースで、Platform で使用できるすべてのソースを使用できるようになりました。使用できるソースの完全なリストについては、[ソースカタログ](../../sources/home.md)を参照してください。 |
| 新しい [!DNL Oracle Eloqua] ソースの一般提供 | [!DNL Oracle Eloqua] ソースを使用して、[!DNL Oracle Eloqua] インスタンス（アカウント、キャンペーン、連絡先）から Platform にシームレスにデータを取り込むことができるようになりました。詳しくは、[ [!DNL Oracle Eloqua]  ソース接続の作成](../../sources/connectors/marketing-automation/oracle-eloqua.md)のドキュメントを参照してください。 |
| [!DNL Data Landing Zone] の API の強化 | [!DNL Data Landing Zone] ソースは、[!DNL Flow Service] API 使用時のファイルプロパティの自動検出をサポートするようになりました。詳しくは、[ [!DNL Data Landing Zone]  ソース接続の作成](../../sources/tutorials/api/create/cloud-storage/data-landing-zone.md)のドキュメントを参照してください。 |

{style=&quot;table-layout:auto&quot;}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
