---
keywords: Experience Platform；ホーム；人気の高いトピック；アラート
description: データフローの作成時にアラートをサブスクライブして、フロー実行のステータス、成功または失敗に関するアラートメッセージを受け取ることができます。
title: UI でのコンテキスト内アラートへのサブスクライブ
exl-id: 5d51edaa-ecba-4ac0-8d3c-49010466b9a5
source-git-commit: 3f7f66c0d58d127299ad12027869ca0e9837f5cd
workflow-type: tm+mt
source-wordcount: '868'
ht-degree: 21%

---

# UI でのソースデータフローに関するアラートのサブスクライブ

>[!NOTE]
>
>非実稼働用サンドボックスでは、アラートはサポートされていません。 アラートを購読するには、実稼動用サンドボックスを使用していることを確認する必要があります。

Adobe Experience Platform では、Adobe Experience Platform アクティビティに関するイベントベースのアラートを登録できます。 アラートにより、ジョブが完了したか、ワークフロー内の特定のマイルストーンに達したか、何らかのエラーが発生したかを確認するために、[[!DNL Observability Insights] API](../../../observability/api/overview.md) をポーリングする必要が低減される、またはなくなります。

データフローを作成する際にアラートの配信を登録して、フロー実行のステータス、成功または失敗に関するアラートメッセージを受信できます。

このドキュメントでは、ソースデータフローのアラートメッセージを受信する方法をサブスクライブする手順を説明します。

## はじめに

このドキュメントでは、Adobe Experience Platformの次のコンポーネントに関する十分な知識が必要です。

* [ソース](../../home.md)：[!DNL Experience Platform] を使用すると、データを様々なソースから取得しながら、[!DNL Platform] サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [Observability では、統計的な指標とイベント通知を使用して Platform アクティビティを監視できます。](../../../observability/home.md)[!DNL Observability Insights]
   * [アラート](../../../observability/alerts/overview.md):Platform 操作で特定の条件セットに達すると（システムがしきい値に達した場合に問題が発生する可能性があるなど）、Platform は、組織内でその条件を購読したユーザーにアラートメッセージを配信できます。

## UI でアラートを購読 {#subscribe-sources-alerts}

>[!CONTEXTUALHELP]
>id="platform_sources_alerts_subscribe"
>title="ソースアラートの購読"
>abstract="アラートを使用すると、ソースデータフローのステータスに基づいて、通知を受け取ることができます。アラート通知を設定すると、データフローが開始された場合、成功した場合、失敗した場合、データが取り込まれなかった場合に、更新情報を受け取ることができます。"
>text="Learn more in documentation"

>[!IMPORTANT]
>
>データフローの電子メールベースのアラート通知を受け取るには、Platform アカウント用の電子メールのインスタント通知を有効にする必要があります。

データフローに関するアラートは、 [!UICONTROL データフローの詳細] 「ソース」ワークスペース内のソースワークフローのステップ。

![dataflow-detail](../../images/tutorials/alerts/dataflow-detail.png)

ソースデータフローで使用可能なアラートは次のとおりです。

| アラート | 説明 |
| --- | --- |
| ソースのデータフロー実行開始 | このアラートは、ソースのデータフローが開始すると、メッセージを送信します。 |
| ソースのデータフロー実行成功 | このアラートは、ソースのデータが Platform に正常に取り込まれると、メッセージを送信します。 |
| ソースのデータフロー実行エラー | このアラートは、データフローでエラーが発生した場合にメッセージを送信します。 |
| ~~データフローの取り込み不足のソース~~ | ~~このアラートは、取り込みが 7 時間以上遅れ、データが Platform に取り込まれない場合に、メッセージを送信します。~~ <br>**注意：** このアラートは非推奨となったので、アラートを受け取らなくなります。 |

購読するアラートを選択し、「 」を選択します。 **[!UICONTROL 次へ]** をクリックし、データフローを確認して終了します。

![select-alerts](../../images/tutorials/alerts/select-alerts.png)

UI でソースデータフローを作成する詳細な手順については、次のガイドを参照してください。

* [広告](./dataflow/advertising.md)
* [クラウドストレージ](./dataflow/batch/cloud-storage.md)
* [CRM](./dataflow/crm.md)
* [データベース](./dataflow/databases.md)
* [E コマース](./dataflow/ecommerce.md)
* [ローカルファイル](./create/local-system/local-file-upload.md)
* [マーケティングの自動処理](./dataflow/marketing-automation.md)
* [支払い](./dataflow/payments.md)
* [プロトコル](./dataflow/protocols.md)

## アラートを受信

データフローを実行すると、UI または電子メールでアラートを受け取ることができます。

### UI 内

アラートは、UI 内で、Platform UI の上部のヘッダーに通知アイコンで表されます。 通知アイコンを選択して、データフローに関する特定のアラートメッセージを表示します。

![通知](../../images/tutorials/alerts/notification.png)

通知パネルが表示され、作成したデータフローのステータス更新のリストが表示されます。

![alert-window](../../images/tutorials/alerts/alert-window.png)

アラートメッセージにマウスポインターを置いて読み取りとマークしたり、時計アイコンを選択して、データフローのステータスに関する将来のリマインダーを設定したりできます。

![remind-me](../../images/tutorials/alerts/remind-me.png)

アラートメッセージを選択して、データフローの特定の情報を表示します。

![select-alert-message](../../images/tutorials/alerts/select-alert-message.png)

この [!UICONTROL データフロー実行の概要] ページが表示されます。 画面の上半分には、属性に関する情報、対応するデータフロー実行 ID、エラー概要など、データフローの概要が表示されます。

![データフローの概要](../../images/tutorials/alerts/dataflow-overview.png)

ページの下半分には、 [!UICONTROL データフロー実行エラー] データフローの実行ステージで発生した問題を修正しました。 ここから、エラー診断をプレビューするか、 [[!DNL Data Access] API](https://www.adobe.io/experience-platform-apis/references/data-access/) ：データフローに対応するエラー診断またはファイルマニフェストをダウンロードする場合。

![dataflow-run-errors](../../images/tutorials/alerts/dataflow-run-error.png)

データフローエラーの処理について詳しくは、 [UI でのソースデータフローの監視](../../../dataflows/ui/monitor-sources.md).

### E メール別

また、データフローのアラートは E メールで配信されます。 データフローの詳細を確認するには、E メールの本文でデータフロー名を選択します。

![メール](../../images/tutorials/alerts/email.png)

UI アラートと同様に、 [!UICONTROL データフロー実行の概要] ページが表示され、データフローに関連するエラーを調査するためのインターフェイスが提供されます。

![データフローの概要](../../images/tutorials/alerts/dataflow-overview.png)

## アラートを購読および購読解除

内の既存のデータフローに関して、より多くのアラートを購読したり、確立されたアラートを購読解除したりできます。 [!UICONTROL データフロー] ページ。 リストから作成するデータフローを探し、省略記号 (`...`) をクリックして、オプションのドロップダウンメニューを表示します。 次に、 **[!UICONTROL アラートを購読]** をクリックして、データフローのアラート設定を変更します。

![options](../../images/tutorials/alerts/options.png)

ポップアップウィンドウが開き、ソースアラートのリストが表示されます。 購読するアラートを選択するか、購読を解除するアラートの選択を解除します。 完了したら「**[!UICONTROL 保存]**」を選択します。

![保存](../../images/tutorials/alerts/save.png)

## 次の手順

このドキュメントでは、ソースデータフローのコンテキスト内アラートをサブスクライブする方法に関するステップバイステップのガイドを提供しました。 詳しくは、 [alerts UI ガイド](../../../observability/alerts/ui.md).