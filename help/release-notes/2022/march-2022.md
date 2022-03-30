---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
source-git-commit: 04d35137a301492794ab8c0c67183cf5c76f2105
workflow-type: tm+mt
source-wordcount: '1063'
ht-degree: 30%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 3 月 30 日（PT）**

Adobe Experience Platform の新機能：

- [監査ログ](#audit-logs)

Adobe Experience Platform の既存の機能に対するアップデート：

- [アラート](#alerts)
- [[!DNL Dashboards]](#dashboards)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [[!DNL Query Service]](#query-service)
- [ソース](#sources)

## 監査ログ {#audit-logs}

Experience Platformを使用すると、様々なサービスや機能のユーザーアクティビティを監査できます。 監査ログは、誰がいつ何を実行したかに関する情報を提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| データセット、スキーマ、クラス、フィールドグループ、データタイプ、サンドボックス、宛先、セグメント、結合ポリシー、計算済み属性、製品プロファイルおよびアカウント (Adobe) の監査ログ | これらは、監査ログで記録されるリソースです。 この機能を有効にすると、アクティビティの発生時に監査ログが自動的に収集されます。 手動でログの収集を有効にする必要はありません。 |
| 監査ログの書き出し | 監査ログは、 `CSV` または `JSON` ファイル。 生成されたファイルは、直接お使いのマシンに保存されます。 |

{style=&quot;table-layout:auto&quot;}

Platform での監査ログについて詳しくは、 [監査ログの概要](../../landing/governance-privacy-security/audit-logs/overview.md).

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

プロファイルダッシュボードについて詳しくは、 [プロファイルダッシュボードの概要](../../dashboards/guides/profiles.md).

### 宛先ダッシュボード

「宛先」ダッシュボードには、組織が宛先内で有効にした宛先のスナップショットがExperience Platformされます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 宛先数ウィジェット | ウィジェットは、オーディエンスをアクティブ化し、システム内で配信できる、使用可能なエンドポイントの合計数を提供します。 この数には、アクティブな宛先と非アクティブな宛先の両方が含まれます。 詳しくは、 [destinations standard ウィジェットドキュメント](../../dashboards/guides/destinations.md#standard-widgets) を参照してください。 |

Platform での宛先ダッシュボードについて詳しくは、 [宛先ダッシュボードの概要](../../dashboards/guides/destinations.md).

## エクスペリエンスデータモデル（XDM） {#xdm}

エクスペリエンスデータモデル (XDM) は、Adobe Experience Platformに取り込まれるデータの共通の構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているため、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客行動から有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

| 機能 | 説明 |
| --- | --- |
| スキーマの個々の標準フィールドを追加または削除する | スキーマエディターの UI で、標準フィールドグループの一部をスキーマに追加できるようになり、カスタムリソースを最初から作成しなくても、含めるように選択したフィールドの柔軟性が向上しました。<br><br>また、スキーマ構造内で直接アドホックカスタムフィールドを定義し、事前にフィールドグループを作成または編集しなくても、新しいカスタムフィールドグループまたは既存のカスタムフィールドグループに割り当てることができるようになりました。<br><br>詳しくは、 [UI でのスキーマの作成と編集](../../xdm/ui/resources/schemas.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

Platform での XDM について詳しくは、 [XDM システムの概要](../../xdm/home.md).

## クエリサービス {#query-service}

[!DNL Query Service] では、標準 SQL を使用して Adobe Experience Platform でデータに対してクエリを実行できます [!DNL Data Lake]。任意のデータセットを [!DNL Data Lake] から結合し、クエリの結果を新しいデータセットとして取得することで、レポートや Data Science Workspace で使用したり、リアルタイム顧客プロファイルに取り込んだりできます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| `table_exists` | 新しいフィーチャコマンドは、現在システムにテーブルが存在するかどうかを確認するために使用されます。 このコマンドは、次のブール値を返します。 `true` ( テーブルが **は** 存在し、 `false` テーブルが **not** 存在する。 詳しくは、 [SQL 構文ドキュメント](../../query-service/sql/syntax.md) を参照してください。 |

使用可能な機能について詳しくは、 [クエリサービスの概要](../../query-service/home.md).

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムと CRM サービスの認証と接続、取り込みの実行時間の設定、全体でのデータ取り込みの管理をおこなうことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| B2B を使用する際に新しいソースが利用できるようになりました | B2B の使用例に対して、Platform で利用可能なすべてのソースを使用できるようになりました。 詳しくは、 [ソースカタログ](../../sources/home.md) を参照してください。 |
| 新しいの一般リリース [!DNL Oracle Eloqua] ソース | これで、 [!DNL Oracle Eloqua] データをシームレスに取り込むためのソース [!DNL Oracle Eloqua] インスタンス（アカウント、キャンペーン、連絡先）を Platform に送信します。 詳しくは、 [作成 [!DNL Oracle Eloqua] ソース接続](../../sources/connectors/oracle-eloqua.md) を参照してください。 |
| の API 強化 [!DNL Data Landing Zone] | この [!DNL Data Landing Zone] ソースで、 [!DNL Flow Service] API 詳しくは、 [作成 [!DNL Data Landing Zone] ソース接続](../../sources/tutorials/api/create/cloud-storage/data-landing-zone.md) を参照してください。 |

{style=&quot;table-layout:auto&quot;}

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
