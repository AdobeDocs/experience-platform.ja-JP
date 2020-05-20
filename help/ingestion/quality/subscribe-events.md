---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データ取り込みイベントのサブスクライブ
topic: overview
translation-type: tm+mt
source-git-commit: 1498739d753bdb569e0d3e091e4160bdae40a32f
workflow-type: tm+mt
source-wordcount: '851'
ht-degree: 2%

---


# データ取り込み通知

データをAdobe Experience Platformに取り込むプロセスは、複数の手順で構成されます。 プラットフォームに取り込む必要のあるデータファイルを特定したら、取り込みプロセスが開始され、データが正常に取り込まれるか、または失敗するまで各手順が連続して行われます。 インジェスト処理は、 [Adobe Experience Platform Data Ingest API](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/ingest-api.yaml) 、またはExperience Platformユーザーインターフェイスを使用して開始できます。

プラットフォームに読み込まれたデータは、目的のデータレーク(Data Lake)またはリアルタイム顧客プロファイルデータストアに到達するために、複数の手順を経る必要があります。 各手順では、データの処理、データの検証、データの格納、データの次の手順への渡しを行います。 取り込まれるデータ量に応じて、この処理は時間がかかる場合があり、検証、セマンティック、または処理エラーが原因でプロセスが失敗する可能性が常にあります。 エラーがイベントした場合は、データの問題を修正し、修正したデータファイルを使用してインジェストプロセス全体を再起動する必要があります。

インジェストプロセスの監視を支援するため、Experience Platformでは、プロセスの各手順で公開された一連のイベントに登録し、取り込まれたデータのステータスと発生する可能性のあるエラーを通知できます。

## 使用可能な状態通知イベント

以下に、サブスクライブ可能なデータ取り込みステータス通知のリストを示します。

>[!NOTE] すべてのデータ取り込み通知に対して1つのイベントトピックのみが提供されます。 異なるステータスを区別するために、イベントコードを使用できます。

| プラットフォームサービス | Status | イベントの説明 | イベントコード |
| ---------------- | ------ | ----------------- | ---------- |
| データのランディング | success | 取り込み — バッチが成功しました | ing_load_success |
| データのランディング | 失敗 | 取り込み — バッチが失敗しました | ing_load_failure |
| リアルタイム顧客プロファイル | success | プロファイルサービス — データ読み込みバッチが成功しました | ps_load_success |
| リアルタイム顧客プロファイル | 失敗 | プロファイルサービス — データ読み込みバッチが失敗しました | ps_load_failure |
| 識別グラフ | success | IDグラフ — データ読み込みバッチが成功しました | ig_load_success |
| 識別グラフ | 失敗 | IDグラフ — データの読み込みバッチに失敗しました | ig_load_failure |

## 通知ペイロードスキーマ

データ取り込み通知イベントスキーマは、取り込まれるデータのステータスに関する詳細を提供するフィールドと値を含むExperience Data Model(XDM)スキーマです。 最新の [通知ペイロードスキーマを表示するには、パブリックXDM GitHubリポジトリにアクセスしてください](https://github.com/adobe/xdm/blob/master/schemas/common/notifications/ingestion.schema.json)。

## データ取り込みステータス通知のサブスクライブ

Adobe I/O [イベントを通じて](https://www.adobe.io/apis/experienceplatform/events.html)、Webフックを使用して複数の通知タイプをサブスクライブできます。 以下の節では、Adobe Developer Consoleを使用してデータ取り込みイベントのプラットフォーム通知をサブスクライブする手順を説明します。

### Adobe Developer Consoleでの新しいプロジェクトの作成

Adobe Developer Console [に移動し](https://www.adobe.com/go/devs_console_ui) 、Adobe IDでサインインします。 次に、Adobe Developer Consoleドキュメントで空のプロジェクトの [作成に関するチュートリアルに概要を説明している手順に従い](https://www.adobe.io/apis/experienceplatform/console/docs.html#!AdobeDocs/adobeio-console/master/projects-empty.md) ます。

### プロジェクトへの追加エクスペリエンスプラットフォームのイベント

新しいプロジェクトを作成したら、そのプロジェクトの概要画面に移動します。 ここから、 **[!UICONTROL イベントをクリックします]**。

![](../images/quality/subscribe-events/add-event-button.png)

[ _[!UICONTROL イベント]_]ダイアログが表示されます。 「**[!UICONTROL &#x200B;エクスペリエンスプラットフォーム&#x200B;]**」をクリックして利用可能なオプションのリストをフィルターし、「**[!UICONTROL &#x200B;プラットフォーム通知&#x200B;]**」をクリックしてから「**[!UICONTROL &#x200B;次へ&#x200B;]**」をクリックします。

![](../images/quality/subscribe-events/select-platform-events.png)

次の画面には、登録するイベントタイプのリストが表示されます。 「 **[!UICONTROL データ取り込み通知]**」を選択し、「 **[!UICONTROL 次へ]**」をクリックします。

![](../images/quality/subscribe-events/choose-event-subscriptions.png)

次の画面では、JSON Web Token(JWT)の作成を求めるプロンプトが表示されます。 自動的にキーペアを生成するか、端末で生成した独自の公開鍵をアップロードするかを選択できます。

このチュートリアルでは、最初のオプションに従います。 「キーペアを **[!UICONTROL 生成」のオプションボックスをクリックし]**、右下隅の「キーペアを **[!UICONTROL 生成]** 」ボタンをクリックします。

![](../images/quality/subscribe-events/generate-keypair.png)

キーペアが生成されると、ブラウザーによって自動的にダウンロードされます。 このファイルは開発者コンソールで保持されないので、自分で保存する必要があります。

次の画面では、新しく生成されたキーペアの詳細を確認できます。 「**[!UICONTROL 次へ]**」をクリックして次に進みます。

![](../images/quality/subscribe-events/keypair-generated.png)

次の画面で、イベント登録の名前と説明を入力します。 ベストプラクティスは、同じプロジェクト内の他のユーザーとこのイベントの登録を区別できるように、一意で、簡単に識別できる名前を作成することです。

![](../images/quality/subscribe-events/registration-details.png)

同じ画面の下で、イベントの受信方法をオプションで設定できます。 **[!UICONTROL Webhook]** では、イベントを受け取るカスタムWebフックアドレスを指定できますが、 **[!UICONTROL Runtime action]** では、 [Adobe I/O Runtime](https://www.adobe.io/apis/experienceplatform/runtime/docs.html).

このチュートリアルでは、このオプションの設定手順は省略します。 完了したら、「設定済みのイベントを **[!UICONTROL 保存]** 」をクリックしてイベントの登録を完了します。

![](../images/quality/subscribe-events/receive-events.png)

新しく作成されたイベント登録の詳細ページが表示され、受信したイベントを確認し、デバッグトレースを実行して設定を編集できます。

![](../images/quality/subscribe-events/registration-complete.png)

## 次の手順

プラットフォーム通知をプロジェクトに登録すると、プロジェクトダッシュボードから受信したイベントを表示できます。 イベントをトレースする方法の詳細な手順については、『 [Adobe I/Oイベントのトレース](https://www.adobe.io/apis/experienceplatform/events/docs.html#!adobedocs/adobeio-events/master/support/tracing.md) 』ガイドを参照してください。
