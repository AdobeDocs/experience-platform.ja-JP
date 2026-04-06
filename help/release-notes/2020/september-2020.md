---
title: Adobe Experience Platform リリースノート 2020年9月
description: Adobe Experience Platform の 2020年9月のリリースノート。
doc-type: release notes
last-update: September 8, 2020
author: crhoades, ens25212
exl-id: bf401f3a-b088-4cbd-9a64-224294b797b9
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '866'
ht-degree: 38%

---

# Adobe Experience Platform リリースノート

**リリース日：2020年9月9日（PT）**

Adobe Experience Platform の既存の機能のアップデート：

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
| データセットラベル付けUIの機能強化 | 大規模なスキーマの操作を容易にするために、いくつかの新しい並べ替えとフィルタリング制御がデータセットラベル付けUIに追加されました。 <ul><li>完全なスキーマパスに基づいて、アルファベット順でフィールドを並べ替えます。</li><li>フィールドパス名に対して部分検索を実行します。</li><li>ラベルなし、選択したラベル、またはラベルカテゴリを含むフィールドをフィルタリングします。</li></ul> |

このサービスについて詳しくは、[データガバナンスの概要](../../data-governance/home.md)を参照してください。

## 宛先 {#destinations}

[Real-Time Customer Data Platform](../../rtcdp/overview.md)では、配信先は配信先プラットフォームとの事前定義済みの統合であり、それらのパートナーに対してシームレスにデータをアクティベートできます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| UXの改善 | ユーザーは、インラインテーブルアクションにアクセスして、データの追加、スケジュールの編集、セグメントの追加などのプライマリアクションに簡単にアクセスできます。 詳しくは、[宛先ワークスペース &#x200B;](../../destinations/ui/destinations-workspace.md) ドキュメントを参照してください。 |

詳しくは、[宛先の概要](../../destinations/home.md)を参照してください。

## [!DNL Observability Insights] {#observability}

[!DNL Observability Insights]では、統計指標とイベント通知を使用して、Adobe Experience Platform上のアクティビティをモニターできます。

**新機能**

| 機能 | 説明 |
| --- | --- |
| Adobe I/O イベント通知 | [!DNL Observability Insights]は、Adobe I/O Eventsを活用して、複数のExperience Platform サービスのイベント通知を作成します。 通知ペイロードは設定されたWebhookに送信され、それを使用してさらに下流プロセスを自動化できます。 |

サービスの詳細については、[[!DNL Observability Insights] 概要](../../observability/home.md)を参照してください。

## [!DNL Privacy Service] {#privacy}

いくつかの法的および組織的規制により、ユーザーはリクエストに応じてデータストアから個人データにアクセスしたり削除したりできます。 Adobe Experience Platform [!DNL Privacy Service]には、お客様からのこれらのデータリクエストの管理に役立つRESTful APIとユーザーインターフェイスが用意されています。 [!DNL Privacy Service]を使用すると、Adobe Experience Cloud アプリケーションから個人または個人の顧客データにアクセスして削除するリクエストを送信でき、法的および組織のプライバシー規制への自動コンプライアンスが容易になります。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| LGPD のサポート（ブラジル） | ブラジルの [!DNL Lei Geral de Proteção de Dados]（LGPD）規制の下でプライバシー関連のジョブを作成できるようになりました。これらのジョブは、規制コード `lgpd_bra` の下で追跡されます。 |

サービスの詳細については、[Privacy Serviceの概要](../../privacy-service/home.md)を参照してください。

## リアルタイム顧客プロファイル {#profile}

Adobe Experience Platform を使用すると、顧客がいつどこからブランドとやり取りしても、顧客に合わせて調整された、一貫性と関連性のある体験を提供できます。[!DNL Real-Time Customer Profile]を使用すると、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルからのデータを組み合わせた個々の顧客の全体像を表示できます。 [!DNL Profile]では、異なる顧客データを統合して、すべての顧客インタラクションの実用的なタイムスタンプ付きアカウントを提供する統合ビューを作成できます。

| 機能 | 説明 |
| ------- | ----------- |
| プロファイルビューアー | Experience Platform UIのプロファイルビューアが、完全にカスタマイズされたダッシュボードに更新されました。 ユーザーには、次のタスクを実行するオプションが追加されました。 <ul><li>基本情報ウィジェットで選択した標準属性とカスタマイズされた属性を更新します。</li><li>カスタムウィジェットの作成、編集および削除</li><li>ウィジェットのサイズ変更と並べ替え</li></ul> |

[!DNL Real-Time Customer Profile] データの操作に関するチュートリアルやベストプラクティスなど、[!DNL Profile]の詳細については、[&#x200B; リアルタイム顧客プロファイルの概要](../../profile/home.md)を参照してください。

## セグメント化サービス {#segmentation}

Adobe Experience Platform セグメント化サービスは、セグメントを作成し、[!DNL Real-Time Customer Profile] データからオーディエンスを生成できるユーザーインターフェイスおよび RESTful API を提供します。これらのセグメントは、[!DNL Experience Platform] 上で一元的に設定および管理され、アドビのアプリケーションから簡単にアクセスできます。

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。セグメントは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて作成できます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| ジョブの書き出し | 書き出しジョブの一部としてセグメントを評価できるように、フラグが追加されました。 その結果、ユーザーは1つのジョブでセグメント化と書き出しの両方を実行できます。 |
| 結合ポリシー | 1つのバッチセグメント化ジョブに複数の結合ポリシーを含めることができます。 |

[!DNL Segmentation Service]について詳しくは、[&#x200B; セグメント化の概要](../../segmentation/home.md)を参照してください

## ソース {#sources}

Adobe Experience Platformでは、[!DNL Experience Platform] サービスを使用して外部ソースからデータを取り込めますが、そのデータを構造化、ラベル付け、強化することができます。 アドビアプリケーション、クラウドベースのストレージ、サードパーティのソフトウェア、CRM など、様々なソースからデータを取得することができます。

[!DNL Experience Platform]には、RESTful APIとインタラクティブ UIが用意されており、さまざまなデータプロバイダーのソース接続を簡単に設定できます。 これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新機能**

| 機能 | 説明 |
| ------- | ----------- |
| 自動マッピング | [!DNL Experience Platform]は、ユーザーが選択したターゲット スキーマまたはデータセットに基づいて、データ取り込みワークフロー中に自動マッピングに関するインテリジェントな推奨事項を提供します。 ユースケースに合わせて、柔軟な自動マッピングルールを手動で調整できます。 |
| UXの改善 | ユーザーは、インラインテーブルアクションにアクセスして、データの追加、スケジュールの編集、セグメントの追加などのプライマリアクションに簡単にアクセスできます。 詳しくは、[&#x200B; データフローの監視](../../sources/tutorials/ui/monitor.md) ドキュメントを参照してください。 |

ソースについて詳しくは、[ソースの概要](../../sources/home.md)を参照してください。
