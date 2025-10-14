---
keywords: Experience Platform；ホーム；人気のトピック；取り込み；バッチデータの取り込み；チュートリアル；バッチ取り込み；チュートリアル；ui ガイド；
solution: Experience Platform
title: Experience Platformへのデータの取り込み
type: Tutorial
description: Adobe Experience Platformでは、データを Parquet ファイルまたは既知の Experience Data Model （XDM）スキーマに準拠するデータの形式で、バッチファイルとして簡単にインポートできます。
exl-id: a4a7358d-b117-4d81-8cb0-3dbbfeccdcbd
source-git-commit: 31631b03b6ff1e50e55c01e948fae5c29fd618dd
workflow-type: tm+mt
source-wordcount: '1322'
ht-degree: 51%

---

# Adobe Experience Platform へのデータの取得

Adobe Experience Platformでは、データをバッチファイルとして [!DNL Experience Platform] に簡単にインポートできます。 取り込まれるデータの例としては、CRM システム内のフラットファイル（Parquet ファイルなど）のプロファイルデータや、スキーマレジストリ内の既知の [!DNL Experience Data Model] （XDM）スキーマに準拠するデータなどがあります。

## はじめに

このチュートリアルを完了するには、[!DNL Experience Platform] へのアクセス権が必要です。 [!DNL Experience Platform] の組織にアクセスする権限がない場合は、続行する前にシステム管理者に問い合わせてください。

データ取得 API を使用してデータを取得する場合は、まず『[バッチ取得開発者ガイド](../batch-ingestion/api-overview.md)』をお読みください。

## データセットワークスペース

[!DNL Experience Platform] 内のデータセット ワークスペースを使用すると、組織が作成したすべてのデータセットを表示および管理したり、新しいデータセットを作成したりできます。

左側のナビゲーションで「**[!UICONTROL データセット]**」をクリックして、「データセット」ワークスペースを表示します。データセットワークスペースには、データセットのリストが含まれています。このリストには、名前、作成済み（日時）、ソース、スキーマ、前回のバッチステータスおよびデータセットが最後に更新された日時を示す列が含まれます。

>[!NOTE]
>
>検索バーの横にあるフィルターアイコンをクリックして、フィルタリング機能を使用し、[!DNL Profile] ークフローが有効になっているデータセットのみを表示します。

![すべてのデータセットの表示](../images/tutorials/ingest-batch-data/datasets-overview.png)

## データセットの作成

データセットを作成するには、「データセット」ワークスペースの右上隅にある「**[!UICONTROL データセットを作成]**」をクリックします。

![](../images/tutorials/ingest-batch-data/click-create-datasets.png)

**[!UICONTROL データセットを作成]** 画面で、「[!UICONTROL &#x200B; スキーマからデータセットを作成 &#x200B;]」または「[!UICONTROL CSV ファイルからデータセットを作成 &#x200B;]」のどちらを実行するかを選択します。

このチュートリアルでは、スキーマを使用してデータセットを作成します。続行するには、「**[!UICONTROL スキーマからデータセットを作成]**」をクリックします。

![データソースの選択](../images/tutorials/ingest-batch-data/create-dataset.png)

## データセットスキーマの選択

**[!UICONTROL スキーマを選択]**&#x200B;画面で、使用するスキーマの横にあるラジオボタンをクリックしてスキーマを選択します。このチュートリアルでは、データセットは Loyalty Members スキーマを使用して作成します。検索バーを使用してスキーマをフィルターすると、探している正確なスキーマを見つけられます。

使用するスキーマの横のラジオボタンを選択したら、「**[!UICONTROL 次へ]**」をクリックします。

![スキーマの選択](../images/tutorials/ingest-batch-data/select-schema.png)

## データセットの設定

**[!UICONTROL データセットを設定]** 画面で、データセットに名前を付ける必要があり、データセットの説明も提供する場合があります。

**データセット名に関する注意事項：**

- 後でライブラリ内で簡単に見つけられるように、データセット名は短く、わかりやすい名前にする必要があります。
- データセット名は一意である必要があります。つまり、今後再利用されないように十分な固有の名前を付ける必要があります。
- ベストプラクティスは、説明フィールドを使用して、データセットに関する追加情報を提供することです。これは、今後、他のユーザーがデータセットを区別する際に役立つ可能性があるためです。

データセットに名前と説明が付いたら、「**[!UICONTROL 完了]**」をクリックします。

![データセットの設定](../images/tutorials/ingest-batch-data/configure-dataset.png)

## データセットアクティビティ

空のデータセットが作成され、データセットワークスペースの「**[!UICONTROL データセットアクティビティ]**」タブに戻りました。ワークスペースの左上隅にデータセットの名前と、「バッチが追加されていません」という通知が表示されます。このデータセットにバッチをまだ追加していないので、これは期待通りです。

データセットワークスペースの右側には、データセット ID、名前、説明、テーブル名、スキーマ、ストリーミング、ソースなど、新しいデータセットに関連する情報を含む **[!UICONTROL 情報]** タブが表示されます。 「情報」タブには、データセットが作成された日時と最終変更日に関する情報も含まれています。

また、「情報」タブには、データセットを [!DNL Real-Time Customer Profile] で使用できるようにするための **[!UICONTROL プロファイル]** 切り替えスイッチがあります。 この切り替えと [!DNL Real-Time Customer Profile] の使用について詳しくは、次の節で説明します。

![データセットアクティビティ](../images/tutorials/ingest-batch-data/sample-dataset.png)

## [!DNL Real-Time Customer Profile] のデータセットを有効にする {#enable-for-profile}

データセットは、データを [!DNL Experience Platform] に取り込むために使用され、そのデータは、最終的に個人を識別し、複数のソースからの情報を結び付けるために使用されます。 結び付けられた情報は [!DNL Real-Time Customer Profile] と呼ばれます。 [!DNL Real-Time Profile] ージに含める必要がある情報を [!DNL Experience Platform] ーザーが把握できるように、**[!UICONTROL プロファイル]** 切り替えスイッチを使用して、データセットを含めるようにマークできます。

デフォルトでは、この切り替えはオフになっています。[!DNL Profile] をオンに切り替えた場合、データセットに取り込まれたすべてのデータは、個人を識別して [!DNL Real-Time Profile] ータを結び付けるのに役立つように使用されます。

ID の [!DNL Real-Time Customer Profile] 用と操作について詳しくは、[ID サービス &#x200B;](../../identity-service/home.md) ドキュメントを参照してください。

データセットの [!DNL Real-Time Customer Profile] を有効にするには、「**[!UICONTROL 情報]**」タブの **[!UICONTROL プロファイル]** 切り替えスイッチをクリックします。

![プロファイル切り替え](../images/tutorials/ingest-batch-data/dataset-profile-toggle.png)

[!DNL Real-Time Customer Profile] のデータセットを有効にするかどうかを確認するダイアログが表示されます。

![プロファイル有効ダイアログ](../images/tutorials/ingest-batch-data/enable-dataset-for-profile.png)

「**[!UICONTROL 有効]**」をクリックすると、切り替えが青に変わり、オンになっていることを示します。

![プロファイルで有効](../images/tutorials/ingest-batch-data/profile-enabled-dataset.png)

## データセットへのデータの追加

データは様々な方法でデータセットに追加できます。[!DNL Data Ingestion] API を使用するか、[!DNL Unifi] や [!DNL Informatica] などの ETL パートナーを使用するかを選択できます。 このチュートリアルでは、UI 内の「**[!UICONTROL データの追加]**」タブを使用してデータセットにデータを追加します。

データセットへのデータの追加を開始するには、「**[!UICONTROL データの追加]**」タブをクリックします 。ファイルをドラッグ&amp;ドロップしたり、追加するファイルをコンピューターで参照したりできるようになりました。

>[!NOTE]
>
>Experience Platformでは、データ取得用に Parquet と JSON の 2 種類のファイルをサポートしています。 一度に 5 個までのファイルを追加でき、各ファイルの最大ファイルサイズは 1 GB です。

![「データを追加」タブ](../images/tutorials/ingest-batch-data/drag-and-drop.png)

## ファイルのアップロード {#upload-file}

アップロードする Parquet または JSON ファイルをドラッグ&amp;ドロップ（または参照して選択）す [!DNL Experience Platform] と、ファイルの処理がすぐに開始され、ファイルのアップロードの進行状況を示す **[!UICONTROL データを追加]** タブに **[!UICONTROL アップロード]** ダイアログが表示されます。

![アップロードダイアログ](../images/tutorials/ingest-batch-data/uploading-file.png)

## データセット指標

ファイルのアップロードが完了すると、「**[!UICONTROL データセットのアクティビティ]** 」タブに「バッチが追加されていません」と表示されることはなくなります。代わりに、「**[!UICONTROL データセットアクティビティ]**」タブにデータセット指標が表示されるようになりました。 バッチがまだ読み込まれていないので、すべての指標はこの段階で「0」と表示されます。

タブの下部には、「[データセットへのデータ追加](#add-data-to-dataset)」処理で取得されたデータの&#x200B;**[!UICONTROL バッチ ID]** を示す追加リストが表示されます。取り込まれた日付、取り込まれたレコード数、現在のバッチステータスなど、バッチに関連する情報も含まれます。

![データセット指標](../images/tutorials/ingest-batch-data/batch-id.png)

## バッチの詳細

**[!UICONTROL バッチの概要]**&#x200B;でバッチに関する追加の詳細を表示するには「**[!UICONTROL バッチ ID]**」をクリックします。バッチの読み込みが完了すると、バッチに関する情報が更新され、取り込まれたレコード数とファイルサイズが表示されます。 ステータスも「成功」または「失敗」に変わります。 バッチが失敗した場合は、取得中に「**[!UICONTROL エラーコード]**」セクションに、エラーに関する詳細が含まれます。

バッチ取得に関する詳細とよくある質問については、『[バッチ取得のトラブルシューティングガイド](../batch-ingestion/troubleshooting.md)』を参照してください。

**[!UICONTROL データセットアクティビティ]**&#x200B;画面に戻るには 、階層リンクでデータセットの名前（**[!UICONTROL Loyalty Details]**）をクリックします。

![バッチの概要](../images/tutorials/ingest-batch-data/batch-details.png)

## データセットのプレビュー

データセットの準備が整うと、「**[!UICONTROL データセットアクティビティ]**」タブの上部に「**[!UICONTROL データセットのプレビュー]**」オプションが表示されます。

「**[!UICONTROL プレビューデータセット]**」をクリックすると、データセット内のサンプルデータを示すダイアログが開きます。データセットがスキーマを使用して作成された場合は、データセットスキーマの詳細がプレビューの左側に表示されます。矢印を使用してスキーマを展開し、構造を確認できます。データセット内の各列見出しは、プレビューセット内の 1 つのフィールドを表します。

![データセットの詳細](../images/tutorials/ingest-batch-data/dataset-preview.png)

## 次の手順とその他のリソース

データセットを作成し、データを [!DNL Experience Platform] に正常に取り込んだので、これらの手順を繰り返して、新しいデータセットを作成したり、既存のデータセットにさらにデータを取り込んだりできます。

バッチ取り込みについて詳しくは、[&#x200B; バッチ取り込みの概要 &#x200B;](../batch-ingestion/overview.md) を参照し、以下のビデオを視聴して知識を補ってください。

>[!WARNING]
>
>次のビデオに示す [!DNL Experience Platform] UI は旧式のものです。最新の UI のスクリーンショットと機能については、上記のドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/34394?quality=12&learn=on&captions=jpn)
