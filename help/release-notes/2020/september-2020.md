---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2020 年 9 月 9 日
doc-type: release notes
last-update: September 8, 2020
author: crhoades, ens25212
exl-id: bf401f3a-b088-4cbd-9a64-224294b797b9
source-git-commit: 03e7863f38b882a2fbf6ba0de1755e1924e8e228
workflow-type: tm+mt
source-wordcount: '859'
ht-degree: 36%

---

# Adobe Experience Platform リリースノート

**リリース日：2020 年 9 月 9 日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

- [データガバナンス](#governance)
- [[!DNL Destinations]](#destinations)
- [[!DNL Observability Insights]](#observability)
- [[!DNL Privacy Service]](#privacy)
- [[!DNL Real-time Customer Profile]](#profile)
- [[!DNL Segmentation Service]](#segmentation)
- [[!DNL Sources]](#sources)

## データガバナンス {#governance}

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。内で重要な役割を果たします [!DNL Experience Platform] 様々なレベル（カタログ化、データ系列、データ使用のラベル付け、データアクセスポリシー、マーケティングアクションのデータに対するアクセス制御など）で使用できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| データセットのラベル付け UI の強化 | 大きなスキーマでの作業を容易にするために、データセットのラベル付け UI に、次の新しい並べ替えおよびフィルタリングコントロールが追加されました。 <ul><li>完全なスキーマパスに基づいて、アルファベット順にフィールドを並べ替えます。</li><li>フィールドパス名で部分検索を実行します。</li><li>ラベル、選択したラベル、またはラベルカテゴリのないフィールドをフィルタリングします。</li></ul> |

このサービスについて詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。

## 宛先 {#destinations}

In [Real-time Customer Data Platform](../../rtcdp/overview.md)の宛先は、宛先プラットフォームとの事前に構築された統合であり、これらのパートナーに対するデータをシームレスにアクティブ化します。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| UX の改善 | インラインテーブルのアクションにアクセスして、データの追加、スケジュール設定の編集、セグメントの追加などの主要なアクションに容易にアクセスできます。 詳しくは、 [宛先 workspace](../../destinations/ui/destinations-workspace.md) ドキュメントを参照してください。 |

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## [!DNL Observability Insights] {#observability}

[!DNL Observability Insights] では、統計指標とイベント通知を使用して、Adobe Experience Platform上のアクティビティを監視できます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| Adobe I/Oイベントの通知 | [!DNL Observability Insights] では、Adobe I/Oイベントを利用して、複数のイベントサービス用のExperience Platform通知を作成します。 通知ペイロードは、設定済みの Webhook に送信され、それを使用して、さらに下流のプロセスを自動化できます。 |

このサービスについて詳しくは、[[!DNL Observability Insights]  の概要](../../observability/home.md)を参照してください。

## [!DNL Privacy Service] {#privacy}

いくつかの法規制や組織の規制により、ユーザーはリクエストに応じてデータストアから個人データにアクセスしたり削除したりする権利を与えられます。 Adobe Experience Platform [!DNL Privacy Service] には、顧客からのこれらのデータリクエストを管理するのに役立つ RESTful API とユーザーインターフェイスが用意されています。 を使用 [!DNL Privacy Service]を使用すると、Adobe Experience Cloudアプリケーションから非公開または個人の顧客データに対するアクセスおよび削除のリクエストを送信でき、法規制や組織のプライバシー規制への自動コンプライアンスが容易になります。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| LGPD のサポート（ブラジル） | ブラジルの [!DNL Lei Geral de Proteção de Dados] (LGPD) 規制。 これらのジョブは、規制コードの下で追跡されます `lgpd_bra`. |

詳しくは、 [Privacy Serviceの概要](../../privacy-service/home.md) 」を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。を使用 [!DNL Real-time Customer Profile]を使用すると、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。 [!DNL Profile] では、様々な顧客データを統合ビューに統合し、顧客インタラクションごとに実用的なタイムスタンプ付きの説明を提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| プロファイルビューアー | Platform UI のプロファイルビューアが更新され、完全にカスタマイズされたダッシュボードになりました。 ユーザーは、次のタスクを実行するオプションが追加されました。 <ul><li>基本情報ウィジェットで、選択した標準およびカスタマイズされた属性を更新します。</li><li>カスタムウィジェットの作成、編集、削除</li><li>ウィジェットのサイズ変更と並べ替え</li></ul> |

詳しくは、 [!DNL Real-time Customer Profile]を操作するためのチュートリアルやベストプラクティスを含む [!DNL Profile] データを読んでください [リアルタイム顧客プロファイルの概要](../../profile/home.md).

## セグメント化サービス {#segmentation}

Adobe Experience Platform セグメント化サービスは、セグメントを作成し、[!DNL Real-time Customer Profile] データからオーディエンスを生成できるユーザーインターフェイスおよび RESTful API を提供します。これらのセグメントは、[!DNL Platform] 上で一元的に設定および管理され、アドビのアプリケーションから簡単にアクセスできます。

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| ジョブの書き出し | フラグが追加され、書き出しジョブの一部としてセグメントを評価できるようになりました。 その結果、ユーザーはセグメント化と書き出しの両方を 1 つのジョブで実行できます。 |
| 結合ポリシー | 1 つのバッチセグメント化ジョブに複数の結合ポリシーを含めることができます。 |

詳しくは、 [!DNL Segmentation Service]詳しくは、 [セグメント化の概要](../../segmentation/home.md)

## ソース {#sources}

Adobe Experience Platformで外部ソースからデータを取り込みながら、を使用してデータの構造化、ラベル付け、拡張をおこなうことができます。 [!DNL Platform] サービス。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 自動マッピング | [!DNL Platform] は、ユーザーが選択したターゲットスキーマまたはデータセットに基づいて、データ取り込みワークフロー中の自動マッピングに関するインテリジェントな推奨事項を提供します。 柔軟な自動マッピングルールは、使用例に合わせて手動で調整できます。 |
| UX の改善 | インラインテーブルのアクションにアクセスして、データの追加、スケジュール設定の編集、セグメントの追加などの主要なアクションに容易にアクセスできます。 詳しくは、 [データフローの監視](../../sources/tutorials/ui/monitor.md) ドキュメントを参照してください。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
