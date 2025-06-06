---
title: Privacy Serviceのベストプラクティス
description: これらの最適な使用ガイドラインに従って、プライバシーリクエストを完了する際に組織に発生する処理時間とコストを削減する方法を説明します。
exl-id: 1333d6c6-5ca0-41c1-9f9e-aa2a5a8b8a9c
source-git-commit: 8be502c9eea67119dc537a5d63a6c71e0bff1697
workflow-type: tm+mt
source-wordcount: '1234'
ht-degree: 0%

---

# Privacy Serviceのベストプラクティス

Privacy Serviceを使用すると、お客様がデータストアから個人データへのアクセスや削除を希望する場合に、データプライバシー規則への準拠を自動化できます。 このような変化するビジネスニーズに対応するために、Privacy Serviceでは、Adobe Experience Cloud アプリケーションをまたいでカスタマーデータのアクセスおよび削除要求を送信するための RESTful API および UI を提供しています。

このガイドでは、プライバシーリクエストを効率的に処理し、顧客データリクエストを管理する際の完了応答時間を最適化するためのベストプラクティスについて説明します。

## はじめに {#getting-started}

このガイドでは、[Privacy Service](./home.md) およびAdobe Experience Cloud アプリケーション全体のデータ主体（顧客）からのアクセスリクエストと削除リクエストを管理する方法について実際に理解している必要があります。 また、[UI でのプライバシージョブリクエストの作成 ](./ui/user-guide.md#create-a-new-privacy-job-request) または [API](./api/overview.md) に関するガイドを読み、これらの操作をプログラムで実行する方法を理解しておくことをお勧めします。

## 前提条件 {#prerequisites}

Adobe Experience Platform Privacy Serviceへのアクセスは、Adobe Admin Consoleの詳細な役割ベースの権限によって制御されます。 Privacy ServiceUI と API で特定の機能を使用するには、製品プロファイルで関連する権限が必要です。 追加の権限が必要な場合は、システム管理者にお問い合わせください。

管理者に関する詳細は、[Privacy Serviceの権限の管理 ](./permissions.md) に関するガイドを参照してください。

## プライバシージョブ作成のガイドライン {#creation-guidelines}

リクエスト処理を効率化し応答時間を改善するには、プライバシージョブを作成する際に次のガイドラインを考慮します。 これは、API メソッドと UI メソッドの両方に適用されます。

1. **リクエストごとのデータ主体の最大化：** リクエストごとに可能な限り多くのデータ主体（最大 1,000 個）を含めます。
2. **効率のためのグループ ID:** 各リクエストで 1 つのデータ主体（最大 9 個）に対して複数の ID をグループ化します。 **ID は、同じリクエストの異なるAdobe サービスから取得でき** す。
3. **アクセスジョブと削除ジョブの組み合わせ：** データ主体によって必要とされる場合は、「アクセス」ジョブタイプと「削除」ジョブタイプの両方を 1 つのリクエストに含めます。
4. **必要な製品のみを含める：** 必要またはライセンスのある製品のみを含めます。 製品を追加すると、処理時間が長くなり、コストが増加する可能性があります。

## プライバシージョブステータスの監視 {#monitor-status}

プライバシージョブを効果的に監視し、そのステータスを確認するために、Privacy Serviceでは 3 つの方法を提供しています。 利用可能な方法は、効率と生産性を監視する順序で以下に示します。 各方法には、エクスペリエンスを向上させるためのベストプラクティスガイドラインに続いて、すべてのアプローチを組み合わせた理想的なシナリオの例が含まれています。

### リアルタイム通知を受信 {#real-time-notifications}

**I/O イベント** は、ステータスイベントを通じて、ほぼリアルタイムのステータス監視を提供します。 これは、ポーリングメカニズムを実装する必要がなく、追加の API トラフィックが発生するので、最も効率的な方法です。

**Recommendations:**

- **Webhook 設定：** 送信されたジョブのステータスが変更されたときにプッシュ通知を受信する Webhook を設定します。 これにより、リアルタイムの監視が容易になります。
- **通知：** ジョブと製品の両方のレベルで通知を使用して、リクエストの進行状況を監視するのに役立ちます。

Privacy Service通知のイベント登録のセットアップ手順と通知ペイロードの解釈方法については、[Privacy Serviceイベントの登録 ](./privacy-events.md) に関するドキュメントを参照してください。

### フィルターに基づくすべてのジョブの取得 {#retrieve-filtered-responses-for-all-jobs}

指定したフィルターに基づいてすべてのプライバシージョブデータを取得するには、**`/jobs` エンドポイントに対してGETリクエストを実行します**。 この API 呼び出しは、1 回のリクエストで大量のジョブ ID の現在のジョブステータスの概要を表示する場合に役立ちます。 詳細な製品応答はありませんが、[`/jobs/{jobID}` エンドポイント ](#retrieve-detailed-responses-for-specific-jobs) を使用して見つけることができます。

`/jobs` エンドポイントへのGETリクエストは、多数のジョブ ID のステータスデータを収集または比較するために最適ですが、通常のポーリングタイプのアクティビティ向けには **ありません** 使用できます。

**Recommendations:**

- **クエリパラメーター：** 特定のフィルターを使用して、結果を絞り込みます。例：データ範囲、規制タイプおよびステータス（処理中、完了など）。

組織内の現在のすべてのプライバシージョブのリストは、Privacy ServiceUI から表示できます。 ジョブリクエストリストをフィルタリングする方法については、[UI ドキュメントのプライバシージョブの管理 ](./ui/user-guide.md#job-requests) を参照してください。 または、[Privacy ServiceAPI での/job エンドポイントの使用 ](./api/privacy-jobs.md) に関するドキュメントを参照してください。

[ 使用可能なクエリパラメーターフィルター ](https://developer.adobe.com/experience-platform-apis/references/privacy-service/#tag/Privacy-jobs/operation/listPrivacyJobs) の詳細については、Privacy ServiceAPI ドキュメントを参照してください。

### 1 つのジョブに対する詳細な応答の取得 {#retrieve-detailed-responses-for-specific-jobs}

1 つのジョブに対する詳細な応答を取得するには、**/jobs/{jobID} エンドポイントに対してGETリクエストを実行します**。 この方法は、製品固有の応答や成功メッセージなど、より深い情報収集を目的としています。 このエンドポイントの呼び出しは、どの製品が応答し、どの製品がまだ保留中かを確認する最適な方法ですが、通常のポーリングアクティビティ向けでは **ありません** です。

[ 特定のジョブのステータスを確認する方法 ](./api/privacy-jobs.md#check-status) について詳しくは、`/jobs/{JOB_ID}` エンドポイントのドキュメントを参照してください。

### 理想的なシナリオの例 {#ideal-scenario}

Webhook を使用すると、システムはレコードを自動的に更新し、リクエストからの ID のグループが完了したときにレポートやアラートを提供できます。 ジョブがまだ未処理の場合、Privacy ServiceAPI `/jobs` エンドポイントへのGETリクエストを使用してこれらのジョブステータスが取得され、リストの大まかな更新が提供されます。

特定のジョブがまだ保留中であるか、エラーが返された場合は、`/job/{jobId}` エンドポイントへのGETリクエストを使用して詳細な応答を取得できます。

## リクエストデータへのアクセス {#access-request-data}

データ主体の情報が要求されると、各サービスは、データの保存方法や使用方法に合った形式でデータを返します。 すべてのサービスがリクエストを完了すると、このデータをダウンロードできるように、ジョブの詳細に.ZIP アーカイブファイルの URL が指定されます。 [ プライバシージョブの結果をダウンロードする方法 ](https://experienceleague.adobe.com/docs/experience-platform/privacy/troubleshooting-guide.html?lang=ja#how-do-i-download-the-results-of-my-completed-privacy-jobs%3F) について詳しくは、トラブルシューティングガイドを参照してください。

データ・アーカイブの管理に関する重要事項を次に示します。

- すべてのアーカイブ・ファイルは、30 日後にExperience Platform・サーバから削除されます。 30 日を超える顧客データはクエリできません。
- アーカイブファイルの構造は、リクエストに含まれる製品毎のフォルダと、その中に含まれるデータファイルとを含む。 指定した ID のデータが見つからなかった場合、アーカイブファイルまたはフォルダーは空になることがあります。
- 以前作成したジョブのデータには、完了日から 30 日間のみアクセスできます。 その後、データがシステムから削除され、新しいリクエストを行う必要があります。

**Recommendations:**

- **Protect データアーカイブ：** URL ファイルと.ZIP ファイルには、データ主体の個人を特定できる情報（PII）が含まれている可能性があるので、どちらも保護する必要があります。

## 技術上の考慮事項 {#technical-considerations}

Privacy Serviceリクエストを行う際には、次の技術的な考慮事項に注意する必要があります。

- **データ保持期間：** ジョブのグループの最大ルックバック期間は 60 日で、クエリの最大期間は 30 日（開始日/終了日）です。
- **ゲートウェイタイムアウト：** 要求が 60 秒を超えると、ゲートウェイからドロップされる可能性があることに注意してください。
- **エラー処理：** エラーメッセージを十分に確認し、必要に応じてリクエストを再送信します。 Privacy Serviceは、エラーの後にジョブを自動的に再処理しません。
- **HTTP 429 エラーについて：** HTTP 429 エラーメッセージと、問題を軽減するために必要な手順について確認します。 HTTP 429 エラーは、「リクエストが多すぎます」が原因です。 問題の解決方法について詳しくは、トラブルシューティングガイドの [ 一般的なエラーメッセージ ](./troubleshooting-guide.md#common-error-messages) の節を参照してください。

## 次の手順

このドキュメントを読むことで、Privacy Serviceを効率的かつ効果的に使用するために必要な知識とプラクティスを得ることができました。 次に、Privacy Serviceに関するよくある質問への回答と API でよく発生するエラーに関する情報については、[ トラブルシューティングガイド ](./troubleshooting-guide.md) を参照してください。
