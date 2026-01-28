---
title: Experience Platformのプレリリースノート
description: Adobe Experience Platformの最新のリリースノートのプレビュー。
exl-id: f2c41dc8-9255-4570-b459-4f9fc28ee58b
source-git-commit: 6fa71c48151e937f2e18d8b9761aad94eca85ade
workflow-type: tm+mt
source-wordcount: '1316'
ht-degree: 16%

---

# Adobe Experience Platformのプレリリースノート

>[!IMPORTANT]
>
>このドキュメントは、今月のリリースノートの **プレビュー** として提供することを目的としています。 リリース項目は変更される場合があり、最終リリースで追加または削除される場合があります。

>[!TIP]
>
>その他のAdobe Experience Platform アプリケーションのリリースノートについては、次のドキュメントを参照してください。
>
>- [Adobe Journey Optimizer](https://experienceleague.adobe.com/ja/docs/journey-optimizer/using/whats-new/release-notes)
>- [Adobe Journey Optimizer B2B](https://experienceleague.adobe.com/ja/docs/journey-optimizer-b2b/user/release-notes)
>- [Customer Journey Analytics](https://experienceleague.adobe.com/en/docs/analytics-platform/using/releases/pre-release-notes)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/en/docs/federated-audience-composition/using/e-release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/en/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2026 年 1 月**

Adobe Experience Platformの既存の機能に対する新機能とアップデート：

- [Agent Orchestrator](#agent-orchestrator)
- [宛先](#destinations)
- [リアルタイム顧客プロファイル](#real-time-customer-profile)
- [スキーマ](#schemas)
- [セグメント化サービス](#segmentation-service)
- [ソース](#sources)

## Agent Orchestrator {#agent-orchestrator}

Agent Orchestratorを使用すると、ワークフローを自動化し、複数のチャネルをまたいで顧客とやり取りできる、AI を活用したエージェントを構築およびデプロイできます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Agent Orchestratorの公判の申立て | Agent Orchestratorでは体験版を提供するようになりました。これにより、お客様は完全購入をコミットする前にサービスを参照およびテストできます。 この「購入前に試す」オプションを使用すると、スキルやオーケストレーション機能など、Agent Orchestratorの機能を組織内で評価できます。 この体験版では、AI を利用したエージェントの構築と、それらが既存のワークフローにどのように統合できるかを理解する実践的なエクスペリエンスを提供します。 |

{style="table-layout:auto"}

詳しくは、[Agent Orchestrator ドキュメント ](https://experienceleague.adobe.com/ja/docs/experience-cloud-ai/experience-cloud-ai/agents/agent-orchestrator) を参照してください。

## 宛先 {#destinations}

Experience Platformから [!DNL Destinations] データの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| ケベル宛先コネクタが使用可能になりました | [[!DNL Kevel]](https://www.kevel.com/) は、革新的なコマースリーダーがリテールメディアでローンチ、拡大、成功するのを支援する、AI 対応のテクノロジーとエキスパートガイダンスを提供します。 [!DNL Kevel] の Retail Media Cloud は、オンサイト広告とオフサイト広告のために、ターゲットを絞った、帰属可能なカスタマイズ可能な広告フォーマットを強化します。 |
| インデックス交換の宛先コネクタが使用可能になりました | [!DNL Index] は、メディア所有者が全画面にわたってコンテンツの価値を最大化するのに役立つ、グローバル広告のサプライサイドのプラットフォームです。 20 年以上にわたる業界のリーダーシップを持つ [!DNL Index] は、世界最大のブランドとプレミアムなエクスペリエンスメーカーを結び付け、高品質の消費者体験を提供します。 |
| Braze 接続の地域エンドポイントのサポート | [ でサポートされているすべての ](https://www.braze.com/docs/user_guide/administrative/access_braze/sdk_endpoints) 地域固有のエンドポイント [!DNL Braze] が、宛先設定フロー中に選択できるようになりました。 使用するエンドポイントインスタンスを [!DNL Braze] 担当者に問い合わせます。 |
| Liveramp オンボーディングの毎週および毎月のスケジュールのサポート | Liveramp オンボーディング宛先の毎週および毎月の書き出しスケジュールを設定できるようになりました。 |
| Amazon S3 の宛先に対する AES256 暗号化のサポート | Amazon S3 の書き出しに AES256 暗号化を設定できるようになりました。 |
| Trade Desk とMicrosoft Bing の宛先のアクティベーションエクスペリエンスの強化 | Trade Desk とMicrosoft Bing の宛先に、最適化されたアクティベーションエクスペリエンスのための事前定義済みの必須マッピングが含まれるようになりました。 |

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Targetの宛先のガードレール制限を更新しました | 1 つのAdobe Targetの宛先にマッピングできるオーディエンスの最大数が 50 から 250 に増えました。 これにより、Adobe Targetが他の宛先の標準のオーディエンス制限に合わせられ、オーディエンスアクティベーションワークフローの柔軟性が向上します。 複数のデータフローを作成しなくても、Adobe Targetの宛先に対して、より多くのオーディエンスをアクティブ化できるようになりました。 |
| [ 宛先の編集 ](/help/destinations/ui/edit-destination.md) および [ マーケティングアクションの編集 ](/help/destinations/ui/edit-activation.md#edit-marketing-actions) 一般提供 | 宛先とマーケティングアクションを編集するオプションが、すべてのユーザーが使用できるようになりました。 |
| マッピングステップでのフィールド表示名の切り替え | スキーマフィールドを宛先にマッピングする際に、完全な XDM フィールド名の表示と表示名のみの表示を切り替えられるようになりました。 |

{style="table-layout:auto"}

詳しくは、[ 宛先の概要 ](../destinations/home.md) を参照してください。

## リアルタイム顧客プロファイル {#real-time-customer-profile}

リアルタイム顧客プロファイルを使用すると、オンライン、オフライン、CRM、サードパーティデータなど、複数のチャネルのデータを組み合わせて、各顧客の全体像を確認できます。 プロファイルを使用すると、顧客データを統合ビューに統合して、顧客インタラクションごとにアクションにつながるタイムスタンプ付きアカウントを提供できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| ストリーミング容量の適用 | Experience Platformでは、リアルタイム顧客プロファイルと ID サービスのストリーミングスループット機能を適用するようになりました。 顧客が契約したストリーミング容量を超えると、データは先入れ先出しの方法でキューに入れられ、処理されます。 これにより、予測可能なシステムパフォーマンスが確保され、容量違反によってデータ取り込みの品質に影響が及ぶのを防ぐことができます。 重要なメモ：処理能力を超えると、データレイクでアップサートのストリーミングを使用できなくなります。この適用は、Adobe Journey Optimizer ライセンスを持つお客様には適用されません。処理能力が利用可能になると、キュー内のデータが順次処理されます。 |
| Real-Time CDP Primeの API アクセスの廃止 | エクスペリエンスイベントの API アクセスは、すべてのReal-Time CDP Prime ユーザーに対して非推奨（廃止予定）になりました。 この変更は、API を介してエクスペリエンスイベントを直接クエリする機能に影響を与えます。 Real-Time CDP Ultimateのお客様は、正式な例外プロセスを通じて例外をリクエストし、必要に応じてエクスペリエンスイベント API アクセスを有効にすることができます。 この非推奨（廃止予定）は、Real-Time CDPとライセンス機能の連携に役立ちます。 |
| データフロー実行の監視 | プロファイルでデータフロー実行の進行状況と準備状況を監視できるようになりました。 |

{style="table-layout:auto"}

詳しくは、[[!DNL Real-Time Customer Profile] 概要](../profile/home.md)を参照してください。

## スキーマ {#schemas}

Experience Platform では、スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。あらゆるシステムにわたって一貫した形式でデータを定義することで、意味を保持しやすくなり、したがってデータから価値を得やすくなります。スキーマは、基本クラスと 0 個以上のスキーマフィールドグループで構成されます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 検索、フィルター、タグ、フォルダーによるスキーマインベントリの最新化 | スキーマの参照ページが最新化され、組織および検出機能が強化されました。 新しい機能には、高度な検索およびフィルタリングオプション、スキーマを整理するためのユーザー生成タグおよびフォルダーのサポート、ワークフローを効率化するためのインラインアクションが含まれます。 主な改善点：更新済み列（名前、クラス、データセット、ID、関係、プロファイルを有効、動作、スキーマタイプ、タグ、作成日、最終変更日）、詳細フィルター（プロファイルを表示、スキーマタイプ、クラス、タグあり、作成日、変更日、プライマリ ID あり、関係、プライマリ ID 名前空間あり）、インラインアクション（編集、削除、ラベルを適用、ラベルを適用、非リレーショナルスキーマのデータセットを作成、タグを管理、フォルダーに移動、パッケージに追加、JSON 構造をコピー、サンプルファイルをダウンロード）、タグとフォルダーを使用してスキーマを整理する機能。 これらの機能強化により、スキーマリソースを包括的に表示できるようになり、サンドボックスレベルでのスキーマ管理がより効率的になります。 |

詳しくは、[[!DNL Schemas] 概要](../xdm/home.md)を参照してください。

## セグメント化サービス {#segmentation-service}

[!DNL Segmentation Service] は、顧客ベース内のマーケティング可能なユーザーグループを区別する基準を記述することで、プロファイルの特定のサブセットを定義します。オーディエンスは、レコードデータ（人口統計情報など）や、顧客によるブランドとのやり取りを表す時系列イベントに基づいて設定できます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| 外部オーディエンス TTL の更新 | 外部オーディエンス（CSV アップロードなど）で、Time-to-Live （TTL）設定の強制更新機能がサポートされるようになりました。 この機能を使用すると、外部オーディエンスの TTL 有効期限を手動で更新でき、オーディエンスのライフサイクル管理をより詳細に制御できます。 これは、初期 TTL 期間を超えて保持する必要があるオーディエンスや、データを再アップロードせずに再アクティブ化する必要があるオーディエンスで特に便利です。 |

詳しくは、[[!DNL Segmentation Service] 概要](../segmentation/home.md)を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新規または更新されたソース**

| ソース | 説明 |
| --- | --- |
| [!DNL Oracle Eloqua] V2 ソース | 非推奨のコネクタに代わって、新しい [!DNL Oracle Eloqua] ソースコネクタが使用できるようになりました。 この更新されたコネクタにより、[!DNL Oracle Eloqua] からExperience Platformにデータを取り込む機能が強化され、信頼性が向上しています。 既存の接続は機能しなくなるので、既存のコネクタを使用しているお客様は、新しい実装に移行する必要があります。 新しいコネクタは、に接続してマーケティング自動化データを取り込むために必要なすべてのセットアップ [!DNL Oracle Eloqua] 設定手順をサポートしています。 |
| [!DNL Salesforce Marketing Cloud] V2 ソース | 非推奨のコネクタに代わって、新しい [!DNL Salesforce Marketing Cloud] ソースコネクタが使用できるようになりました。 この更新されたコネクタにより、パフォーマンスが向上し、[!DNL Salesforce Marketing Cloud] からExperience Platformにデータを取り込む機能が増えました。 既存のコネクタを使用しているお客様は、新しい実装に移行する必要があります。 新しいコネクタには、[!DNL Salesforce Marketing Cloud] に接続してマーケティング自動化データを取り込むための包括的な設定手順が含まれています。 |

詳しくは、[ソースの概要](../sources/home.md)を参照してください。

