---
title: Adobe Experience Platform リリースノート（2023年2月）
description: Adobe Experience Platform の 2023年3月のリリースノート。
source-git-commit: 44075e38664c01c64e7d09f5856353bff64becb5
workflow-type: tm+mt
source-wordcount: '663'
ht-degree: 45%

---

# Adobe Experience Platform リリースノート

**リリース日：2023年3月29日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [データ収集](#data-collection)
- [データ準備](#data-prep)
- [セグメント化サービス](#segmentation)
- [ソース](#sources)

## データ収集 {#data-collection}

Adobe Experience Platform では、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Network に送信できます。そこでデータを補強して変換し、アドビまたはアドビ以外の宛先に配信できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| メタコンバージョン API（ベータ版）の新しいクイックスタートワークフロー | データコレクションのホーム画面の「はじめに」の新しいクイックスタートワークフローにアクセスできます。 この [メタ変換 API のクイックスタートワークフロー](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/meta/overview.html?lang=en#quick-start) を使用すれば、わずか数回の簡単な手順で、サーバー側から Meta へと迅速にイベントデータを収集し、転送して広告コンバージョンをおこなうことができます。 |
| [!DNL Braze] イベント転送拡張機能 | この [[!DNL Braze Track Events API]](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/braze/overview.html) イベント転送拡張機能を使用すると、Adobe Experience Platform Edge Network で取り込んだデータを活用し、に送信できます。 [!DNL Braze] を使用して、サーバー側のイベントの形式で [!DNL Braze] ユーザー追跡 API |
| [!DNL Mixpanel] イベント転送拡張機能 | この [[!DNL Mixpanel Track Events API]](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/server/braze/overview.html) 拡張機能を使用すると、イベント転送を活用してAdobe Experience Platform Edge Network でイベント情報をキャプチャし、イベント追跡 API を使用して Mixpanel に送信できます。 |

{style="table-layout:auto"}

## データ準備 {#data-prep}

Data Prep を使用すると、データエンジニアはエクスペリエンスデータモデル（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| URL 文字列のエンコードとデコードに関する新しい関数 | <ul><li>この `get_url_encoded` 関数は、URL を入力として受け取り、特殊文字を ASCII 文字に置き換えたり、エンコードしたりします。</li><li>この `get_url_decoded` 関数は、URL を入力として受け取り、ASCII 文字を特殊文字にデコードします。</li></ul> 詳しくは、 [データ準備関数ガイド](../../data-prep/functions.md). 予約文字とその対応するエンコード済み文字の包括的なリストについては、 [特殊文字](../../data-prep/functions.md#special-characters). |

Data Prep の詳細については、 [データ準備の概要](../../data-prep/home.md).

## セグメント化サービス {#segmentation}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| プロファイル指標 | プロファイル指標をより正確に表すために、メンバーシップの分類とチャーン指標は組み合わされ、24 時間にわたって計算されるようになりました。 詳しくは、 [セグメント化 UI ガイド](../../segmentation/ui/overview.md) |

{style="table-layout:auto"}

[!DNL Segmentation Service] について詳しくは、[セグメント化の概要](../../segmentation/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むことができ、Platform サービスを使用してそのデータの構造化、ラベル付け、および拡張を行うことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**更新された機能**

| 機能 | 説明 |
| --- | --- |
| [!DNL Chatlio] のベータ版の可用性 | この [!DNL Chatlio] ベータ版でのソースの利用が可能になりました。 以下を使用： [!DNL Chatlio] ストリーミングするソース [!DNL Chatlio] イベントデータからExperience Platformへ。 詳しくは、[[!DNL Chatlio] 概要](../../sources/connectors/marketing-automation/chatlio-webhook.md)を参照してください。 |
| [!DNL Customer.io] のベータ版の可用性 | この [!DNL Customer.io] ベータ版でのソースの利用が可能になりました。 以下を使用： [!DNL Customer.io] 顧客イベントデータをExperience Platformにストリーミングするソース。 詳しくは、[[!DNL Customer.io] 概要](../../sources/connectors/marketing-automation/customerio-webhook.md)を参照してください。 |
| [!DNL Pendo] のベータ版の可用性 | この [!DNL Pendo] ベータ版でのソースの利用が可能になりました。 以下を使用： [!DNL Pendo] 製品分析データをExperience Platformにストリーミングするソース。 詳しくは、[[!DNL Pendo] 概要](../../sources/connectors/analytics/pendo-webhook.md)を参照してください。 |
| ドラフトデータフローのサポート | これで、フローサービス API を使用して、データフローをドラフト状態に設定できます。 下書きのデータフローは、後で更新し、新しい情報で公開できます。 詳しくは、 [ソースのデータフローをドラフトとして設定](../../sources/tutorials/api/draft.md). |

{style="table-layout:auto"}

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
