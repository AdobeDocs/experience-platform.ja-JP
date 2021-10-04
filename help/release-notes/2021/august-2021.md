---
title: Adobe Experience Platform リリースノート
description: 2021年8月25日の Experience Platform リリースノート。
doc-type: release notes
last-update: August 25, 2021
author: ens28527
exl-id: 0513b9dc-b16c-43b3-8e17-4be4499308d4
source-git-commit: e9d5f24bec8cd2793ce30245b46c1d912bf17cc7
workflow-type: tm+mt
source-wordcount: '709'
ht-degree: 100%

---

# Adobe Experience Platform リリースノート

**リリース日：2021年8月25日**

Adobe Experience Platform の既存の機能のアップデート：

- [宛先](#destinations)
- [Observability Insights](#observability)
- [リアルタイム顧客プロファイル](#profile)
- [ソース](#sources)

## 宛先 {#destinations}

宛先は、Adobe Experience Platform からのデータの円滑なアクティベーションを可能にする、宛先プラットフォームとの事前定義済みの統合です。宛先を使用して、クロスチャネルマーケティングキャンペーン、電子メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新しい宛先**

| 宛先 | 説明 |
| ----------- | ----------- |
| [[!DNL Airship Attributes]](../../destinations/catalog/mobile-engagement/airship-attributes.md) | Airship Attributes の宛先（以前はベータ版）が一般公開されました。 |
| [[!DNL Airship Tags]](../../destinations/catalog/mobile-engagement/airship-tags.md) | Airship タグの宛先（以前はベータ版）が一般公開されました。 |
| [[!DNL Braze]](../../destinations/catalog/mobile-engagement/braze.md) | Braze の宛先（以前はベータ版）が一般公開されました。 |
| [[!DNL Pinterest Customer List]](../../destinations/catalog/advertising/pinterest.md) | Pinterest の顧客リストの宛先を使用すると、顧客リスト、サイトを訪問した人物、または Pinterest でコンテンツに対して何らかのアクションを起こしたことがある人物からオーディエンスを作成できます。 |
| [[!DNL Twitter Custom Audiences]](../../destinations/catalog/social/twitter.md) | Twitter で既存のフォロワーと顧客をターゲットに設定し、Adobe Experience Platform 内に構築したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します。 |
| [[!DNL Verizon Media/Yahoo DataX]](../../destinations/catalog/advertising/datax.md) | DataX は Verizon Media／Yahoo の集約インフラストラクチャです。安全で自動化されたスケーラブルな方法で Verizon Media／Yahoo が外部パートナーとデータを交換できるよう様々なコンポーネントをホストしています。 |

**新機能**

| 機能 | 説明 |
| --- | --- |
| [[!DNL Destination SDK]](../../destinations/destination-sdk/overview.md) | Adobe Experience Platform Destination SDK は、Experience Platform の宛先統合パターンを設定し、選択したデータと認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントに配信できるようにする設定 API のスイートです。設定は Experience Platform に保存され、API 経由で取得することで追加アップデートを入手できます。 |
| [宛先の使いやすさの向上](../../destinations/ui/activation-overview.md) | 宛先の使いやすさを向上しました。これにより、マーケターは既存の宛先に対するセグメントをシームレスにアクティブ化できます。 |

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## 観察性インサイト {#observability}

Observability Insights では、統計的な指標とイベント通知を使用して Platform アクティビティを監視できます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| アラート | Platform 上で実行されるワークフローに関連する、重要なアラートを登録できるようになりました。特定のアラートルールを登録すると、重要なライフサイクルイベント（データ取り込みの成功など）が発生した場合や、注意が必要な問題（取り込みフローの失敗やセグメントジョブの予想以上の時間など）が発生した場合に、UI 内通知とメールが届きます。詳しくは、[アラートの概要](../../observability/alerts/overview.md)を参照してください。 |

このサービスについて詳しくは、[Observability Insights の概要](../../observability/home.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きのアカウントを提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| 結合ポリシーまたは ID によるプロファイルの参照 | Experience Platform でプロファイルを参照する際、結合ポリシー別に参照し、選択した結合ポリシーに基づいて 20 件のサンプルプロファイルをプレビューできるようになりました。また、ID で参照して、ID 名前空間と関連する ID 値を使用して特定のプロファイルを検索することもできます。詳しくは、[リアルタイム顧客プロファイル UI ガイド](../../profile/ui/user-guide.md)を参照してください。 |

プロファイルデータを操作するためのチュートリアルやベストプラクティスなど、リアルタイム顧客プロファイルの詳細については、[リアルタイム顧客プロファイルの概要](../../profile/home.md)を参照してください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

| 機能 | 説明 |
| ------- | ----------- |
| ローカルファイルアップロードソースコネクタ | ファイル取り込みカテゴリの名前がローカルシステムに変更され、ローカルファイルアップロードコネクタを使用してローカルファイルを Platform に直接取り込めるようになりました。このコネクタを介して取り込まれたデータは、監視ダッシュボードで監視できます。詳しくは、[ローカルファイルアップロードソースの概要](../../sources/connectors/local-system/local-file-upload.md)を参照してください。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
