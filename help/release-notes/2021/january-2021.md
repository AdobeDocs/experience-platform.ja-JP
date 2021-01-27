---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2021 年 1 月 27 日
doc-type: release notes
last-update: January 27, 2021
author: ens60013
translation-type: tm+mt
source-git-commit: cf70b21f3a8c02b25e5acd3be8c8feaa3f52a5e3
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 41%

---


# Adobe Experience Platform リリースノート

**リリース日：2021 年 1 月 27 日**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Prep]](#data-prep)
- [[!DNL Sources]](#sources)

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] データエンジニアがエクスペリエンスデータモデル(XDM)との間でデータのマッピング、変換、検証を行えるようにします。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 正規式関数 | [!DNL Data Prep] マッパーで、正規式に基づく入力フィールドの一部の一致と抽出がサポートされるようになりました。 |

詳しくは、[[!DNL Data Prep] 概要](../../data-prep/home.md)を参照してください。

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

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
