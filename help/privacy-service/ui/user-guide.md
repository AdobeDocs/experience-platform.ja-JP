---
keywords: Experience Platform;home;popular topics;export;Export
solution: Experience Platform
title: Privacy Service ユーザーガイド
topic: UI guide
translation-type: tm+mt
source-git-commit: a09d80f4bacd5d4be77443d75aad278ad89259ef
workflow-type: tm+mt
source-wordcount: '1059'
ht-degree: 85%

---


# [!DNL Privacy Service] ユーザーガイド

This document provides steps for creating and managing privacy requests using the [!DNL Privacy Service] user interface.

## Browse the [!DNL Privacy Service] UI dashboard

The dashboard for the [!DNL Privacy Service] UI provides two widgets that allow you to view the status of your privacy jobs: **[!UICONTROL Status Report]** and **[!UICONTROL Job Requests]**. また、ダッシュボードには、表示されたジョブに対して現在選択されている規制も表示されます。

![UI ダッシュボード](../images/user-guide/dashboard.png)

### 規則の種類

[!DNL Privacy Service] は、次の4種類の規則タイプに対するジョブ要求をサポートします。

* 欧州和集合 [!DNL General Data Protection Regulation] ([!UICONTROL GDPR])
* The [!DNL California Consumer Privacy Act] ([!UICONTROL CCPA])
* ブラジル [!DNL Lei Geral de Proteção de Dados] ([!UICONTROL LGPD_BRA])
* タイ [!DNL Personal Data Protection Act] ([!UICONTROL PDPA_THA])

それぞれの規制タイプのジョブは、別々に追跡されます。規制タイプを切り替えるには、**[!UICONTROL Regulation Type]** ドロップダウンメニューをクリックし、リストから目的の規制を選択します。

![Regulation Type ドロップダウン](../images/user-guide/regulation.png)

規制の種類を変更すると、ダッシュボードが更新され、選択した規制に適用されるすべての操作、フィルター、ウィジェット、ジョブ作成のダイアログが表示されます。

![更新されたダッシュボード](../images/user-guide/dashboard-update.png)

### ステータスレポート

ステータスレポートウィジェットの左側のグラフは、エラーが発生してレポートが返された可能性のあるジョブについて、送信されたジョブを追跡します。右側のグラフは、30 日間のコンプライアンス期間の終わり近くにあるジョブを追跡します。

グラフの上にある 2 つの切り替えボタンの 1 つをクリックして、それぞれの指標の表示／非表示を切り替えます。

![](../images/user-guide/hide-errors.png)

グラフ上の任意のデータポイントに関連付けられているジョブの正確な数を表示するには、該当するデータポイントの上にマウスを移動します。

![データポイント上へのマウス移動](../images/user-guide/mouse-over.png)

特定の表示ポイントに関する詳細情報を表示するには、該当するデータポイントをクリックし、関連するジョブをジョブリクエストウィジェットに表示します。ジョブリストのすぐ上に適用されるフィルターをメモしておきます。

![ウィジェットでのフィルターの適用](../images/user-guide/apply-filter.png)

>[!NOTE]
>
> フィルターがジョブリクエストウィジェットに適用されている場合は、フィルターピルの「**X**」をクリックしてフィルターを削除できます。そうすれば、ジョブリクエストはデフォルトの追跡リストに戻ります。

### ジョブリクエスト

ジョブリクエストウィジェットには、リクエストの種類、現在のステータス、期日、要求者の E メールアドレスなどの詳細も含め、組織内で使用可能なすべてのジョブリクエストがリスト表示されます。

>[!NOTE]
>
> 以前に作成したジョブのデータは、完了日から 30 日間のみアクセスできます。

「Job Requests」タイトルの下の検索バーにキーワードを入力することで、このリストをフィルタリングできます。リストは、入力に応じて自動的にフィルタリングをおこない、検索用語に一致する値を含んだリクエストを表示します。**[!UICONTROL Requested on]** ドロップダウンメニューを使用して、リストに表示されているジョブの時間範囲を選択することもできます。

![ジョブリクエストの検索オプション](../images/user-guide/job-search.png)

特定のジョブリクエストの詳細を表示するには、リストでリクエストのジョブ ID をクリックして、**[!UICONTROL Job Details]** ページを開きます。

![GDPR UI のジョブ詳細](../images/user-guide/job-details.png)

This dialog contains status information about each [!DNL Experience Cloud] solution and its current state in relation to the overall job. プライバシージョブが非同期の場合は、各ソリューションの最新の通信日時（GMT）がページに表示されます。これは、リクエストの処理に他のソリューションより多くの時間が必要な場合があるからです。

ソリューションから追加のデータが提供された場合は、このダイアログで表示できます。このデータを表示するには、個々の製品行をクリックします。

完全なジョブデータを CSV ファイルとしてダウンロードするには、ダイアログの右上にある「**[!UICONTROL Export to CSV]**」をクリックします。

## プライバシージョブリクエストの新規作成

>[!NOTE]
>
> プライバシージョブリクエストを作成するには、アクセスまたは削除するデータの所有者である特定の顧客の ID 情報を指定する必要があります。この節を続行する前に、[プライバシーリクエストの ID データ](../identity-data.md)に関するドキュメントを確認してください。

The [!DNL Privacy Service] UI provides two methods to create new job requests:

* [リクエストビルダーの使用](#request-builder)
* [JSON ファイルのアップロード](#json)

これらの各方法の使用手順について、次の節で説明します。

### リクエストビルダーの使用 {#request-builder}

リクエストビルダーを使用すると、ユーザーインターフェイスで新しいプライバシージョブリクエストを手動で作成できます。リクエストビルダーは、リクエストをユーザーごとに 1 つの ID タイプに制限するので、よりシンプルでより小さなリクエストセットに最適です。より複雑なリクエストについては、代わりに [JSON ファイルをアップロード](#json)する方がよい場合があります。

リクエストビルダーの使用を開始するには、画面の右側でステータスレポートウィジェットの下にある「**[!UICONTROL Create Request]**」をクリックします。

![「Create Request」のクリック](../images/user-guide/create-request.png)

**[!UICONTROL Create Request]** ダイアログが開き、現在選択されている規制タイプのプライバシージョブリクエストを送信するために使用できるオプションが表示されます。

<img src="../images/user-guide/request-builder.png" width="500" /><br/>

リクエストの「**[!UICONTROL Job Type]**」（「Delete」または「Access」）を選択し、該当する製品を「**[!UICONTROL Products]**」のリストから 1 つ以上選択します。

<img src="../images/user-guide/type-and-products.png" width="500" /><br/>

Under **[!UICONTROL Namespace type]**, select the appropriate namespace type for the customer IDs being sent to [!DNL Privacy Service].

<img src="../images/user-guide/namespace-type.png" width="500" /><br/>

_standard_ タイプの名前空間を使用する場合は、ドロップダウンメニューから名前空間（E メール、ECID、AAID のいずれか）を選択し、右側のテキストボックスに ID 値を入力し、ID ごとに **Enter** キーを押してリストに追加します。

<img src="../images/user-guide/standard-namespace.png" width="500" /><br/>

_custom_ タイプの名前空間を使用する場合は、名前空間を手動で入力してから、その下で ID 値を入力する必要があります。

<img src="../images/user-guide/custom-namespace.png" width="500" /><br/>

完了したら、「**[!UICONTROL Create]**」をクリックします。

<img src="../images/user-guide/request-builder-create.png" width="500" /><br/>

ダイアログが閉じ、新しいジョブ（複数の場合あり）が現在の処理ステータスと共にジョブリクエストウィジェットにリスト表示されます。

### JSON ファイルのアップロード {#json}

処理するデータサブジェクトごとに複数の ID タイプを使用するリクエストなど、より複雑なリクエストを作成する場合は、JSON ファイルをアップロードしてリクエストを作成できます。

画面の右側でステータスレポートウィジェットの下にある「**[!UICONTROL Create Request]**」の右横の矢印をクリックします。表示されるオプションリストから、「**[!UICONTROL Upload JSON]**」を選択します。

![リクエスト作成オプション](../images/user-guide/create-options.png)

**[!UICONTROL Upload JSON]** ダイアログが開き、JSON ファイルをドラッグ＆ドロップできるウィンドウが表示されます。

<img src="../images/user-guide/upload-json.png" width="500" /><br/>

アップロードする JSON ファイルがない場合は、「**[!UICONTROL Download Adobe-GDPR-Request.json]**」をクリックして、データサブジェクトから収集した値に従って入力できるテンプレートをダウンロードします。


<img src="../images/user-guide/privacy-template.png" width="500" /><br/>


コンピューター上で JSON ファイルを探し、ダイアログウィンドウにドラッグします。アップロードが正常に完了すると、ダイアログにファイル名が表示されます。必要に応じて、引き続き JSON ファイルをダイアログにドラッグ＆ドロップして追加できます。

完了したら、「**[!UICONTROL Create]**」をクリックします。ダイアログが閉じ、新しいジョブ（複数の場合あり）が現在の処理ステータスと共に&#x200B;_ジョブリクエスト_&#x200B;ウィジェットにリスト表示されます。

### 次の手順

By reading this document, you have learned how to use the [!DNL Privacy Service] UI to create a privacy job, view a job&#39;s details and monitor its processing status, and download the results once it has completed.

For steps on how to perform these operations programmatically using the [!DNL Privacy Service] API, please refer to the [developer guide](../api/getting-started.md).