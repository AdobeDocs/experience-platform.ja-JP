---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
exl-id: f854f9e5-71be-4d56-a598-cfeb036716cb
source-git-commit: 3f1750d75bd69c5cf47eb593144f564564f90405
workflow-type: tm+mt
source-wordcount: '1193'
ht-degree: 27%

---

# Adobe Experience Platform リリースノート

**リリース日：2022年3月30日（PT）**

Adobe Experience Platform の新機能：

- [監査ログ](#audit-logs)
- [Real-Time CDP B2B Edition の関連アカウント](#related-accounts)

Adobe Experience Platform の既存の機能に対するアップデート：

- [アラート](#alerts)
- [[!DNL Dashboards]](#dashboards)
- [データ収集](#data-collection)
- [[!DNL Query Service]](#query-service)
- [ソース](#sources)

<!-- - [Experience Data Model (XDM)](#xdm) -->

## 監査ログ {#audit-logs}

Experience Platformを使用すると、様々なサービスや機能のユーザーアクティビティを監査できます。 監査ログは、誰がいつ何を実行したかに関する情報を提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| データセット、スキーマ、クラス、フィールドグループ、データタイプ、サンドボックス、宛先、セグメント、結合ポリシー、計算済み属性、製品プロファイルおよびアカウント (Adobe) の監査ログ | これらは、監査ログで記録されるリソースです。 この機能を有効にすると、アクティビティの発生時に監査ログが自動的に収集されます。 手動でログの収集を有効にする必要はありません。 |
| 監査ログの書き出し | 監査ログは、 `CSV` または `JSON` ファイル。 生成されたファイルは、直接お使いのマシンに保存されます。 |

{style=&quot;table-layout:auto&quot;}

Platform での監査ログについて詳しくは、 [監査ログの概要](../../landing/governance-privacy-security/audit-logs/overview.md).

## Real-Time CDP B2B Edition の関連アカウント {#related-accounts}

>[!NOTE]
>
>関連アカウント機能は、Real-Time CDP B2B Edition のお客様のみが利用できます。

B2B 企業は、多くの場合、同じ実世界のビジネスエンティティに対する部分的または競合するデータのみを含め、顧客情報を複数のシステムに保存します。 これにより、顧客の正確な見地に到達するという大きな課題が生じ、B2B マーケティングと販売の取り組みの効率と効果が低下します。 関連アカウントのリリースに伴い、 [!DNL Real-time CDP B2B] では、参照しているアカウントに類似したアカウントのリストが表示されます。 セグメント定義に関連するアカウントを含めて、リーチを広げたり、セグメントに幅広い条件を適用したりできます。

この機能の詳細については、次のドキュメントページを参照してください。

- [Real-Time CDP B2B Edition の関連アカウントの概要](../../rtcdp/b2b-ai-ml-services/related-accounts.md)
- [アカウントプロファイル UI ガイドの「関連アカウント」タブ](../../rtcdp/accounts/account-profile-ui-guide.md#related-accounts-tab)
- [セグメント定義での関連アカウントの使用方法](../../rtcdp/segmentation/b2b.md#related-accounts)

Real-time CDP B2B Edition の詳細については、 [概要](../../rtcdp/overview.md).

## アラート {#alerts}

Experience Platform では、様々な Platform アクティビティに関するイベントベースのアラートを登録できます。Platform ユーザーインターフェイスの「[!UICONTROL アラート]」タブを使用して、様々なアラートルールを購読し、UI 内または電子メール通知を通じてアラートメッセージを受け取るように選択できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しいアラートルール | データの取り込みに関連するソースで、2 つの新しいアラートルールを使用できるようになりました。 更新されたアラートタイプのリストの概要については、[アラートルール](../../observability/alerts/rules.md)を参照してください。 |

{style=&quot;table-layout:auto&quot;}

Platform のアラートについて詳しくは、[アラートの概要](../../observability/alerts/overview.md)を参照してください。

## ダッシュボード {#dashboards}

Adobe Experience Platform [!DNL dashboards] 毎日のスナップショットでキャプチャされた、組織のデータに関する重要な情報を表示できます。

### プロファイルダッシュボード

プロファイルダッシュボードには、組織がExperience Platform内のプロファイルストアに保持している属性（レコード）データのスナップショットが表示されます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 非セグメント化プロファイルウィジェット | ウィジェットは、どのセグメントにも関連付けられていないすべてのプロファイルの合計数を提供します。 生成された数は、最後のスナップショット時点での正確さで、組織全体でのプロファイルのアクティベーションの機会を表します。 詳しくは、 [profiles standard widgets に関するドキュメント](../../dashboards/guides/profiles.md#standard-widgets) を参照してください。 |
| 非セグメント化プロファイルトレンドウィジェット | このウィジェットには、特定の期間にどのセグメントにも接続されていないプロファイルの数を示す線グラフの図が表示されます。 トレンドは、30 日、90 日、12 か月の期間で視覚化できます。 詳しくは、 [profiles standard widgets に関するドキュメント](../../dashboards/guides/profiles.md#standard-widgets) を参照してください。 |
| ID ウィジェットによる非セグメント化プロファイル | このウィジェットは、セグメント化されていないプロファイルの合計数を、一意の識別子で分類します。 データは棒グラフで表示されます。 詳しくは、 [profiles standard widgets に関するドキュメント](../../dashboards/guides/profiles.md#standard-widgets) を参照してください。 |
| 単一の ID プロファイルウィジェット | このウィジェットは、電子メールまたは ECID のいずれかの ID を作成する 1 種類の ID タイプのみを持つ組織のプロファイルの数を提供します。 詳しくは、 [profiles standard widgets に関するドキュメント](../../dashboards/guides/profiles.md#standard-widgets) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

プロファイルダッシュボードについて詳しくは、 [プロファイルダッシュボードの概要](../../dashboards/guides/profiles.md).

### 宛先ダッシュボード

「宛先」ダッシュボードには、組織が宛先内で有効にした宛先のスナップショットがExperience Platformされます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 宛先数ウィジェット | ウィジェットは、オーディエンスをアクティブ化し、システム内で配信できる、使用可能なエンドポイントの合計数を提供します。 この数には、アクティブな宛先と非アクティブな宛先の両方が含まれます。 詳しくは、 [destinations standard ウィジェットドキュメント](../../dashboards/guides/destinations.md#standard-widgets) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

Platform での宛先ダッシュボードについて詳しくは、 [宛先ダッシュボードの概要](../../dashboards/guides/destinations.md).

## データ収集 {#data-collection}

Platform は、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信します。そこでデータを強化、変換、アドビまたはアドビ以外の宛先への配信を可能にする一連のテクノロジーを提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| グローバルデータストリーム設定 | データストリームを設定する際に、次の新しいグローバル設定をいくつか設定できるようになりました。地域、ファーストパーティ ID cookie およびサードパーティ ID 同期。 詳しくは、 [データストリームの設定](../../edge/fundamentals/datastreams.md#configure) （データストリーム UI ガイド）を参照してください。 |
| [Edge Network Server API](../../server-api/overview.md) | Server API を使用すると、新しい認証済みエンドポイントを使用してExperience PlatformEdge ネットワークとやり取りし、様々なデータ収集、パーソナライズ、広告、マーケティングの使用例を実現できます。 |

Platform のデータ収集について詳しくは、[データ収集概要](../../collection/home.md)を参照してください。

<!-- ## Experience Data Model (XDM) {#xdm}

Experience Data Model (XDM) is an open-source specification that provides common structures and definitions (schemas) for data that is brought into Adobe Experience Platform. By adhering to XDM standards, all customer experience data can be incorporated into a common representation to deliver insights in a faster, more integrated way. You can gain valuable insights from customer actions, define customer audiences through segments, and use customer attributes for personalization purposes.

| Feature | Description |
| --- | --- |
| Add or remove individual standard fields for a schema | The Schema Editor UI now allows you to add portions of standard field groups to your schemas, providing more flexibility for the fields you choose to include without needing to build custom resources from scratch.<br><br>You can now also define ad-hoc custom fields directly within the schema structure and assign them to a new or existing custom field group without needing to create or edit the field group beforehand.<br><br>See the guide on [creating and editing schemas in the UI](../../xdm/ui/resources/schemas.md) for more information on these new workflows. |

{style="table-layout:auto"}

For more information on XDM in Platform, see the [XDM System overview](../../xdm/home.md). -->

## クエリサービス {#query-service}

[!DNL Query Service] では、標準 SQL を使用して Adobe Experience Platform でデータに対してクエリを実行できます [!DNL Data Lake]。任意のデータセットを [!DNL Data Lake] から結合し、クエリの結果を新しいデータセットとして取得することで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| `table_exists` | 新しいフィーチャコマンドは、現在システムにテーブルが存在するかどうかを確認するために使用されます。 このコマンドは、次のブール値を返します。 `true` ( テーブルが **は** 存在し、 `false` テーブルが **not** 存在する。 詳しくは、 [SQL 構文ドキュメント](../../query-service/sql/syntax.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

使用可能な機能について詳しくは、 [クエリサービスの概要](../../query-service/home.md).

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムと CRM サービスの認証と接続、取り込みの実行時間の設定、全体でのデータ取り込みの管理をおこなうことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| B2B を使用する際に新しいソースが利用できるようになりました | B2B の使用例に対して、Platform で利用可能なすべてのソースを使用できるようになりました。 詳しくは、 [ソースカタログ](../../sources/home.md) を参照してください。 |
| 新しいの一般リリース [!DNL Oracle Eloqua] ソース | これで、 [!DNL Oracle Eloqua] データをシームレスに取り込むためのソース [!DNL Oracle Eloqua] インスタンス（アカウント、キャンペーン、連絡先）を Platform に送信します。 詳しくは、 [作成 [!DNL Oracle Eloqua] ソース接続](../../sources/connectors/marketing-automation/oracle-eloqua.md) を参照してください。 |
| の API 強化 [!DNL Data Landing Zone] | この [!DNL Data Landing Zone] ソースで、 [!DNL Flow Service] API 詳しくは、 [作成 [!DNL Data Landing Zone] ソース接続](../../sources/tutorials/api/create/cloud-storage/data-landing-zone.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
