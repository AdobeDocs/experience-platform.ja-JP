---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platform の最新のリリースノートです。
source-git-commit: 762a4b7336f1c26b79883db9484d8f5fc7bff53c
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 62%

---

# Adobe Experience Platform リリースノート

**リリース日：2022 年 2 月 23 日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [[!DNL Data Prep]](#data-prep)
- [[!DNL Identity Service]](#identity)
- [ソース](#sources)

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

**新機能**

| 機能 | 説明 |
| --- | --- |
| 新しい権限： `view-identity-graph` | これで、 `view-identity-graph` 組織内のユーザーが id グラフデータを表示できるかどうかを制御する権限。 この権限を持たないユーザーは、UI で ID グラフビューアにアクセスすることは禁止されます [!DNL Identity Service] ID を返す API。 詳しくは、 [アクセス制御の概要](../../access-control/home.md) 権限の詳細を参照してください。 |

[!DNL Identity Service] の一般的な情報について詳しくは、[ID サービスの概要](../../identity-service/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

| 機能 | 説明 |
| --- | --- |
| ベータ版ソースの一般公開（GA） | 以下のソースがベータ版から一般公開（GA）に昇格しました。 <ul><li>[[!DNL Mailchimp Campaigns]](../../sources/connectors/marketing-automation/mailchimp.md)</li><li>[[!DNL Mailchimp Members]](../../sources/connectors/marketing-automation/mailchimp.md)</li><li>[[!DNL Zoho CRM]](../../sources/connectors/crm/zoho.md)</li></ul> |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
