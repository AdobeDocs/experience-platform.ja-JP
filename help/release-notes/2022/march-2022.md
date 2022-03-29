---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
source-git-commit: 7145867795bcd8e1093c09df3fefdee518f9578a
workflow-type: tm+mt
source-wordcount: '447'
ht-degree: 50%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 3 月 30 日（PT）**

Adobe Experience Platform の新機能：

- [[!DNL Audit Logs]](#audit-logs)

Adobe Experience Platform の既存の機能に対するアップデート：

- [アラート](#alerts)
- [ソース](#sources)

## [!DNL Audit Logs] {#audit-logs}

Experience Platformを使用すると、様々なサービスや機能のユーザーアクティビティを監査できます。 監査ログは、誰がいつ何を実行したかに関する情報を提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| データセット、スキーマ、クラス、フィールドグループ、データタイプ、サンドボックス、宛先、セグメント、結合ポリシー、計算済み属性、製品プロファイルおよびアカウント (Adobe) の監査ログ | これらは、監査ログで記録されるリソースです。 この機能を有効にすると、アクティビティの発生時に監査ログが自動的に収集されます。 手動でログの収集を有効にする必要はありません。 |
| 監査ログの書き出し | 監査ログは、 `CSV` または `JSON` ファイル。 生成されたファイルは、直接お使いのマシンに保存されます。 |

Platform での監査ログについて詳しくは、 [監査ログの概要](../../landing/governance-privacy-security/audit-logs/overview.md).

## アラート {#alerts}

Experience Platform では、様々な Platform アクティビティに関するイベントベースのアラートを登録できます。Platform ユーザーインターフェイスの「[!UICONTROL アラート]」タブを使用して、様々なアラートルールを購読し、UI 内または電子メール通知を通じてアラートメッセージを受け取るように選択できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しいアラートルール | データの取り込みに関連するソースで、2 つの新しいアラートルールを使用できるようになりました。 更新されたアラートタイプのリストの概要については、[アラートルール](../../observability/alerts/rules.md)を参照してください。 |

Platform のアラートについて詳しくは、[アラートの概要](../../observability/alerts/overview.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| B2B を使用する際に新しいソースが利用できるようになりました | B2B の使用例に対して、Platform で利用可能なすべてのソースを使用できるようになりました。 詳しくは、 [ソースカタログ](../../sources/home.md) を参照してください。 |
| 新しいの一般リリース [!DNL Oracle Eloqua] ソース | これで、 [!DNL Oracle Eloqua] データをシームレスに取り込むためのソース [!DNL Oracle Eloqua] インスタンス（アカウント、キャンペーン、連絡先）を Platform に送信します。 詳しくは、 [作成 [!DNL Oracle Eloqua] ソース接続](../../sources/connectors/oracle-eloqua.md) を参照してください。 |
| の API 強化 [!DNL Data Landing Zone] | この [!DNL Data Landing Zone] ソースで、 [!DNL Flow Service] API 詳しくは、 [作成 [!DNL Data Landing Zone] ソース接続](../../sources/tutorials/api/create/cloud-storage/data-landing-zone.md) を参照してください。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
