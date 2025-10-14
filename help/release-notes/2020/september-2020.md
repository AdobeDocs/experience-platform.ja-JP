---
title: Adobe Experience Platform リリースノート 2020年9月
description: Adobe Experience Platform の 2020年9月のリリースノート。
doc-type: release notes
last-update: September 8, 2020
author: crhoades, ens25212
exl-id: bf401f3a-b088-4cbd-9a64-224294b797b9
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '866'
ht-degree: 38%

---

# Adobe Experience Platform リリースノート

**リリース日：2020年9月9日（PT）**

Adobe Experience Platform の既存の機能に対するアップデート：

- [データガバナンス](#governance)
- [[!DNL Destinations]](#destinations)
- [[!DNL Observability Insights]](#observability)
- [[!DNL Privacy Service]](#privacy)
- [[!DNL Real-Time Customer Profile]](#profile)
- [[!DNL Segmentation Service]](#segmentation)
- [[!DNL Sources]](#sources)

## データガバナンス {#governance}

Adobe Experience Platform データガバナンスは、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへの準拠を確保するために使用される一連の戦略とテクノロジーです。これは、[!DNL Experience Platform] において様々なレベルで重要な役割を果たします。例えば、カタログ作成、データ系列、データ使用のラベル付け、データアクセスポリシー、マーケティングアクションのデータに関するアクセス制御などです。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| データセットのラベル付け UI の強化 | 大きなスキーマを扱いやすくするために、データセットのラベル付け UI に新しい並べ替えおよびフィルタリングのコントロールがいくつか追加されました。 <ul><li>完全なスキーマパスに基づいて、アルファベット順でフィールドを並べ替えます。</li><li>フィールドパス名に対して部分検索を実行します。</li><li>ラベルのないフィールド、選択したラベル、またはラベルカテゴリをフィルターします。</li></ul> |

このサービスについて詳しくは、[データガバナンスの概要](../../data-governance/home.md)を参照してください。

## 宛先 {#destinations}

[Real-Time Customer Data Platform](../../rtcdp/overview.md) では、宛先は、宛先プラットフォームとの事前定義済みの統合であり、シームレスな方法でこれらのパートナーにデータをアクティブ化します。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| UX の向上 | ユーザーは、インラインテーブルアクションにアクセスして、データの追加、スケジュールの編集、セグメントの追加などの主要なアクションにより簡単にアクセスできます。 詳しくは、[&#x200B; 宛先ワークスペース &#x200B;](../../destinations/ui/destinations-workspace.md) ドキュメントを参照してください。 |

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## [!DNL Observability Insights] {#observability}

[!DNL Observability Insights] では、統計指標とイベント通知を使用して、Adobe Experience Platformのアクティビティを監視できます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| Adobe I/O イベントの通知 | [!DNL Observability Insights] は、Adobe I/O Eventsを活用して、複数のExperience Platform サービス用のイベント通知を作成します。 通知ペイロードは設定済みの Webhook に送信され、それを使用してさらにダウンストリームのプロセスを自動化できます。 |

サービスについて詳しくは、[[!DNL Observability Insights]  概要 &#x200B;](../../observability/home.md) を参照してください。

## [!DNL Privacy Service] {#privacy}

いくつかの法規制や組織の規制により、ユーザーは、要求に応じて、データストアから個人データにアクセスしたり、個人データを削除したりする権利が与えられています。 Adobe Experience Platform [!DNL Privacy Service] は、お客様からのこれらのデータリクエストを管理するのに役立つ RESTful API とユーザーインターフェイスを提供します。 [!DNL Privacy Service] を使用すると、Adobe Experience Cloud アプリケーションから非公開または個人的な顧客データに対するアクセスおよび削除のリクエストを送信でき、法的および組織のプライバシー規制への自動コンプライアンスが容易になります。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| LGPD のサポート（ブラジル） | ブラジルの [!DNL Lei Geral de Proteção de Dados]（LGPD）規制の下でプライバシー関連のジョブを作成できるようになりました。これらのジョブは、規制コード `lgpd_bra` の下で追跡されます。 |

このサービスについて詳しくは、[Privacy Serviceの概要 &#x200B;](../../privacy-service/home.md) を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。[!DNL Real-Time Customer Profile] を使用すると、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を把握できます。 [!DNL Profile] を使用すると、様々な顧客データを統合ビューに統合して、顧客インタラクションごとに実用的なタイムスタンプ付きの説明を提供できます。

| 機能 | 説明 |
| ------- | ----------- |
| プロファイルビューアー | Experience Platform UI のプロファイルビューアが更新されて、完全にカスタマイズできるダッシュボードになりました。 ユーザーには、次のタスクを実行するオプションが追加されました。 <ul><li>基本情報ウィジェットで選択した標準属性とカスタマイズ属性を更新します。</li><li>カスタムウィジェットの作成、編集、削除</li><li>ウィジェットのサイズ変更と並べ替え</li></ul> |

[!DNL Profile] データを操作するためのチュートリアルやベストプラクティスなど、[!DNL Real-Time Customer Profile] ークフローについて詳しくは、[&#x200B; リアルタイム顧客プロファイルの概要 &#x200B;](../../profile/home.md) を参照してください。

## セグメント化サービス {#segmentation}

Adobe Experience Platform セグメント化サービスは、セグメントを作成し、[!DNL Real-Time Customer Profile] データからオーディエンスを生成できるユーザーインターフェイスおよび RESTful API を提供します。これらのセグメントは、[!DNL Experience Platform] 上で一元的に設定および管理され、アドビのアプリケーションから簡単にアクセスできます。

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| ジョブの書き出し | セグメントをエクスポートジョブの一部として評価できるようにするためのフラグを追加しました。 その結果、ユーザーはセグメント化と書き出しの両方を 1 つのジョブで実行できます。 |
| 結合ポリシー | 1 つのバッチセグメント化ジョブに、複数の結合ポリシーを含めることができます。 |

[!DNL Segmentation Service] について詳しくは、[&#x200B; セグメント化の概要 &#x200B;](../../segmentation/home.md) を参照してください

## ソース {#sources}

Adobe Experience Platformでは、外部ソースからデータを取り込むときに、[!DNL Experience Platform] サービスを使用して、そのデータの構造化、ラベル付け、拡張を行うことができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform] は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を提供します。 これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 自動マッピング | [!DNL Experience Platform] は、ユーザーが選択したターゲットスキーマまたはデータセットに基づいて、データ取り込みワークフロー中に自動マッピングするためのインテリジェントなレコメンデーションを提供します。 ユースケースに合わせて、柔軟な自動マッピングルールを手動で調整できます。 |
| UX の向上 | ユーザーは、インラインテーブルアクションにアクセスして、データの追加、スケジュールの編集、セグメントの追加などの主要なアクションにより簡単にアクセスできます。 詳しくは、[&#x200B; データフローの監視 &#x200B;](../../sources/tutorials/ui/monitor.md) ドキュメントを参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
