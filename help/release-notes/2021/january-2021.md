---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2021 年 1 月 27 日（PT）
doc-type: release notes
last-update: January 27, 2021
author: ens60013
exl-id: 6fb92e35-922c-47ba-8cf4-44edd92acfa1
source-git-commit: 02c22453470d55236d4235c479742997e8407ef3
workflow-type: ht
source-wordcount: '713'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2021 年 1 月 27 日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Prep]](#data-prep)
- [[!DNL Destinations]](#destinations)
- [[!DNL Real-time Customer Profile]](#profile)
- [[!DNL Sources]](#sources)

## [!DNL Data Prep] {#data-prep}

[!DNL Data Prep] を使用すると、データエンジニアは Experience Data Model（XDM）との間でデータのマッピング、変換、検証をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 正規表現関数 | [!DNL Data Prep] マッパーで、正規表現に基づく入力フィールドの一部の照合と抽出がサポートされるようになりました。 |

詳しくは、 [[!DNL Data Prep]  の概要](../../data-prep/home.md) を参照してください。

## 宛先 {#destinations}

[!DNL Destinations] は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [!DNL Azure Blob] | [!DNL Azure Blob] は、Microsoft のクラウド向けオブジェクトストレージソリューションです。 |

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 高度な ID 照合 | 外部 ID、電話番号、モバイルデバイス ID などの追加 ID の照合サポートを追加し、[!DNL Facebook Custom Audiences] と [!DNL Google Customer Match] のオーディエンスマッチ率機能を強化しました。詳しくは、以下のドキュメントを参照してください。 <ul><li>[Facebook の宛先](../../destinations/catalog/social/facebook.md)</li><li>[Google カスタマーマッチの宛先](../../destinations/catalog/advertising/google-customer-match.md)</li><li>[ストリーミングセグメント書き出しの宛先に対するオーディエンスデータのアクティブ化](../../destinations/ui/activate-segment-streaming-destinations.md)</li></ul> |

詳しくは、 [宛先の概要](../../destinations/home.md) を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。[!DNL Profile] を使用すると、個別の顧客データを統合ビューに取り込み、顧客インタラクションごとにアクションにつながる、タイムスタンプ付きのアカウントを提供できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| プロファイルストアからのデータセットの削除 | Experience Platform Data Lake からデータセットを削除すると、データセットはプロファイルストアからも自動的に削除されます。プロファイルストアからデータセットを明示的に削除する削除リクエストをおこなう場合に、Profile System jobs API エンドポイントを使用する必要がなくなりました。詳しくは、 [プロファイルシステムジョブ API エンドポイントのガイド](../../profile/api/profile-system-jobs.md) を参照してください。 |
| 特定のセグメントに対する ID 名前空間の推定数 | プロファイルの推定数について、プレビュー API が次の情報をレポートするようになりました。<ul><li>特定の名前空間のセグメントにおける推定プロファイルの合計数。</li><li>特定の名前空間のプロファイル和集合スキーマにおける推定プロファイルの合計数。</li></ul>詳しくは、 [プロファイルプレビュー API エンドポイントのガイド](../../profile/api/preview-sample-status.md) を参照してください。 |

[!DNL Profile] データを操作するためのチュートリアルやベストプラクティスなど、リアルタイム顧客プロファイルの詳細については、[リアルタイム顧客プロファイルの概要](../../profile/home.md)を参照してください。

## [!DNL Sources] {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| Adobe Audience Manager ソースコネクタの機能強化 | Audience Manager から個々のファーストパーティセグメントをフィルタリングして選択し、Platform に取り込んだり、ファーストパーティの特性を除外したりできるようになりました。詳細については、 [Audience Manager ソースコネクタの作成](../../sources/tutorials/ui/create/adobe-applications/audience-manager.md) に関するチュートリアルを参照してください。 |
| [!DNL Google BigQuery] ソースコネクタの機能強化 | [!DNL BigQuery] ソースコネクタを使用して、1 回のフロー実行で 10 GB を超えるファイルを取り込めるようになりました。詳しくは、 [[!DNL BigQuery] ソースコネクタの概要](../../sources/connectors/databases/bigquery.md) を参照してください。 |
| クラウドストレージ用の複雑なデータ型のサポート | クラウドストレージソースコネクタを使用する場合、JSON ファイル内の配列などの複雑なデータ型を取り込めるようになりました。詳しくは、 [UI での](../../sources/tutorials/ui/dataflow/batch/cloud-storage.md)、または[  [!DNL Flow Service] API を使用した](../../sources/tutorials/api/collect/cloud-storage.md)クラウドストレージのデータフロー の作成に関するチュートリアルを参照してください。 |
| [!DNL Microsoft Dynamics] ソースのサービスプリンシパルキーベースの認証のサポート | パスワードベースの認証の代わりに、サービスプリンシパルキーを使用して [!DNL Dynamics] アカウントを認証できるようになりました。詳しくは、 [[!DNL Dynamics] ソースコネクタの概要](../../sources/connectors/crm/ms-dynamics.md) を参照してください。 |
| クラウドストレージソースでのカスタム区切り文字の UI のサポート | コンマ（`,`）、タブ（`\t`）、パイプ（`|`）などのカスタム列区切り記号を設定して、UI から区切りファイルを収集できるようになりました。詳しくは、 [クラウドストレージソースコネクタを使用したデータフローの作成](../../sources/tutorials/ui/dataflow/batch/cloud-storage.md) のチュートリアルを参照してください。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
