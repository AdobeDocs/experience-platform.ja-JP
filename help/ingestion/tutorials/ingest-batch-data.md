---
keywords: Experience Platform；ホーム；人気の高いトピック；取り込み；バッチデータの取り込み；チュートリアル；バッチ取り込み；チュートリアル；ui ガイド；
solution: Experience Platform
title: データの取り込みExperience Platform
type: Tutorial
description: Adobe Experience Platformでは、Parquet ファイルの形式のバッチファイル、または既知の Experience Data Model(XDM) スキーマに準拠するデータを、簡単にバッチファイルとして読み込むことができます。
exl-id: a4a7358d-b117-4d81-8cb0-3dbbfeccdcbd
source-git-commit: e802932dea38ebbca8de012a4d285eab691231be
workflow-type: tm+mt
source-wordcount: '1322'
ht-degree: 51%

---

# Adobe Experience Platform へのデータの取得

Adobe Experience Platformでは、次の場所に簡単にデータを読み込むことができます。 [!DNL Platform] をバッチファイルとして使用します。 取り込むデータの例としては、CRM システムのフラットファイルのプロファイルデータ（Parquet ファイルなど）や、既知の [!DNL Experience Data Model] (XDM) スキーマをスキーマレジストリに追加します。

## はじめに

このチュートリアルを完了するには、 [!DNL Experience Platform]. の IMS 組織へのアクセス権がない場合 [!DNL Experience Platform]続行する前に、システム管理者にお問い合わせください。

データ取得 API を使用してデータを取得する場合は、まず『[バッチ取得開発者ガイド](../batch-ingestion/api-overview.md)』をお読みください。

## データセットワークスペース

内の「データセット」ワークスペース [!DNL Experience Platform] では、IMS 組織が作成したすべてのデータセットの表示と管理、および新しいデータセットの作成をおこなうことができます。

左側のナビゲーションで「**[!UICONTROL データセット]**」をクリックして、「データセット」ワークスペースを表示します。「データセット」ワークスペースには、名前、作成日時、ソース、スキーマ、最終バッチステータスを示す列、および最終更新日時を含むデータセットのリストが含まれています。

>[!NOTE]
>
>検索バーの横にあるフィルターアイコンをクリックして、フィルタリング機能を使用し、有効なデータセットのみを表示します。 [!DNL Profile].

![すべてのデータセットの表示](../images/tutorials/ingest-batch-data/datasets-overview.png)

## データセットの作成

データセットを作成するには、「データセット」ワークスペースの右上隅にある「**[!UICONTROL データセットを作成]**」をクリックします。

![](../images/tutorials/ingest-batch-data/click-create-datasets.png)

の **[!UICONTROL データセットを作成]** 画面で、「[!UICONTROL スキーマからデータセットを作成]&quot;または&quot;[!UICONTROL CSV ファイルからデータセットを作成]&quot;.

このチュートリアルでは、スキーマを使用してデータセットを作成します。続行するには、「**[!UICONTROL スキーマからデータセットを作成]**」をクリックします。

![データソースの選択](../images/tutorials/ingest-batch-data/create-dataset.png)

## データセットスキーマの選択

**[!UICONTROL スキーマを選択]**&#x200B;画面で、使用するスキーマの横にあるラジオボタンをクリックしてスキーマを選択します。このチュートリアルでは、データセットは Loyalty Members スキーマを使用して作成します。検索バーを使用してスキーマをフィルターすると、探している正確なスキーマを見つけられます。

使用するスキーマの横のラジオボタンを選択したら、「**[!UICONTROL 次へ]**」をクリックします。

![スキーマの選択](../images/tutorials/ingest-batch-data/select-schema.png)

## データセットの設定

の **[!UICONTROL データセットを設定]** 画面が表示されたら、データセットに名前を付け、データセットの説明も入力できます。

**データセット名に関する注意事項：**

- 後でライブラリ内で簡単に見つけられるように、データセット名は短く、わかりやすい名前にする必要があります。
- データセット名は一意である必要があります。つまり、今後再利用されないように十分な固有の名前を付ける必要があります。
- ベストプラクティスは、説明フィールドを使用して、データセットに関する追加情報を提供することです。これは、今後、他のユーザーがデータセットを区別する際に役立つ可能性があるためです。

データセットに名前と説明が付いたら、「**[!UICONTROL 完了]**」をクリックします。

![データセットの設定](../images/tutorials/ingest-batch-data/configure-dataset.png)

## データセットアクティビティ

空のデータセットが作成され、データセットワークスペースの「**[!UICONTROL データセットアクティビティ]**」タブに戻りました。ワークスペースの左上隅にデータセットの名前と、「バッチが追加されていません」という通知が表示されます。このデータセットにバッチをまだ追加していないので、これは期待通りです。

「データセット」ワークスペースの右側には、 **[!UICONTROL 情報]** タブには、新しいデータセットに関連する情報（データセット ID、名前、説明、テーブル名、スキーマ、ストリーミング、ソースなど）が含まれます。 また、「情報」タブには、データセットの作成日時と最終変更日に関する情報も含まれます。

また、「情報」タブには、  **[!UICONTROL プロファイル]** でデータセットを使用するための切り替え [!DNL Real-Time Customer Profile]. この切り替えを使用し、 [!DNL Real-Time Customer Profile]を参照してください。詳しくは、次の節を参照してください。

![データセットアクティビティ](../images/tutorials/ingest-batch-data/sample-dataset.png)

## のデータセットを有効にする [!DNL Real-Time Customer Profile]

データセットは、にデータを取り込むために使用されます [!DNL Experience Platform]に含まれるデータは、最終的には個人を識別し、複数のソースから得られる情報を組み合わせるために使用されます。 この情報を組み合わせたものを、 [!DNL Real-Time Customer Profile]. 次のために [!DNL Platform] どの情報を含めるべきかを知る [!DNL Real-Time Profile]を使用すると、データセットを **[!UICONTROL プロファイル]** 切り替え

デフォルトでは、この切り替えはオフになっています。オンに切り替える場合 [!DNL Profile]の場合、データセットに取り込まれるすべてのデータは、個人を特定し、それらを結合するのに使用されます [!DNL Real-Time Profile].

詳しくは、以下を参照してください。 [!DNL Real-Time Customer Profile] ID の使用については、 [ID サービス](../../identity-service/home.md) ドキュメント。

のデータセットを有効にするには [!DNL Real-Time Customer Profile]、 **[!UICONTROL プロファイル]** 切り替え **[!UICONTROL 情報]** タブをクリックします。

![プロファイル切り替え](../images/tutorials/ingest-batch-data/dataset-profile-toggle.png)

のデータセットを有効にするかどうかを確認するダイアログが表示されます [!DNL Real-Time Customer Profile].

![プロファイル有効ダイアログ](../images/tutorials/ingest-batch-data/enable-dataset-for-profile.png)

「**[!UICONTROL 有効]**」をクリックすると、切り替えが青に変わり、オンになっていることを示します。

![プロファイルで有効](../images/tutorials/ingest-batch-data/profile-enabled-dataset.png)

## データセットへのデータの追加

データは様々な方法でデータセットに追加できます。以下を使用することもできます。 [!DNL Data Ingestion] API または ETL パートナー（など） [!DNL Unifi] または [!DNL Informatica]. このチュートリアルでは、UI 内の「**[!UICONTROL データの追加]**」タブを使用してデータセットにデータを追加します。

データセットへのデータの追加を開始するには、「**[!UICONTROL データの追加]**」タブをクリックします 。ファイルをドラッグ&amp;ドロップしたり、追加するファイルをコンピューターで参照したりできるようになりました。

>[!NOTE]
>
>Platform では、データ取り込み用に 2 種類のファイル (Parquet、JSON) がサポートされています。 一度に 5 個までのファイルを追加でき、各ファイルの最大ファイルサイズは 1GB です。

![「データを追加」タブ](../images/tutorials/ingest-batch-data/drag-and-drop.png)

## ファイルのアップロード

アップロードする Parquet または JSON ファイルをドラッグ&amp;ドロップ（または参照して選択）したら、次の手順を実行します。 [!DNL Platform] が直ちにファイルの処理を開始し、 **[!UICONTROL アップロード中]** ダイアログが **[!UICONTROL データを追加]** タブに、ファイルのアップロードの進行状況が表示されます。

![アップロードダイアログ](../images/tutorials/ingest-batch-data/uploading-file.png)

## データセット指標

ファイルのアップロードが完了すると、「**[!UICONTROL データセットのアクティビティ]** 」タブに「バッチが追加されていません」と表示されることはなくなります。代わりに、 **[!UICONTROL データセットアクティビティ]** 「 」タブにデータセット指標が表示されるようになりました。 バッチがまだ読み込まれていないので、すべての指標はこの段階で「0」と表示されます。

タブの下部には、「[データセットへのデータ追加](#add-data-to-dataset)」処理で取得されたデータの&#x200B;**[!UICONTROL バッチ ID]** を示す追加リストが表示されます。また、取り込まれた日付、取り込まれたレコード数、現在のバッチの状態など、バッチに関する情報も含まれます。

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

これで、データセットが作成され、データが [!DNL Experience Platform]の場合は、これらの手順を繰り返して新しいデータセットを作成したり、既存のデータセットにさらにデータを取り込んだりできます。

バッチ取り込みの詳細については、 [バッチ取り込みの概要](../batch-ingestion/overview.md) 以下のビデオを見て、学習を補完します。

>[!WARNING]
>
>次のビデオに示す [!DNL Platform] UI は旧式のものです。最新の UI のスクリーンショットと機能については、上記のドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/27269?quality=12&learn=on)
