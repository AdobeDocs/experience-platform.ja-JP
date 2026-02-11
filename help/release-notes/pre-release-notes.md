---
title: Experience Platformのプレリリースノート
description: Adobe Experience Platformの最新のリリースノートのプレビュー。
exl-id: f2c41dc8-9255-4570-b459-4f9fc28ee58b
source-git-commit: b8c257ad9ab4e7ee085687f6c03cf55d7fb83ef0
workflow-type: tm+mt
source-wordcount: '1022'
ht-degree: 24%

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
>- [Customer Journey Analytics](https://experienceleague.adobe.com/ja/docs/analytics-platform/using/releases/latest)
>- [連合オーディエンス構成](https://experienceleague.adobe.com/ja/docs/federated-audience-composition/using/release-notes)
>- [Real-Time CDP Collaboration](https://experienceleague.adobe.com/ja/docs/real-time-cdp-collaboration/using/latest)

**リリース日：2026 年 2 月**

Adobe Experience Platformの既存の機能に対する新機能とアップデート：

- [Agent Orchestrator](#agent-orchestrator)
- [アラート](#alerts)
- [データ収集](#data-collection)
- [宛先](#destinations)
- [エクスペリエンスデータモデル（XDM）](#xdm)
- [クエリサービス](#query-service)
- [ソース](#sources)

## Agent Orchestrator {#agent-orchestrator}

Agent Orchestratorを使用すると、ワークフローを自動化し、複数のチャネルをまたいで顧客とやり取りできる、AI を活用したエージェントを構築およびデプロイできます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| データオンボーディングエージェント | データオンボーディングエージェントを使用して、ソース接続の設定、データ品質の検証、セマンティックエンリッチメントの適用、スキーマのレビューと検証、データ取り込みの実行を行います。 B2C フローと B2B フローの両方のワークフローを手順ごとに実行し、期待される出力を確認し、一般的な問題のトラブルシューティングを行います。 |
| Data Distiller Agent | Data Distiller Agent を使用して、自然言語からの SQL ジョブの作成、SQL パフォーマンスの最適化、SQL エラーからのリカバリ、SQL ジョブのスケジュール設定と管理、ジョブ・ステータスの監視を行います。 開始するには、ガードレール、必要な権限およびトラブルシューティングガイダンスを確認します。 |
| データ収集エージェント | データ収集エージェントを使用すると、複雑なデータ収集設定に関するコンテキスト内のガイダンスを取得でき、対話型のインサイトを通じてデータ収集オブジェクト全体の系列、依存関係、および関係を調べることができます。 |

{style="table-layout:auto"}

詳しくは、[Agent Orchestrator ドキュメント &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-cloud-ai/experience-cloud-ai/agents/agent-orchestrator) を参照してください。

## アラート {#alerts}

Experience Platformでは、様々なExperience Platform アクティビティに関するイベントベースのアラートを登録できます。 Experience Platform ユーザーインターフェイスの「[!UICONTROL Alerts]」タブを使用して、様々なアラートルールを購読し、UI 内またはメール通知を通じてアラートメッセージを受け取るように選択できます。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| 顧客向けアラートの [!DNL Slack] 統合 | 顧客向けのアラートを [!DNL Slack] に配信できるようになりました。 ステップバイステップのチュートリアルに従って [!DNL Slack] 統合を設定し、[!DNL Slack] ワークスペースで直接アラート通知を受け取ります。 |

{style="table-layout:auto"}

詳しくは、[[!DNL Observability Insights] 概要](../observability/home.md)を参照してください。

## データ収集 {#data-collection}

Adobe Experience Platform Data Collection は、クライアントサイドのカスタマーエクスペリエンスデータを収集し、Adobe Experience Platform Edge Networkやその他の宛先に送信できる一連のテクノロジーを提供します。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Adobe Platform Tags Extension Management | 新しい拡張機能管理機能を使用して、組織の拡張機能をアップロード、パッケージ化し、開発、プライベートおよびパブリック配布にリリースします。 トップレベルの会社表示で、所有している拡張機能と一緒に、共有プライベート拡張機能を見つけます。 この機能は、web、edge およびモバイル拡張機能をサポートします。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; データ収集ドキュメント &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-platform/collection/home) を参照してください。

## 宛先 {#destinations}

Experience Platformから [!DNL Destinations] データの円滑なアクティベーションを可能にする、事前定義済みの出力先プラットフォームとの統合です。 宛先を使用して、クロスチャネルマーケティングキャンペーン、メールキャンペーン、ターゲット広告、その他多くの使用事例に関する既知および不明なデータをアクティブ化できます。

**新規宛先または更新された宛先**

| 宛先 | 説明 |
| --- | --- |
| 一般入手可能な [!DNL Snowflake] バッチ | [!DNL Snowflake] バッチ宛先が一般提供（GA）に移動しました。 書き出されたデータの結合ポリシー ID 列を、タイムスタンプ、マッピング属性、オーディエンスメンバーシップなどの既存の列と共に表示できるようになりました。 |
| [Amazon S3](../destinations/catalog/cloud-storage/amazon-s3.md#destination-details) 宛先に対する AES256 暗号化のサポート | Amazon S3 の書き出しに AES256 暗号化を設定できるようになりました。 次の 2 つのオプションから選択します。 <ul><li>**[!UICONTROL Default]**:Experience Platformは、バケットに設定されたデフォルトの暗号化アルゴリズムを使用して、保存中のデータを暗号化します。</li><li>**[!UICONTROL SSE-S3/AES256]**:Experience Platformは、書き出しに `s3:x-amz-server-side-encryption": "AES256` ヘッダーを追加し、S3 に到達すると、保存中のデータを AES256 アルゴリズムで暗号化します。 **このオプションは、S3 バケットで設定したデフォルトの暗号化アルゴリズムよりも優先されます**。</li></ul> |

{style="table-layout:auto"}

詳しくは、[&#x200B; 宛先の概要 &#x200B;](../destinations/home.md) を参照してください。

## エクスペリエンスデータモデル（XDM） {#xdm}

XDM は、Experience Platformに取り込むデータの共通の構造と定義（スキーマ）を提供するオープンソース仕様です。 XDM 標準規格に準拠しているので、すべての顧客体験データを共通の表現に反映させて、迅速かつ統合的な方法でインサイトを提供できます。顧客アクションから有益なインサイトを得たり、セグメントを通じて顧客オーディエンスを定義したり、パーソナライズ機能のために顧客属性を使用したりできます。

**新機能または更新された機能**

| 機能 | 説明 |
| ------- | ----------- |
| スキーマインベントリの組織と検索 | スキーマの参照ページに、検索とフィルタリングの強化、インラインアクション、ユーザー定義のタグとフォルダーのサポートが含まれるようになりました。 これらの更新により、手動のナビゲーションやメンテナンス作業を軽減しながら、サンドボックス間でスキーマを検索、整理、管理しやすくなります。 |

詳しくは、[[!DNL XDM] 概要](../xdm/home.md)を参照してください。

## クエリサービス {#query-service}

クエリサービスを使用すると、標準 SQL を使用して Adobe Experience Platform [!DNL Data Lake] でデータに対してクエリを実行できます。[!DNL Data Lake] の任意のデータセットを結合したり、クエリ結果を新しいデータセットとして取得したりすることで、それらのデータセットをレポートやデータサイエンスワークスペースで使用したり、リアルタイム顧客プロファイルへの取り込みが可能になります。

**新機能または更新された機能**

| 機能 | 説明 |
| --- | --- |
| Data Distillerの年間計算リセット日の整合性（限定リリース） | Data Distillerの年間計算時間は、ライセンスの購入日または更新日に基づいて、Data Distiller契約の記念日にリセットされるようになりました。 これにより、ライセンス使用状況レポートが契約条件に合わせて調整され、現在の使用状況値に対して 1 回限りの調整が行われる場合があります。 |
| Data Distiller session management （限定リリース） | 承認済み管理者は、UI を使用して、組織内およびサンドボックス内のアクティブなクエリサービスおよびデータDistiller セッションを表示および管理できます。 セッション管理を使用して、アイドル状態のセッションを特定し、それらのセッションを終了して容量を解放します。 組み込みの保護機能により、アクティブなクエリを含むセッションを終了することはできません。 この機能は、監査を行うためにすべての立ち退きアクションをログに記録し、影響を受けるユーザーに通知します。 この機能にアクセスするには、**クエリセッションの管理** 権限が必要です。 |

{style="table-layout:auto"}

詳しくは、[&#x200B; クエリサービスの概要 &#x200B;](../query-service/home.md) を参照してください。

## ソース {#sources}

Experience Platform は、様々なデータプロバイダーのソース接続を簡単に設定できる RESTful API とインタラクティブ UI を備えています。これらのソース接続を使用すると、外部ストレージシステムおよび CRM サービスの認証と接続、取得実行時間の設定、データ取得スループットの管理を行うことができます。

**新規または更新されたソース**

| ソース | 説明 |
| --- | --- |
| ソースコネクタでの Unity カタログ [!DNL Databricks] サポート | [!DNL Databricks] ソースコネクタで Unity カタログがサポートされるようになりました。 ソース接続を設定する際に Unity カタログを使用する方法については、更新された [[!DNL Databricks]](../sources/connectors/databases/databricks.md) ドキュメントをお読みください。 |

{style="table-layout:auto"}

詳しくは、[ソースの概要](../sources/home.md)を参照してください。
