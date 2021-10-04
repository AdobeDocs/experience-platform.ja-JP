---
keywords: Experience Platform；ホーム；人気の高いトピック；書き出し；書き出し
solution: Experience Platform
title: プライバシー UI でのPrivacy Serviceジョブの管理
topic-legacy: UI guide
description: Privacy Serviceユーザーインターフェイスを使用して、様々なExperience Cloudアプリケーション間でプライバシーリクエストを調整および監視する方法を説明します。
exl-id: aa8b9f19-3e47-4679-9679-51add1ca2ad9
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1100'
ht-degree: 60%

---

# プライバシー UI でのPrivacy Serviceジョブの管理

このドキュメントでは、[!DNL Privacy Service] ユーザーインターフェイスを使用してプライバシーリクエストを作成および管理する手順を説明します。

## [!DNL Privacy Service] UI ダッシュボードを参照

[!DNL Privacy Service] UI のダッシュボードには、プライバシージョブのステータスを表示できる 2 つのウィジェットが用意されています。&quot;[!UICONTROL  ステータスレポート ]&quot;および&quot;[!UICONTROL  ジョブリクエスト ]&quot; また、ダッシュボードには、表示されたジョブに対して現在選択されている規制も表示されます。

![UI ダッシュボード](../images/user-guide/dashboard.png)

### 規則の種類

[!DNL Privacy Service] は、複数のプライバシー規制に対するジョブリクエストをサポートしています。

* [!DNL California Consumer Privacy Act] ([!UICONTROL CCPA])
* 欧州連合の [!DNL General Data Protection Regulation] ([!UICONTROL GDPR])
* タイの [!DNL Personal Data Protection Act] ([!UICONTROL PDPA_THA])
* ブラジルの [!DNL Lei Geral de Proteção de Dados] ([!UICONTROL LGPD_BRA])
* ニュージーランド [!DNL Privacy Act] ([!UICONTROL NZPA_NZL])

それぞれの規制タイプのジョブは、別々に追跡されます。規制タイプを切り替えるには、**[!UICONTROL Regulation Type]** ドロップダウンメニューを選択し、リストから目的の規制を選択します。

![Regulation Type ドロップダウン](../images/user-guide/regulation.png)

規制の種類を変更すると、ダッシュボードが更新され、選択した規制に適用されるすべての操作、フィルター、ウィジェット、ジョブ作成のダイアログが表示されます。

![更新されたダッシュボード](../images/user-guide/dashboard-update.png)

### ステータスレポート

ステータスレポートウィジェットの左側のグラフは、エラーが発生してレポートが返された可能性のあるジョブについて、送信されたジョブを追跡します。右側のグラフは、30 日間のコンプライアンス期間の終わり近くにあるジョブを追跡します。

グラフの上にある 2 つの切り替えボタンの 1 つを選択して、それぞれの指標の表示/非表示を切り替えます。

![](../images/user-guide/hide-errors.png)

グラフ上の任意のデータポイントに関連付けられているジョブの正確な数を表示するには、該当するデータポイントの上にマウスを移動します。

![データポイント上へのマウス移動](../images/user-guide/mouse-over.png)

特定のデータポイントに関する詳細を表示するには、該当するデータポイントを選択して、関連するジョブをジョブリクエストウィジェットに表示します。 ジョブリストのすぐ上に適用されるフィルターをメモしておきます。

![ウィジェットでのフィルターの適用](../images/user-guide/apply-filter.png)

>[!NOTE]
>
>フィルターがジョブリクエストウィジェットに適用されている場合は、フィルターピルの **X** を選択して、フィルターを削除できます。 そうすれば、ジョブリクエストはデフォルトの追跡リストに戻ります。

### ジョブリクエスト

ジョブリクエストウィジェットには、リクエストの種類、現在のステータス、期日、要求者の E メールアドレスなどの詳細も含め、組織内で使用可能なすべてのジョブリクエストがリスト表示されます。

>[!NOTE]
>
> 以前に作成したジョブのデータは、完了日から 30 日間のみアクセスできます。

「Job Requests」タイトルの下の検索バーにキーワードを入力することで、このリストをフィルタリングできます。リストは、入力に応じて自動的にフィルタリングをおこない、検索用語に一致する値を含んだリクエストを表示します。**[!UICONTROL Requested on]** ドロップダウンメニューを使用して、リストに表示されているジョブの時間範囲を選択することもできます。

![ジョブリクエストの検索オプション](../images/user-guide/job-search.png)

特定のジョブリクエストの詳細を表示するには、リストからリクエストのジョブ ID を選択して、**[!UICONTROL ジョブの詳細]** ページを開きます。

![GDPR UI のジョブ詳細](../images/user-guide/job-details.png)

このダイアログには、各 [!DNL Experience Cloud] ソリューションのステータス情報と、ジョブ全体に関する現在の状態が表示されます。 プライバシージョブが非同期の場合は、各ソリューションの最新の通信日時（GMT）がページに表示されます。これは、リクエストの処理に他のソリューションより多くの時間が必要な場合があるからです。

ソリューションから追加のデータが提供された場合は、このダイアログで表示できます。このデータを表示するには、個々の製品行を選択します。

完全なジョブデータを CSV ファイルとしてダウンロードするには、ダイアログの右上にある「**[!UICONTROL CSV に書き出し]**」を選択します。

## プライバシージョブリクエストの新規作成

>[!NOTE]
>
> プライバシージョブリクエストを作成するには、アクセスまたは削除するデータの所有者である特定の顧客の ID 情報を指定する必要があります。この節を続行する前に、[プライバシーリクエストの ID データ](../identity-data.md)に関するドキュメントを確認してください。

[!DNL Privacy Service] UI には、新しいジョブリクエストを作成する 2 つの方法が用意されています。

* [リクエストビルダーの使用](#request-builder)
* [JSON ファイルのアップロード](#json)

これらの各方法の使用手順について、次の節で説明します。

### リクエストビルダーの使用 {#request-builder}

リクエストビルダーを使用すると、ユーザーインターフェイスで新しいプライバシージョブリクエストを手動で作成できます。リクエストビルダーは、リクエストをユーザーごとに 1 つの ID タイプに制限するので、よりシンプルでより小さなリクエストセットに最適です。より複雑なリクエストについては、代わりに [JSON ファイルをアップロード](#json)する方がよい場合があります。

リクエストビルダーの使用を開始するには、画面の右側でステータスレポートウィジェットの下にある「**[!UICONTROL Create Request]**」を選択します。

![「リクエストを作成」を選択します。](../images/user-guide/create-request.png)

**[!UICONTROL Create Request]** ダイアログが開き、現在選択されている規制タイプのプライバシージョブリクエストを送信するために使用できるオプションが表示されます。

<img src="../images/user-guide/request-builder.png" width="500" /><br/>

リクエストの **[!UICONTROL ジョブタイプ]**（「削除」または「アクセス」）を選択し、リストから 1 つ以上の使用可能な製品を選択します。

<img src="../images/user-guide/type-and-products.png" width="500" /><br/>

「**[!UICONTROL Namespace type]**」で、[!DNL Privacy Service] に送信する顧客 ID に適した名前空間のタイプを選択します。

<img src="../images/user-guide/namespace-type.png" width="500" /><br/>

standard タイプの名前空間を使用する場合は、ドロップダウンメニューから名前空間（E メール、ECID、AAID のいずれか）を選択し、右側のテキストボックスに ID 値を入力し、ID ごとに **Enter** キーを押してリストに追加します。

<img src="../images/user-guide/standard-namespace.png" width="500" /><br/>

custom タイプの名前空間を使用する場合は、名前空間を手動で入力してから、その下で ID 値を入力する必要があります。

<img src="../images/user-guide/custom-namespace.png" width="500" /><br/>

完了したら、「**[!UICONTROL 作成]**」をクリックします。

<img src="../images/user-guide/request-builder-create.png" width="500" /><br/>

ダイアログが閉じ、新しいジョブ（複数の場合あり）が現在の処理ステータスと共にジョブリクエストウィジェットにリスト表示されます。

### JSON ファイルのアップロード {#json}

処理するデータサブジェクトごとに複数の ID タイプを使用するリクエストなど、より複雑なリクエストを作成する場合は、JSON ファイルをアップロードしてリクエストを作成できます。

画面の右側でステータスレポートウィジェットの下にある「**[!UICONTROL Create Request]**」の横の矢印を選択します。 表示されるオプションリストから、「**[!UICONTROL Upload JSON]**」を選択します。

![リクエスト作成オプション](../images/user-guide/create-options.png)

**[!UICONTROL Upload JSON]** ダイアログが開き、JSON ファイルをドラッグ＆ドロップできるウィンドウが表示されます。

<img src="../images/user-guide/upload-json.png" width="500" /><br/>

アップロードする JSON ファイルがない場合は、「**[!UICONTROL Download template-GDPR-Request.json]**」を選択して、データサブジェクトから収集した値に従って入力できるAdobeをダウンロードします。


<img src="../images/user-guide/privacy-template.png" width="500" /><br/>


コンピューター上で JSON ファイルを探し、ダイアログウィンドウにドラッグします。アップロードが正常に完了すると、ダイアログにファイル名が表示されます。必要に応じて、引き続き JSON ファイルをダイアログにドラッグ＆ドロップして追加できます。

完了したら、「**[!UICONTROL 作成]**」をクリックします。ダイアログが閉じ、新しいジョブ（複数の場合あり）が現在の処理ステータスと共にジョブリクエストウィジェットにリスト表示されます。

### 次の手順

このドキュメントでは、[!DNL Privacy Service] UI を使用してプライバシージョブを作成し、ジョブの詳細を表示し、処理ステータスを監視し、完了後に結果をダウンロードする方法を学びました。

[!DNL Privacy Service] API を使用してこれらの操作をプログラムで実行する手順については、[ 開発者ガイド ](../api/getting-started.md) を参照してください。
