---
keywords: Experience Platform；ホーム；人気のあるトピック；取り込み；バッチデータの取り込み；チュートリアル；バッチ取り込み；チュートリアル；ui ガイド；
solution: Experience Platform
title: データの取り込みExperience Platform
topic-legacy: tutorial
type: Tutorial
description: Adobe Experience Platformでは、Parquet ファイルの形式のバッチファイル、または既知の Experience Data Model(XDM) スキーマに準拠するデータを、データを簡単にバッチファイルとして読み込むことができます。
exl-id: a4a7358d-b117-4d81-8cb0-3dbbfeccdcbd
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '1322'
ht-degree: 50%

---

# Adobe Experience Platform へのデータの取得

Adobe Experience Platformでは、データをバッチファイルとして [!DNL Platform] に簡単にインポートできます。 取り込まれるデータの例としては、CRM システムのフラットファイルのプロファイルデータ（Parquet ファイルなど）や、スキーマレジストリの既知の [!DNL Experience Data Model](XDM) スキーマに適合するデータなどがあります。

## はじめに

このチュートリアルを完了するには、[!DNL Experience Platform] にアクセスできる必要があります。 [!DNL Experience Platform] の IMS 組織にアクセスできない場合は、先に進む前にシステム管理者にお問い合わせください。

データ取得 API を使用してデータを取得する場合は、まず『[バッチ取得開発者ガイド](../batch-ingestion/api-overview.md)』をお読みください。

## データセットワークスペース

[!DNL Experience Platform] 内の「データセット」ワークスペースを使用すると、IMS 組織が作成したすべてのデータセットを表示および管理し、新しいデータセットを作成できます。

左側のナビゲーションで「**[!UICONTROL データセット]**」をクリックして、「データセット」ワークスペースを表示します。「データセット」ワークスペースには、名前、作成日時、ソース、スキーマ、最終バッチステータスを示す列、および最後にデータセットが更新された日時を含むデータセットのリストが含まれています。

>[!NOTE]
>
>検索バーの横にあるフィルターアイコンをクリックして、フィルタリング機能を使用し、[!DNL Profile] に対して有効なデータセットのみを表示します。

![すべてのデータセットの表示](../images/tutorials/ingest-batch-data/datasets-overview.png)

## データセットの作成

データセットを作成するには、「データセット」ワークスペースの右上隅にある「**[!UICONTROL データセットを作成]**」をクリックします。

![](../images/tutorials/ingest-batch-data/click-create-datasets.png)

**[!UICONTROL データセットを作成]** 画面で、「[!UICONTROL  スキーマからデータセットを作成 ]」と「[!UICONTROL CSV ファイルからデータセットを作成 ]」のどちらに設定するかを選択します。

このチュートリアルでは、スキーマを使用してデータセットを作成します。続行するには、「**[!UICONTROL スキーマからデータセットを作成]**」をクリックします。

![データソースの選択](../images/tutorials/ingest-batch-data/create-dataset.png)

## データセットスキーマの選択

**[!UICONTROL スキーマを選択]**&#x200B;画面で、使用するスキーマの横にあるラジオボタンをクリックしてスキーマを選択します。このチュートリアルでは、データセットは Loyalty Members スキーマを使用して作成します。検索バーを使用してスキーマをフィルターすると、探している正確なスキーマを見つけられます。

使用するスキーマの横のラジオボタンを選択したら、「**[!UICONTROL 次へ]**」をクリックします。

![スキーマの選択](../images/tutorials/ingest-batch-data/select-schema.png)

## データセットの設定

**[!UICONTROL データセットを設定]** 画面で、データセットに名前を付け、データセットの説明も入力します。

**データセット名に関する注意事項：**

- 後でライブラリ内で簡単に見つけられるように、データセット名は短く、わかりやすい名前にする必要があります。
- データセット名は一意である必要があります。つまり、今後再利用されないように十分な固有の名前を付ける必要があります。
- ベストプラクティスは、説明フィールドを使用して、データセットに関する追加情報を提供することです。これは、今後、他のユーザーがデータセットを区別する際に役立つ可能性があるためです。

データセットに名前と説明が付いたら、「**[!UICONTROL 完了]**」をクリックします。

![データセットの設定](../images/tutorials/ingest-batch-data/configure-dataset.png)

## データセットアクティビティ

空のデータセットが作成され、データセットワークスペースの「**[!UICONTROL データセットアクティビティ]**」タブに戻りました。ワークスペースの左上隅にデータセットの名前と、「バッチが追加されていません」という通知が表示されます。このデータセットにバッチをまだ追加していないので、これは期待通りです。

「データセット」ワークスペースの右側に、「**[!UICONTROL 情報]**」タブが表示され、データセット ID、名前、説明、テーブル名、スキーマ、ストリーミング、ソースなど、新しいデータセットに関連する情報が含まれます。 「情報」タブには、データセットの作成日時と最終変更日に関する情報も含まれます。

また、「情報」タブには、[!DNL Real-time Customer Profile] で使用するデータセットを有効にするための **[!UICONTROL プロファイル]** の切り替えもあります。 この切り替えと [!DNL Real-time Customer Profile] の使い方については、次の節で詳しく説明します。

![データセットアクティビティ](../images/tutorials/ingest-batch-data/sample-dataset.png)

## [!DNL Real-time Customer Profile] のデータセットを有効にする

データセットは、データを [!DNL Experience Platform] に取り込むために使用され、最終的には、そのデータを使用して個人を識別し、複数のソースから得られる情報を組み合わせます。 この情報をつなぎ合わせたものを [!DNL Real-Time Customer Profile] と呼びます。 [!DNL Platform] が [!DNL Real-Time Profile] に含める必要のある情報を知るために、**[!UICONTROL プロファイル]** 切り替えを使用してデータセットを含めるようにマークできます。

デフォルトでは、この切り替えはオフになっています。[!DNL Profile] をオンに切り替えると、データセットに取り込まれたすべてのデータが、個人を特定し、[!DNL Real-Time Profile] をつなぎ合わせるのに使用されます。

[!DNL Real-time Customer Profile] と ID の使用について詳しくは、[ID サービス ](../../identity-service/home.md) のドキュメントを参照してください。

[!DNL Real-time Customer Profile] のデータセットを有効にするには、「**[!UICONTROL 情報]**」タブの **[!UICONTROL プロファイル]** 切り替えをクリックします。

![プロファイル切り替え](../images/tutorials/ingest-batch-data/dataset-profile-toggle.png)

[!DNL Real-time Customer Profile] のデータセットを有効にするかどうかを確認するダイアログが表示されます。

![プロファイル有効ダイアログ](../images/tutorials/ingest-batch-data/enable-dataset-for-profile.png)

「**[!UICONTROL 有効]**」をクリックすると、切り替えが青に変わり、オンになっていることを示します。

![プロファイルで有効](../images/tutorials/ingest-batch-data/profile-enabled-dataset.png)

## データセットへのデータの追加

データは様々な方法でデータセットに追加できます。[!DNL Data Ingestion] API または [!DNL Unifi] や [!DNL Informatica] などの ETL パートナーを使用するように選択できます。 このチュートリアルでは、UI 内の「**[!UICONTROL データの追加]**」タブを使用してデータセットにデータを追加します。

データセットへのデータの追加を開始するには、「**[!UICONTROL データの追加]**」タブをクリックします 。ファイルをドラッグ&amp;ドロップしたり、追加するファイルをコンピューターで参照したりできるようになりました。

>[!NOTE]
>
>Platform では、データ取り込み用に 2 種類のファイル (Parquet、JSON) がサポートされています。 一度に 5 個までのファイルを追加でき、各ファイルの最大ファイルサイズは 10GB です。

![「データを追加」タブ](../images/tutorials/ingest-batch-data/drag-and-drop.png)

## ファイルのアップロード

アップロードする Parquet または JSON ファイルをドラッグ&amp;ドロップ（または参照して選択）すると、すぐに [!DNL Platform] でファイルの処理が開始され、**[!UICONTROL 「データを追加]** 」タブに **[!UICONTROL アップロード]** ダイアログが表示されます。

![アップロードダイアログ](../images/tutorials/ingest-batch-data/uploading-file.png)

## データセット指標

ファイルのアップロードが完了すると、「**[!UICONTROL データセットのアクティビティ]** 」タブに「バッチが追加されていません」と表示されることはなくなります。代わりに、「**[!UICONTROL データセットアクティビティ]**」タブにデータセット指標が表示されるようになりました。 バッチがまだ読み込まれていないので、すべての指標はこの段階で「0」と表示されます。

タブの下部には、「[データセットへのデータ追加](#add-data-to-dataset)」処理で取得されたデータの&#x200B;**[!UICONTROL バッチ ID]** を示す追加リストが表示されます。また、取得された日付、取得されたレコード数、現在のバッチの状態など、バッチに関する情報も含まれます。

![データセット指標](../images/tutorials/ingest-batch-data/batch-id.png)

## バッチの詳細

**[!UICONTROL バッチの概要]**&#x200B;でバッチに関する追加の詳細を表示するには「**[!UICONTROL バッチ ID]**」をクリックします。バッチの読み込みが完了すると、バッチに関する情報が更新され、取得されたレコード数とファイルサイズが表示されます。 ステータスも「成功」または「失敗」に変わります。 バッチが失敗した場合は、取得中に「**[!UICONTROL エラーコード]**」セクションに、エラーに関する詳細が含まれます。

バッチ取得に関する詳細とよくある質問については、『[バッチ取得のトラブルシューティングガイド](../batch-ingestion/troubleshooting.md)』を参照してください。

**[!UICONTROL データセットアクティビティ]**&#x200B;画面に戻るには 、階層リンクでデータセットの名前（**[!UICONTROL Loyalty Details]**）をクリックします。

![バッチの概要](../images/tutorials/ingest-batch-data/batch-details.png)

## データセットのプレビュー

データセットの準備が整うと、「**[!UICONTROL データセットアクティビティ]**」タブの上部に「**[!UICONTROL データセットのプレビュー]**」オプションが表示されます。

「**[!UICONTROL プレビューデータセット]**」をクリックすると、データセット内のサンプルデータを示すダイアログが開きます。データセットがスキーマを使用して作成された場合は、データセットスキーマの詳細がプレビューの左側に表示されます。矢印を使用してスキーマを展開し、構造を確認できます。データセット内の各列見出しは、プレビューセット内の 1 つのフィールドを表します。

![データセットの詳細](../images/tutorials/ingest-batch-data/dataset-preview.png)

## 次の手順とその他のリソース

データセットを作成し、[!DNL Experience Platform] にデータを正常に取り込んだら、これらの手順を繰り返して新しいデータセットを作成するか、既存のデータセットにデータを取り込むことができます。

バッチ取り込みの詳細については、「[ バッチ取り込みの概要 ](../batch-ingestion/overview.md)」を読み、以下のビデオを見て学習を補完してください。

>[!WARNING]
>
>次のビデオに示す [!DNL Platform] UI は古くなっています。最新の UI スクリーンショットと機能については、上記のドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/27269?quality=12&learn=on)
