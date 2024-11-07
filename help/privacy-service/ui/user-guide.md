---
keywords: Experience Platform；ホーム；人気のトピック；エクスポート；エクスポート
solution: Experience Platform
title: Privacy ServiceUI でのプライバシージョブの管理
description: Privacy Serviceユーザーインターフェイスを使用して、様々なExperience Cloudアプリケーションをまたいでプライバシーリクエストを調整および監視する方法について説明します。
exl-id: aa8b9f19-3e47-4679-9679-51add1ca2ad9
source-git-commit: c870b6be603073d6dd909b272c619deb5b246f05
workflow-type: tm+mt
source-wordcount: '1770'
ht-degree: 45%

---

# Privacy Service UI でプライバシージョブを管理 {#user-guide}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_requests_description"
>title="データ主体のプライバシーリクエストに従う"
>abstract="<h2>説明</h2><p>Adobe Experience Platform Privacy Service では、法的プライバシー規制に従って、個人データにアクセスしたり削除したりする顧客に代わって、プライバシーリクエストを作成および管理できます。</p>"

このドキュメントでは、[!DNL Privacy Service] ユーザーインターフェイスを使用してプライバシーリクエストを作成および管理する手順について説明します。

>[!IMPORTANT]
>
>Privacy Service は、データ主体と消費者の権利リクエストのみを目的としています。それ以外にデータのクリーンアップやメンテナンスに Privacy Service を使用することは、サポートされておらず、許可もされていません。アドビには、それらをタイムリーに履行する法的義務があります。したがって、これが実稼動専用の環境であり、有効なプライバシーリクエストの不要なバックログが作成されるので、Privacy Service の読み込みテストは許可されていません。
>
>サービスの不正使用を防ぐために、1 日あたりのアップロードに対するハードリミットが設定されるようになりました。システムの不正使用が判明したユーザーは、サービスへのアクセスが無効になります。その後、それらのユーザーのアクションに対処するための会議がユーザー本人を交えて開催され、Privacy Service の適切な使用について議論が行われます。

## [!DNL Privacy Service] UI ダッシュボードの参照

[!DNL Privacy Service] UI 用のダッシュボードには、プライバシージョブのステータスを表示できる「[!UICONTROL  ステータスレポート ]」および「[!UICONTROL  ジョブリクエスト ]」の 2 つのウィジェットが用意されています。 また、ダッシュボードには、表示されたジョブに対して現在選択されている規制も表示されます。

![UI ダッシュボード](../images/user-guide/dashboard.png)

### 規則の種類

[!DNL Privacy Service] は、複数のプライバシー規制に対するジョブリクエストをサポートしています。 次の表に、UI で表される、サポートされる規制と対応するラベルを示します。

| UI ラベル | 規則 |
|-------------------------------------|------------------------|
| [!UICONTROL APA_AUS （オーストラリア） ] | [!DNL Australia Privacy Act] |
| [!UICONTROL CCPA （カリフォルニア州） ] | [!DNL California Consumer Privacy Act] |
| [!UICONTROL CPA_USA （コロラド州） ] | [!DNL Colorado Privacy Act] |
| [!UICONTROL CPRA_USA （カリフォルニア州） ] | [!DNL California Consumer Privacy Rights Act (CPRA)] |
| [!UICONTROL CTDPA_USA （コネチカット州） ] | [!DNL Connecticut Data Privacy Act] |
| [!UICONTROL DPDPA_USA （デラウェア州） ] | [!DNL Delaware Personal Data Privacy Act] |
| [!UICONTROL FDBR_USA （フロリダ州） ] | [!DNL Florida Digital Bill of Rights] |
| [!UICONTROL GDPR （欧州連合） ] | 欧州連合 [!DNL General Data Protection Regulation] |
| [!UICONTROL HIPPA_USA （米国） ] | [!DNL Health Insurance Portability and Accountability Act] |
| [!UICONTROL ICDPA_USA] （アイオワ州） | [!DNL Iowa Consumer Data Protection Act] |
| [!UICONTROL LGPD_BRA （ブラジル） ] | ブラジルの「[!DNL General Data Protection Law]」 [!DNL Lei Geral de Proteção de Dados] |
| [!UICONTROL MHMDA_USA （ワシントン） ] | [!DNL Washington My Health My Data Act] |
| [!UICONTROL MCDPA_USA （モンタナ） ] | [!DNL Montana Consumer Data Privacy Act] |
| [!UICONTROL NDPA_USA （ネブラスカ州） ] | [!DNL Nebraska Data Protection Act] |
| [!UICONTROL NZPA_NZL （ニュージーランド） ] | ニュージーランド [!DNL Privacy Act] |
| [!UICONTROL NHPA_USA （ニューハンプシャー州） ] | [!DNL New Hampshire Privacy Act] |
| [!UICONTROL NJDPA_USA （ニュージャージー州） ] | [!DNL New Jersey Data Protection Act] |
| [!UICONTROL OCPA USA （オレゴン） ] | [!DNL Oregon Consumer Privacy Act] |
| [!UICONTROL PDPA_THA （タイ） ] | タイの [!DNL Personal Data Protection Act] |
| [!UICONTROL QL25_CAN （Quebec） ] | [!DNL Quebec Law 25] |
| [!UICONTROL TDPSA USA （テキサス州） ] | [!DNL Texas Data Privacy and Security Act] |
| [!UICONTROL UCPA_USA （ユタ州） ] | [!DNL Utah Consumer Privacy Act] |
| [!UICONTROL VCDPA_USA （バージニア） ] | [!DNL Virginia Consumer Data Protection Act] |

{style="table-layout:auto"}

<!-- 
Waiting:
| **[!UICONTROL PIPA_KOR]**  ?        | South Korea [!DNL Personal Information Privacy Act] |
 -->

>[!NOTE]
>
>各規制の法的背景について詳しくは、[ サポートされるプライバシー規制 ](../regulations/overview.md) の概要を参照してください。

それぞれの規制タイプのジョブは、別々に追跡されます。規制タイプを切り替えるには、「**[!UICONTROL 規制タイプ]**」ドロップダウンメニューを選択し、リストから目的の規制を選択します。

![ 規制タイプのドロップダウンを含むPrivacy Serviceコンソール。](../images/user-guide/regulation.png)

規制の種類を変更すると、ダッシュボードが更新され、選択した規制に適用されるすべての操作、フィルター、ウィジェット、ジョブ作成のダイアログが表示されます。

![更新されたダッシュボード](../images/user-guide/dashboard-update.png)

### ステータスレポート

ステータスレポートウィジェットの左側のグラフは、エラーが発生してレポートが返された可能性のあるジョブについて、送信されたジョブを追跡します。右側のグラフは、30 日間のコンプライアンス期間の終わり近くにあるジョブを追跡します。

グラフの上にある 2 つのトグルボタンの 1 つを選択して、それぞれの指標の表示と非表示を切り替えます。

![](../images/user-guide/hide-errors.png)

グラフ上の任意のデータポイントに関連付けられているジョブの正確な数を表示するには、該当するデータポイントの上にマウスを移動します。

![データポイント上へのマウス移動](../images/user-guide/mouse-over.png)

特定のデータポイントの詳細を表示するには、該当するデータポイントを選択して、ジョブリクエスト ウィジェットに関連するジョブを表示します。 ジョブリストのすぐ上に適用されるフィルターをメモしておきます。

![ウィジェットでのフィルターの適用](../images/user-guide/apply-filter.png)

>[!NOTE]
>
>ジョブリクエストウィジェットにフィルターが適用されている場合は、フィルターピルの **X** を選択することでフィルターを削除できます。 そうすれば、ジョブリクエストはデフォルトの追跡リストに戻ります。

### ジョブリクエスト {#job-requests}

[!UICONTROL  ジョブリクエスト ] ワークスペースには、組織の最近のジョブリクエストに関する詳細がリストされます。 詳細には、リクエストタイプ、現在のステータス、期限、リクエスト者のメールなどが含まれます。 一度に 100 件のレコードのセットが読み込まれます。 デフォルトでは、最近作成されたジョブが上部に表示され、下にスクロールして参照すると、より多くのレコードセットが読み込まれます。

>[!NOTE]
>
>以前作成したジョブのデータには、完了日から 30 日間のみアクセスできます。

[!UICONTROL  ジョブリクエスト ] タイトルの下の検索バーにキーワードを入力して、リストをフィルタリングできます。 リストは、入力に応じて自動的にフィルタリングをおこない、検索用語に一致する値を含んだリクエストを表示します。検索フィールドは、プライバシージョブ ID を、UI で現在レンダリングされている/読み込まれているジョブに一致させる「クイック」検索を実行します。 送信されたすべてのジョブを包括的に検索するわけではありません。 代わりに、読み込まれた結果に適用されるフィルターになります。 Privacy ServiceAPI を使用して [ 特定の規制、日付範囲または単一のジョブに基づいてジョブを返す ](../api/privacy-jobs.md#list) ことができます。

>[!TIP]
>
>過去 30 日間のレコードを UI に読み込むには、テーブルを下にスクロールして、さらにレコードのバッチを読み込む必要があります。

![ 検索フィールドがハイライト表示されたプライバシーコンソールジョブリクエストセクション ](../images/user-guide/job-search.png)

または、検索ボタンを使用して、特定の日付範囲にまたがるプライバシージョブクエリを実行します。 このアクションは、指定された期間内に組織によって送信されたすべてのプライバシージョブを返します。 **[!UICONTROL リクエスト日]** ドロップダウンメニューを選択して、クエリの開始日と終了日を選択します。 使用できるオプションには、「[!UICONTROL  今日 ]」、「[!UICONTROL  過去 7 日間 ]」、「[!UICONTROL  過去 2 週間 ]」、「[!UICONTROL  過去 30 日間 ]」または「[!UICONTROL  カスタム ] があります。 「[!UICONTROL  リクエスト日 ]」オプションと共に使用すると、検索機能には、選択した日付範囲の間に送信されたジョブリクエストのみが表示されます。

![ 検索フィールド、「リクエスト対象」ドロップダウンメニュー、「検索」ボタンがハイライト表示された「ジョブリクエスト」セクション ](../images/user-guide/requested-on-dropdown-menu.png)

特定のジョブリクエストの詳細を表示するには、リストからリクエストのジョブ ID を選択して **[!UICONTROL ジョブの詳細]** ページを開きます。

![GDPR UI のジョブ詳細](../images/user-guide/job-details.png)

このダイアログには、各 [!DNL Experience Cloud] ソリューションに関するステータス情報と、ジョブ全体に対する現在の状態が表示されます。 プライバシージョブが非同期の場合は、各ソリューションの最新の通信日時（GMT）がページに表示されます。これは、リクエストの処理に他のソリューションより多くの時間が必要な場合があるからです。

ソリューションから追加のデータが提供された場合は、このダイアログで表示できます。このデータは、個々の製品行を選択して表示できます。

完全なジョブデータを CSV ファイルとしてダウンロードするには、ダイアログの右上にある **[!UICONTROL CSV に書き出し]** を選択します。

## プライバシージョブリクエストの新規作成 {#create-a-new-privacy-job-request}

>[!CONTEXTUALHELP]
>id="platform_privacyConsole_requests_instructions"
>title="手順"
>abstract="<ul><li>左側のナビゲーションで「<a href="https://experienceleague.adobe.com/docs/experience-platform/privacy/ui/overview.html?lang=ja#logging-in-from-experience-platform">リクエスト</a>」を選択してプライバシー Ul を開き、「<b>リクエストを作成</b>」を選択します。</li><li>ここから、リクエストビルダーを使用するか、データ主体の JSON ファイルをアップロードできます。</li><li>リクエストビルダーを使用する場合、ジョブのタイプ（アクセスまたは削除）を選択し、指定する ID のタイプ（メール、ECID、AAID）を選択するか、カスタム ID 名前空間を入力します。顧客の適切な ID 値を入力し、終了したら「<b>作成</b>」を選択します。</li><li>JSON ファイルをアップロードする場合は、「リクエストを作成」の横にある矢印を選択します。オプションのリストから、「<b>JSON をアップロード</b>」を選択し、ファイルをアップロードします。アップロードする JSON ファイルがない場合は、「<b>Adobe-GDPR-Request.json をダウンロード</b>」を選択して、入力できるテンプレートをダウンロードします。JSON をアップロードし、完了したら「<b>作成</b>」を選択します。</li><li>この機能に関する詳しいヘルプについては、Experience League の <a href="https://experienceleague.adobe.com/docs/experience-platform/privacy/ui/user-guide.html?lang=ja">Privacy Service ユーザーガイド</a>を参照してください。</li></ul>"

>[!NOTE]
>
>プライバシージョブリクエストを作成するには、データがアクセスまたは削除される特定の顧客の ID 情報を指定する必要があります。 この節を続行する前に、[プライバシーリクエストの ID データ](../identity-data.md)に関するドキュメントを確認してください。

[!DNL Privacy Service] UI には、新しいジョブリクエストを作成する 2 つの方法が用意されています。

* [リクエストビルダーの使用](#request-builder)
* [JSON ファイルのアップロード](#json)

これらの各方法の使用手順について、次の節で説明します。

### リクエストビルダーの使用 {#request-builder}

リクエストビルダーを使用すると、ユーザーインターフェイスで新しいプライバシージョブリクエストを手動で作成できます。リクエストビルダーは、リクエストをユーザーごとに 1 つの ID タイプに制限するので、よりシンプルでより小さなリクエストセットに最適です。より複雑なリクエストについては、代わりに [JSON ファイルをアップロード](#json)する方がよい場合があります。

リクエストビルダーの使用を開始するには、画面の右側にあるステータスレポートウィジェットの下の「**[!UICONTROL リクエストを作成]**」を選択します。

![ リクエストを作成を選択 ](../images/user-guide/create-request.png)

**[!UICONTROL Create Request]** ダイアログが開き、現在選択されている規制タイプのプライバシージョブリクエストを送信するために使用できるオプションが表示されます。

<img src="../images/user-guide/request-builder.png" width="500" /><br/>

リクエストの **[!UICONTROL ジョブタイプ]** （「削除」または「アクセス」）と 1 つ以上の使用可能な製品をリストから選択します。

Privacy Serviceは、個人データに対して、[!UICONTROL  アクセス ] （読み取り）、[!UICONTROL  削除 ] の 2 種類のジョブリクエストをサポートしています。 お問い合わせの対象に関連する製品に保持されているすべての情報を受け取るリクエストを送信するか、お問い合わせの対象に関連するすべての情報を削除するリクエストを送信できます。

<img src="../images/user-guide/type-and-products.png" width="500" /><br/>

**[!UICONTROL 名前空間タイプ]** で、[!DNL Privacy Service] に送信される顧客 ID に適した名前空間タイプを選択します。

<img src="../images/user-guide/namespace-type.png" width="500" /><br/>

標準の名前空間タイプを使用する場合、ドロップダウンメニュー（メール、ECID、AAID）から名前空間を選択し、右側のテキストボックスに ID 値を入力します。ID ごとに **\&lt;enter>** キーを押して、リストに追加します。

<img src="../images/user-guide/standard-namespace.png" width="500" /><br/>

カスタム名前空間タイプを使用する場合、以下の ID 値を指定する前に、名前空間を手動で入力する必要があります。

<img src="../images/user-guide/custom-namespace.png" width="500" /><br/>

完了したら、「**[!UICONTROL 作成]**」をクリックします。

<img src="../images/user-guide/request-builder-create.png" width="500" /><br/>

ダイアログが閉じ、新しいジョブ（複数の場合あり）が現在の処理ステータスと共にジョブリクエストウィジェットにリスト表示されます。

### JSON ファイルのアップロード {#json}

処理するデータサブジェクトごとに複数の ID タイプを使用するリクエストなど、より複雑なリクエストを作成する場合は、JSON ファイルをアップロードしてリクエストを作成できます。

画面の右側にあるステータスレポートウィジェットの下にある **[!UICONTROL リクエストを作成]** の横の矢印を選択します。 表示されるオプションリストから、「**[!UICONTROL Upload JSON]**」を選択します。

![リクエスト作成オプション](../images/user-guide/create-options.png)

**[!UICONTROL Upload JSON]** ダイアログが開き、JSON ファイルをドラッグ＆ドロップできるウィンドウが表示されます。

<img src="../images/user-guide/upload-json.png" width="500" /><br/>

アップロードする JSON ファイルがない場合は、「**[!UICONTROL ダウンロードAdobe-GDPR-Request.json]**」を選択して、データ主体から収集した値に従って入力できるテンプレートをダウンロードします。


<img src="../images/user-guide/privacy-template.png" width="500" /><br/>


コンピューター上で JSON ファイルを探し、ダイアログウィンドウにドラッグします。アップロードが正常に完了すると、ダイアログにファイル名が表示されます。必要に応じて、引き続き JSON ファイルをダイアログにドラッグ＆ドロップして追加できます。

終了したら、「**[!UICONTROL 作成]**」を選択します。 ダイアログが閉じ、新しいジョブ（複数の場合あり）が現在の処理ステータスと共にジョブリクエストウィジェットにリスト表示されます。

### 次の手順

このドキュメントでは、[!DNL Privacy Service] UI を使用してプライバシージョブを作成し、ジョブの詳細を表示し、処理ステータスを監視して、完了後に結果をダウンロードする方法について説明しました。

[!DNL Privacy Service] API を使用してこれらの操作をプログラムで実行する手順については、[API ガイド ](../api/overview.md) を参照してください。
