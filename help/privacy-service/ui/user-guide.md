---
keywords: Experience Platform；ホーム；人気のトピック；エクスポート；エクスポート
solution: Experience Platform
title: Privacy Service UI でのプライバシージョブの管理
description: Privacy Service ユーザーインターフェイスを使用して、様々なExperience Cloud アプリケーションをまたいでプライバシーリクエストを調整および監視する方法について説明します。
exl-id: aa8b9f19-3e47-4679-9679-51add1ca2ad9
source-git-commit: b960e67789acaeb27a0a39db933a2bbb7d84f4d5
workflow-type: tm+mt
source-wordcount: '1689'
ht-degree: 44%

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

[!DNL Privacy Service] UI のダッシュボードには、プライバシージョブのステータスを表示できる「[!UICONTROL Status Report]」および「[!UICONTROL Job Requests]」の 2 つのウィジェットが用意されています。 また、ダッシュボードには、表示されたジョブに対して現在選択されている規制も表示されます。

![UI ダッシュボード](../images/user-guide/dashboard.png)

### 規則の種類

[!DNL Privacy Service] は、複数のプライバシー規制に対するジョブリクエストをサポートしています。 次の表に、UI で表される、サポートされる規制と対応するラベルを示します。

消費者の権利と義務付けられているビジネス義務について説明する各規制の説明については、[ プライバシー規制の概要 ](../regulations/overview.md) を参照してください。

>[!TIP]
>
>API 規制タイプは、一般的な利便性のために含まれています。

| UI ラベル | API `regulation_type` | 規制 |
|-------------------------------------------|-----------------------|----------------|
| [!UICONTROL APA_AUS (Australia)] | `apa_aus` | [!DNL Australia Privacy Act] |
| [!UICONTROL CCCA (California)] | `ccpa` | [!DNL California Consumer Privacy Act] （CCPA） |
| [!UICONTROL CPA_CO_USA (Colorado)] | `cpa_co_usa` | [!DNL Colorado Privacy Act] |
| [!UICONTROL CPRA_CA_USA (California)] | `cpra_ca_usa` | [!DNL California Privacy Rights Act] （CPRA） |
| [!UICONTROL CTDPA_CT_USA (Connecticut)] | `ctdpa_ct_usa` | [!DNL Connecticut Data Privacy Act] |
| [!UICONTROL DPDPA_DE_USA (Delaware)] | `dpdpa_de_usa` | [!DNL Delaware Personal Data Privacy Act] |
| [!UICONTROL FDBR_FL_USA (Florida)] | `fdbr_fl_usa` | [!DNL Florida Digital Bill of Rights] |
| [!UICONTROL GDPR (European Union)] | `gdpr` | 欧州連合 [!DNL General Data Protection Regulation] |
| [!UICONTROL HIPAA_USA (United States)] | `hipaa_usa` | [!DNL Health Insurance Portability and Accountability Act] |
| [!UICONTROL ICDPALIA_USA (Iowa)] | `icdpa_ia_usa` | [!DNL Iowa Consumer Data Protection Act] |
| [!UICONTROL LGPD_BRA (Brazil)] | `lgpd_bra` | ブラジルの「[!DNL General Data Protection Law]」 [!DNL Lei Geral de Proteção de Dados] |
| [!UICONTROL MCDPA_MN_USA (Minnesota)] | `mcdpa_mn_usa` | [!DNL Minnesota Consumer Data Privacy Act] |
| [!UICONTROL MCDPA_MT_USA (Montana)] | `mcdpa_mt_usa` | [!DNL Montana Consumer Data Privacy Act] |
| [!UICONTROL MHMDA_WA_USA (Washington)] | `mhmda_wa_usa` | [!DNL Washington My Health My Data Act] |
| [!UICONTROL MODPA_MD_USA (Maryland)] | `modpa_md_usa` | [!DNL Maryland Online Data Privacy Act] |
| [!UICONTROL NDPA_NE_USA (Nebraska)] | `ndpa_ne_usa` | [!DNL Nebraska Data Protection Act] |
| [!UICONTROL NHPA_NH_USA (New Hampshire)] | `nhpa_nh_usa` | [!DNL New Hampshire Privacy Act] |
| [!UICONTROL NJDPA_NJ_USA (New Jersey)] | `njdpa_nj_usa` | [!DNL New Jersey Data Protection Act] |
| [!UICONTROL NZPA_NZL (New Zealand)] | `nzpa_nzl` | ニュージーランド [!DNL Privacy Act] （PA） |
| [!UICONTROL OCPA_OR_USA (Oregon)] | `ocpa_or_usa` | [!DNL Oregon Consumer Privacy Act] |
| [!UICONTROL PDPA_THA (Thailand)] | `pdpa_tha` | タイの [!DNL Personal Data Protection Act] （PDPA） |
| [!UICONTROL PIPA_KOR (South Korea)] | `pipa_kor` | 韓国 [!DNL Personal Information Privacy Act] （PIPA） |
| [!UICONTROL QL25_QC_CAN (Quebec)] | `ql25_qc_can` | [!DNL Quebec Law 25] |
| [!UICONTROL TDPSA_TX_USA (Texas)] | `tdpsa_tx_usa` | [!DNL Texas Data Privacy and Security Act] |
| [!UICONTROL TIPA_TN_USA (Tennessee)] | `tipa_tn_usa` | [!DNL Tennessee Information Protection Act] |
| [!UICONTROL UCPA_UT_USA (Utah)] | `ucpa_ut_usa` | [!DNL Utah Consumer Privacy Act] |
| [!UICONTROL VCDPA_VA_USA (Virginia)] | `vcdpa_va_usa` | [!DNL Virginia Consumer Data Protection Act] （VCDPA） |

{style="table-layout:auto"}

<!-- | [!UICONTROL ICDPA_IN_USA (Indiana)]       | `icdpa_in_usa` | [!DNL Indiana Consumer Data Protection Act]| NOT SUPP YET JAN 1st ### ... -->
<!-- | [!UICONTROL KCDPA_KY_USA (Kentucky)]      | `kcdpa_ky_usa`| [!DNL Kentucky Consumer Data Protection Act]|  NOT SUPP YET JAN 1st ### ... -->
<!-- | [!UICONTROL RIDTPPA_RI_USA (Rhode Island)]| `ridtppa_ri_usa` | [!DNL Rhode Island Data Transparency and Privacy Protection Act]|  NOT SUPP YET JAN 1st ### ... -->

>[!NOTE]
>
>各規制の法的背景について詳しくは、[ サポートされるプライバシー規制 ](../regulations/overview.md) の概要を参照してください。

それぞれの規制タイプのジョブは、別々に追跡されます。規制タイプを切り替えるには、「**[!UICONTROL Regulation Type]**」ドロップダウンメニューを選択し、リストから目的の規制を選択します。

![ 規制タイプのドロップダウンを含むPrivacy Service コンソール。](../images/user-guide/regulation.png)

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

[!UICONTROL Job Requests] ワークスペースには、組織の最近のジョブリクエストに関する詳細がリストされます。 詳細には、リクエストタイプ、現在のステータス、期限、リクエスト者のメールなどが含まれます。 一度に 100 件のレコードのセットが読み込まれます。 デフォルトでは、最近作成されたジョブが上部に表示され、下にスクロールして参照すると、より多くのレコードセットが読み込まれます。

>[!NOTE]
>
>以前作成したジョブのデータには、完了日から 30 日間のみアクセスできます。

リストをフィルタリングするには、[!UICONTROL Job Requests] ールタイトルの下にある検索バーにキーワードを入力します。 リストは、入力に応じて自動的にフィルタリングをおこない、検索語に一致する値を含んだリクエストを表示します。検索フィールドは、プライバシージョブ ID を、UI で現在レンダリングされている/読み込まれているジョブに一致させる「クイック」検索を実行します。 送信されたすべてのジョブを包括的に検索するわけではありません。 代わりに、読み込まれた結果に適用されるフィルターになります。 Privacy Service API を使用して [ 特定の規制、日付範囲または単一のジョブに基づいてジョブを返す ](../api/privacy-jobs.md#list) ことができます。

>[!TIP]
>
>過去 30 日間のレコードを UI に読み込むには、テーブルを下にスクロールして、さらにレコードのバッチを読み込む必要があります。

![ 検索フィールドがハイライト表示されたプライバシーコンソールジョブリクエストセクション ](../images/user-guide/job-search.png)

または、検索ボタンを使用して、特定の日付範囲にまたがるプライバシージョブクエリを実行します。 このアクションは、指定された期間内に組織によって送信されたすべてのプライバシージョブを返します。 **[!UICONTROL Requested on]** ドロップダウンメニューを選択して、クエリの開始日と終了日を選択します。 使用可能なオプションには、[!UICONTROL Today]、[!UICONTROL Last 7 Days]、[!UICONTROL Last 2 Weeks]、[!UICONTROL Last 30 Days] または [!UICONTROL Custom] があります。 [!UICONTROL Requested on] オプションと共に使用した場合、検索機能には、選択した日付範囲の間に送信されたジョブリクエストのみが表示されます。

![ 検索フィールド、「リクエスト対象」ドロップダウンメニュー、「検索」ボタンがハイライト表示された「ジョブリクエスト」セクション ](../images/user-guide/requested-on-dropdown-menu.png)

特定のジョブリクエストの詳細を表示するには、リストからリクエストのジョブ ID を選択して、**[!UICONTROL Job Details]** のページを開きます。

![GDPR UI のジョブ詳細](../images/user-guide/job-details.png)

このダイアログには、各 [!DNL Experience Cloud] ソリューションに関するステータス情報と、ジョブ全体に対する現在の状態が表示されます。 プライバシージョブが非同期の場合は、各ソリューションの最新の通信日時（GMT）がページに表示されます。これは、リクエストの処理に他のソリューションより多くの時間が必要な場合があるからです。

ソリューションから追加のデータが提供された場合は、このダイアログで表示できます。このデータは、個々の製品行を選択して表示できます。

完全なジョブデータを CSV ファイルとしてダウンロードするには、ダイアログの右上にある「**[!UICONTROL Export to CSV]**」を選択します。

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

リクエストビルダーの使用を開始するには、画面の右側にあるステータスレポートウィジェットの下の **[!UICONTROL Create Request]** を選択します。

![ リクエストを作成を選択 ](../images/user-guide/create-request.png)

**[!UICONTROL Create Request]** ダイアログが開き、現在選択されている規制タイプのプライバシージョブリクエストを送信するための使用可能なオプションが表示されます。

![](../images/user-guide/request-builder.png){width=500}

リクエストの **[!UICONTROL Job Type]** （「削除」または「アクセス」）と 1 つ以上の使用可能な製品をリストから選択します。

Privacy Serviceでは、個人データに対して、[!UICONTROL Access] （読み取り）および/または [!UICONTROL Delete] の 2 種類のジョブリクエストをサポートしています。 お問い合わせの対象に関連する製品に保持されているすべての情報を受け取るリクエストを送信するか、お問い合わせの対象に関連するすべての情報を削除するリクエストを送信できます。

![](../images/user-guide/type-and-products.png){width=500}

「**[!UICONTROL Namespace type]**」で、[!DNL Privacy Service] に送信する顧客 ID に適した名前空間タイプを選択します。

![](../images/user-guide/namespace-type.png){width=500}

標準の名前空間タイプを使用する場合、ドロップダウンメニュー（メール、ECID、AAID）から名前空間を選択し、右側のテキストボックスに ID 値を入力します。ID ごとに **\&lt;enter>** キーを押して、リストに追加します。

![](../images/user-guide/standard-namespace.png){width=500}

カスタム名前空間タイプを使用する場合、以下の ID 値を指定する前に、名前空間を手動で入力する必要があります。

![](../images/user-guide/custom-namespace.png){width=500}

終了したら「**[!UICONTROL Create]**」を選択します。

![](../images/user-guide/request-builder-create.png){width=500}

ダイアログが閉じ、新しいジョブ（複数の場合あり）が現在の処理ステータスと共にジョブリクエストウィジェットにリスト表示されます。

### JSON ファイルのアップロード {#json}

処理するデータサブジェクトごとに複数の ID タイプを使用するリクエストなど、より複雑なリクエストを作成する場合は、JSON ファイルをアップロードしてリクエストを作成できます。

画面の右側にあるステータスレポートウィジェットの下にある、**[!UICONTROL Create Request]** の横の矢印を選択します。 表示されるオプションのリストから、「**[!UICONTROL Upload JSON]**」を選択します。

![リクエスト作成オプション](../images/user-guide/create-options.png)

**[!UICONTROL Upload JSON]** ダイアログが表示され、JSON ファイルをドラッグ&amp;ドロップするためのウィンドウが表示されます。

![](../images/user-guide/upload-json.png){width=500}

アップロードする JSON ファイルがない場合は、「**[!UICONTROL Download Adobe-GDPR-Request.json]**」を選択して、データ主体から収集した値に従って入力できるテンプレートをダウンロードします。


![](../images/user-guide/privacy-template.png){width=500}


コンピューター上で JSON ファイルを探し、ダイアログウィンドウにドラッグします。アップロードが正常に完了すると、ダイアログにファイル名が表示されます。必要に応じて、引き続き JSON ファイルをダイアログにドラッグ＆ドロップして追加できます。

終了したら「**[!UICONTROL Create]**」を選択します。 ダイアログが閉じ、新しいジョブ（複数の場合あり）が現在の処理ステータスと共にジョブリクエストウィジェットにリスト表示されます。

### 次の手順

このドキュメントでは、[!DNL Privacy Service] UI を使用してプライバシージョブを作成し、ジョブの詳細を表示し、処理ステータスを監視して、完了後に結果をダウンロードする方法について説明しました。

[!DNL Privacy Service] API を使用してこれらの操作をプログラムで実行する手順については、[API ガイド ](../api/overview.md) を参照してください。
