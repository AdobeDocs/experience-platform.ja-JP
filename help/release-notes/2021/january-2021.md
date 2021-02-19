---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2021 年 1 月 27 日
doc-type: release notes
last-update: January 27, 2021
author: ens60013
translation-type: tm+mt
source-git-commit: 2e3a6acbfaa7f733a9843068c00f31f0b7f535b6
workflow-type: tm+mt
source-wordcount: '490'
ht-degree: 36%

---


# Adobe Experience Platform リリースノート

**リリース日：2021 年 1 月 27 日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Prep]](#data-prep)
- [[!DNL Destinations]](#destinations)
- [[!DNL Sources]](#sources)

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] データエンジニアがエクスペリエンスデータモデル(XDM)との間でデータのマッピング、変換、検証を行えるようにします。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 正規式関数 | [!DNL Data Prep] マッパーで、正規式に基づく入力フィールドの一部の一致と抽出がサポートされるようになりました。 |

詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、目的のプラットフォームと事前にビルドされた統合機能で、Adobe Experience Platformからのデータをシームレスにアクティベーションできます。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [!DNL Azure Blob] | [!DNL Azure Blob] は、Microsoftのクラウド向けオブジェクトストレージソリューションです。 |

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 高度なIDの一致 | 外部ID、電話番号、モバイルデバイスIDなど、追加のID照合のサポートを追加し、[!DNL Facebook Custom Audiences]と[!DNL Google Customer Match]のオーディエンス一致率機能を強化しました。 詳しくは、次のドキュメントを参照してください。 <ul><li>[Facebook の宛先](../../destinations/catalog/social/facebook.md)</li><li>[Google Customer Matchのリンク先](../../destinations/catalog/advertising/google-customer-match.md)</li><li>[宛先へのプロファイルとセグメントのアクティブ化](../../destinations/ui/activate-destinations.md)</li></ul> |

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Audience Managerソースコネクタの機能強化 | Audience ManagerからPlatformに取り込む個々のファーストパーティセグメントをフィルタリングおよび選択でき、ファーストパーティの特性をフィルタリングできるようになりました。 詳細については、[Audience Managerソースコネクタ](../../sources/tutorials/ui/create/adobe-applications/audience-manager.md)の作成に関するチュートリアルを参照してください。 |
| [!DNL Google BigQuery] ソースコネクタの強化 | [!DNL BigQuery]ソースコネクタを使用して、1回のフロー実行で10 GBを超えるファイルを取り込めるようになりました。 詳しくは、[[!DNL BigQuery] ソースコネクタの概要](../../sources/connectors/databases/bigquery.md)を参照してください。 |
| クラウドストレージ用の複雑なデータ型のサポート | クラウドストレージソースコネクタを使用する場合、JSONファイル内の配列などの複雑なデータ型を取り込めるようになりました。 詳しくは、UI](../../sources/tutorials/ui/dataflow/batch/cloud-storage.md)または[でのクラウドストレージのデータフロー[の作成に関するチュートリアル（ [!DNL Flow Service] API](../../sources/tutorials/api/collect/cloud-storage.md)を使用）を参照してください。 |
| [!DNL Microsoft Dynamics]ソースのサービスプリンシパルキーベースの認証のサポート | パスワードベースの認証の代わりに、サービスプリンシパルキーを使用して[!DNL Dynamics]アカウントに対して認証できるようになりました。 詳しくは、[[!DNL Dynamics] ソースコネクタの概要](../../sources/connectors/crm/ms-dynamics.md)を参照してください。 |
| クラウドストレージソースでのカスタム区切り文字のUIのサポート | カンマ(`,`)、タブ(`\t`)、パイプ(`|`)などのカスタムの列区切り文字を設定して、UIから区切りファイルを収集できるようになりました。 詳しくは、[クラウドストレージソースコネクタ](../../sources/tutorials/ui/dataflow/batch/cloud-storage.md)を使用したデータフローの作成のチュートリアルを参照してください |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
