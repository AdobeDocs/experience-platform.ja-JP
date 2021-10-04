---
title: Adobe Experience Platform リリースノート
description: Experience Platform リリースノート 2020 年 9 月 9 日
doc-type: release notes
last-update: September 8, 2020
author: crhoades, ens25212
exl-id: bf401f3a-b088-4cbd-9a64-224294b797b9
source-git-commit: a455134a45137b171636d6525ce9124bc95f4335
workflow-type: tm+mt
source-wordcount: '855'
ht-degree: 35%

---

# Adobe Experience Platform リリースノート

**リリース日：2020年9月9日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

- [[!DNL Data Governance]](#governance)
- [[!DNL Destinations]](#destinations)
- [[!DNL Observability Insights]](#observability)
- [[!DNL Privacy Service]](#privacy)
- [[!DNL Real-time Customer Profile]](#profile)
- [[!DNL Segmentation Service]](#segmentation)
- [[!DNL Sources]](#sources)

## [!DNL Data Governance] {#governance}

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。[!DNL Experience Platform] 内の様々なレベルで重要な役割を果たします。例えば、カタログ化、データ系列、データ使用のラベル付け、データアクセスポリシー、マーケティングアクションのデータに対するアクセス制御などです。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| データセットのラベル付け UI の強化 | データセットのラベル付け UI に、大きなスキーマでの作業を容易にする、次の新しい並べ替えおよびフィルタリングコントロールが追加されました。 <ul><li>完全なスキーマパスに基づいて、アルファベット順にフィールドを並べ替えます。</li><li>フィールドパス名に対して部分検索を実行する。</li><li>ラベルのないフィールド、選択したラベル、またはラベルカテゴリのあるフィールドをフィルターします。</li></ul> |

このサービスについて詳しくは、「[データガバナンスの概要](../../data-governance/home.md)」を参照してください。

## 宛先 {#destinations}

[ リアルタイム顧客データプラットフォーム ](../../rtcdp/overview.md) では、宛先は、宛先プラットフォームとの事前に構築された統合で、これらのパートナーに対するデータをシームレスにアクティブ化します。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| UX の改善 | ユーザーは、インラインテーブルのアクションにアクセスして、データの追加、スケジュール設定の編集、セグメントの追加など、主要なアクションに容易にアクセスできます。 詳しくは、[ 宛先のワークスペース ](../../destinations/ui/destinations-workspace.md) ドキュメントを参照してください。 |

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## [!DNL Observability Insights] {#observability}

[!DNL Observability Insights] では、統計指標とイベント通知を使用して、Adobe Experience Platform上のアクティビティを監視できます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| Adobe I/Oイベントの通知 | [!DNL Observability Insights] では、Adobe I/Oイベントを利用して、複数のExperience Platformサービスのイベント通知を作成します。通知ペイロードは、設定済みの Webhook に送信され、それを使用して、さらに下流のプロセスを自動化できます。 |

このサービスについて詳しくは、[[!DNL Observability Insights]  の概要](../../observability/home.md)を参照してください。

## [!DNL Privacy Service] {#privacy}

いくつかの法規制や組織の規制により、ユーザーはリクエストに応じてデータストアから個人データにアクセスしたり、削除したりする権利が与えられます。 Adobe Experience Platform [!DNL Privacy Service] は、顧客からのこれらのデータリクエストを管理するのに役立つ RESTful API とユーザーインターフェイスを提供します。 [!DNL Privacy Service] を使用すると、Adobe Experience Cloudアプリケーションから個人または個人の顧客データに対するアクセスおよび削除のリクエストを送信でき、法規制や組織のプライバシー規制への自動準拠を促進できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| LGPD のサポート（ブラジル） | ブラジルの [!DNL Lei Geral de Proteção de Dados] (LGPD) 規制の下で、プライバシージョブを作成できるようになりました。 これらのジョブは、規制コード `lgpd_bra` で追跡されます。 |

このサービスの詳細については、[Privacy Serviceの概要 ](../../privacy-service/home.md) を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、いつでもどこでもブランドとのやり取りが顧客に対して調整され、一貫性と関連性のあるエクスペリエンスを提供できます。[!DNL Real-time Customer Profile] では、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせた、各顧客の全体像を確認できます。 [!DNL Profile] では、様々な顧客データを統合ビューに統合し、顧客とのやり取りごとに実用的なタイムスタンプ付きの説明を提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| プロファイルビューアー | Platform UI のプロファイルビューアが更新され、完全なカスタマイズを含むダッシュボードになりました。 ユーザーは、次のタスクを実行するオプションが追加されました。 <ul><li>基本情報ウィジェットで、選択した標準属性とカスタマイズ属性を更新します。</li><li>カスタムウィジェットの作成、編集、削除</li><li>ウィジェットのサイズ変更と並べ替え</li></ul> |

[!DNL Profile] データを扱うためのチュートリアルやベストプラクティスなど、[!DNL Real-time Customer Profile] の詳細については、[ リアルタイム顧客プロファイルの概要 ](../../profile/home.md) を参照してください。

## セグメント化サービス {#segmentation}

Adobe Experience Platform セグメント化サービスは、セグメントを作成し、[!DNL Real-time Customer Profile] データからオーディエンスを生成できるユーザーインターフェイスおよび RESTful API を提供します。これらのセグメントは、[!DNL Platform] 上で一元的に設定および管理され、アドビのアプリケーションから簡単にアクセスできます。

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| ジョブの書き出し | 書き出しジョブの一部としてセグメントを評価できるフラグが追加されました。 その結果、ユーザーはセグメント化とエクスポートの両方を 1 つのジョブで実行できます。 |
| 結合ポリシー | 1 つのバッチセグメント化ジョブに複数の結合ポリシーを含めることができます。 |

[!DNL Segmentation Service] の詳細については、[ セグメント化の概要 ](../../segmentation/home.md) を参照してください。

## ソース {#sources}

Adobe Experience Platformでは、外部ソースからデータを取り込みながら、[!DNL Platform] サービスを使用して、そのデータの構造化、ラベル付け、拡張をおこなうことができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理をおこなうことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 自動マッピング | [!DNL Platform] は、ユーザーが選択したターゲットスキーマまたはデータセットに基づいて、データ取り込みワークフロー中の自動マッピングに関するインテリジェントな推奨事項を提供します。使用例に合わせて柔軟な自動マッピングルールを手動で調整できます。 |
| UX の改善 | ユーザーは、インラインテーブルのアクションにアクセスして、データの追加、スケジュール設定の編集、セグメントの追加など、主要なアクションに容易にアクセスできます。 詳しくは、[ データフローの監視 ](../../sources/tutorials/ui/monitor.md) ドキュメントを参照してください。 |

ソースについて詳しくは、 [ソースの概要](../../sources/home.md) を参照してください。
