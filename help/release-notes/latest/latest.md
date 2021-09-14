---
title: Adobe Experience Platform リリースノート
description: Adobe Experience Platformの最新のリリースノートです。
exl-id: 8f2c9bf8-1487-46e4-993b-bd9b63774cab
source-git-commit: e9d5f24bec8cd2793ce30245b46c1d912bf17cc7
workflow-type: tm+mt
source-wordcount: '711'
ht-degree: 36%

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
| [[!DNL Airship Attributes]](../../destinations/catalog/mobile-engagement/airship-attributes.md) | Airship Attributesの宛先（以前はベータ版）が一般に使用できるようになりました。 |
| [[!DNL Airship Tags]](../../destinations/catalog/mobile-engagement/airship-tags.md) | 以前はベータ版だった飛行船タグの宛先が、一般に使用できるようになりました。 |
| [[!DNL Braze]](../../destinations/catalog/mobile-engagement/braze.md) | 以前はベータ版だったBrazeの宛先が一般的に使用できるようになりました。 |
| [[!DNL Pinterest Customer List]](../../destinations/catalog/advertising/pinterest.md) | pinterestの顧客リストの宛先を使用すると、顧客リスト、サイトを訪問した人、またはPinterestでコンテンツに対して何らかのアクションを起こした人からオーディエンスを作成できます。 |
| [[!DNL Twitter Custom Audiences]](../../destinations/catalog/social/twitter.md) | twitterで既存のフォロワーと顧客をターゲットに設定し、Adobe Experience Platform内で作成したオーディエンスをアクティブ化して、関連するリマーケティングキャンペーンを作成します。 |
| [[!DNL Verizon Media/Yahoo DataX]](../../destinations/catalog/advertising/datax.md) | DataXは、Verizon Media/Yahooが外部パートナーと安全で自動化された拡張性の高い方法でデータを交換できる様々なコンポーネントをホストする、Verizon Media/Yahooの集約インフラストラクチャです。 |

**新機能**

| 機能 | 説明 |
| --- | --- |
| [[!DNL Destination SDK]](../../destinations/destination-sdk/overview.md) | Adobe Experience Platform Destination SDKは、選択したデータおよび認証形式に基づいて、オーディエンスとプロファイルのデータをエンドポイントに配信するためのExperience Platformの宛先統合パターンを設定できる設定APIのスイートです。 設定はExperience Platformに保存され、APIを介して取得して追加の更新をおこなうことができます。 |
| [宛先のユーザビリティの改善](../../destinations/ui/activation-overview.md) | 宛先のユーザビリティの改善により、マーケターは既存の宛先に対するセグメントをシームレスにアクティブ化できます。 |

宛先の一般的な情報については、[宛先の概要](../../destinations/home.md)を参照してください。

## 観察性インサイト {#observability}

観察性インサイトを使用すると、統計指標とイベント通知を使用してPlatformアクティビティを監視できます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| アラート | Platform上で実行されるワークフローに関連する重要なアラートを購読できるようになりました。 特定のアラートルールをサブスクライブした後、重要なライフサイクルイベント（データ取り込みの成功など）が発生した場合や、注意が必要な問題（取り込みフローの失敗やセグメントジョブの予想以上の時間など）が発生した場合に、UI内通知と電子メールを受け取ります。 詳しくは、[アラートの概要](../../observability/alerts/overview.md)を参照してください。 |

このサービスについて詳しくは、「 [観察性インサイトの概要](../../observability/home.md) 」を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。リアルタイム顧客プロファイルでは、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。プロファイルを使用すると、顧客データを統合ビューに統合し、顧客とのやり取りごとに実用的なタイムスタンプ付きの説明を提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| 結合ポリシーまたはIDによるプロファイルの参照 | Experience Platformでプロファイルを参照する際に、結合ポリシーで参照して、選択した結合ポリシーに基づいて20件のサンプルプロファイルをプレビューできるようになりました。 また、ID名前空間と関連するID値を使用して特定のプロファイルを検索するために、IDで参照することもできます。 詳しくは、[リアルタイム顧客プロファイルUIガイド](../../profile/ui/user-guide.md)を参照してください。 |

プロファイルデータを操作するためのチュートリアルやベストプラクティスなど、リアルタイム顧客プロファイルの詳細については、まず「[リアルタイム顧客プロファイルの概要](../../profile/home.md)」をお読みください。

## ソース {#sources}

Adobe Experience Platform では、外部ソースからデータを取り込むときに、Platform サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。アドビのアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM システムなど、様々なソースからデータを取り込むことができます。

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

| 機能 | 説明 |
| ------- | ----------- |
| ローカルファイルアップロードソースコネクタ | ファイル取り込みカテゴリの名前がローカルシステムに変更され、ローカルファイルアップロードコネクタを使用してローカルファイルをPlatformに直接取り込めるようになりました。 このコネクタを使用して取り込まれるデータは、監視ダッシュボードで監視できます。 詳しくは、[ローカルファイルアップロードソースの概要](../../sources/connectors/local-system/local-file-upload.md)を参照してください。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
