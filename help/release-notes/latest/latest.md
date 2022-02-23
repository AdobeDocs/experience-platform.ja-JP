---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
source-git-commit: 3a81a6f41b1e6acfe0e79dd56f01cf1bcd99417c
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 41%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 2 月 23 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [データの収集](#data-collection)
- [[!DNL Data Prep]](#data-prep)
- [[!DNL Identity Service]](#identity)
- [ソース](#sources)

## データの収集 {#data-collection}

Platform は、クライアント側の顧客体験データを収集し、Adobe Experience Platform Edge Network に送信して、Adobeや非Adobeの宛先にエンリッチメント、変換、配布できるテクノロジースイートを提供します。

**新機能**

| 機能 | 説明 |
| --- | --- |
| データストリーム設定の UI ワークフローの改善 | データ収集 UI で新しいデータストリームを作成するためのワークフローが更新されました。 データストリームにサービスを追加する場合、アクセス権のあるサービスのみがオプションのリストに含まれます。 詳しくは、 [データストリームの設定](../../edge/fundamentals/datastreams.md) を参照してください。 |
| データ収集用のデータ準備 | Adobe Experience Platform Web SDK を使用している場合、 Data Prep 機能を利用して、サーバー側の Experience Data Model(XDM) にデータをマッピングできるようになりました。 詳しくは、 [データ収集用のデータ準備](../../edge/fundamentals/datastreams.md#data-prep) （「データストリーム」ガイド）を参照してください。 |
| ファーストパーティデバイス ID | Platform Web SDK を使用して顧客データを収集する際に、独自のデバイス ID をAdobe Experience Platform Edge Network に送信できるようになりました。これにより、サードパーティ cookie の有効期間に関する最近のブラウザー制限の回避策を提供できます。 詳しくは、 [ファーストパーティデバイス ID](../../edge/identity/first-party-device-ids.md) を参照してください。 |

Platform でのデータ収集について詳しくは、 [データ収集の概要](../../collection/home.md).

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータをマッピング、変換および検証できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Data Prep] Adobe Analyticsソースコネクタのサポート | Adobe Analyticsソースコネクタで Data Prep 機能がサポートされ、データフローの作成時に Analytics レポートスイートデータをターゲット XDM スキーマにマッピングできるようになりました。 に関するチュートリアルを参照してください。 [Analytics ソースコネクタの作成](../../sources/tutorials/ui/create/adobe-applications/analytics.md) を参照してください。 |

[!DNL Data Prep] について詳しくは、[[!DNL Data Prep]  の概要](../../data-prep/home.md)を参照してください。

## [!DNL Identity Service] {#identity}

関連性のあるデジタルエクスペリエンスを提供するには、顧客を完全に理解しておく必要があります。顧客データが異なる複数のシステムに断片化されており、そのため各顧客が複数の「ID」を持つと考えられる場合、顧客を理解するのはさらに困難になります。

Adobe Experience Platform [!DNL Identity Service] を使用すると、デバイスやシステム間で ID を結び付けることで、顧客とその行動をより良く把握でき、効果的な個人のデジタルエクスペリエンスをリアルタイムで提供できます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| 新しい権限： `view-identity-graph` | これで、 `view-identity-graph` 組織内のユーザーが id グラフデータを表示できるかどうかを制御する権限。 この権限を持たないユーザーは、UI で ID グラフビューアにアクセスすることは禁止されます [!DNL Identity Service] ID を返す API。 詳しくは、 [アクセス制御の概要](../../access-control/home.md) 権限の詳細を参照してください。 |

[!DNL Identity Service] の一般的な情報について詳しくは、[ID サービスの概要](../../identity-service/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| ベータ版ソースの一般公開（GA） | 以下のソースがベータ版から一般公開（GA）に昇格しました。 <ul><li>[[!DNL Mailchimp Campaigns]](../../sources/connectors/marketing-automation/mailchimp.md)</li><li>[[!DNL Mailchimp Members]](../../sources/connectors/marketing-automation/mailchimp.md)</li><li>[[!DNL Zoho CRM]](../../sources/connectors/crm/zoho.md)</li></ul> |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
