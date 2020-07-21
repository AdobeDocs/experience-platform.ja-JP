---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: データをAdobe Experience Platformに取り込む
topic: tutorial
translation-type: tm+mt
source-git-commit: bfbf2074a9dcadd809de043d62f7d2ddaa7c7b31
workflow-type: tm+mt
source-wordcount: '1277'
ht-degree: 1%

---


# データをAdobe Experience Platformに取り込む

Adobe Experience Platformを使用すると、データをバッチファイル [!DNL Platform] としてに簡単にインポートできます。 取り込むデータの例としては、CRMシステムのフラットファイル（パーケファイルなど）のプロファイルデータ、またはスキーマレジストリの既知の [!DNL Experience Data Model] (XDM)スキーマに適合するデータが挙げられる。

## はじめに

このチュートリアルを完了するには、にアクセスする必要があり [!DNL Experience Platform]ます。 でIMS組織にアクセスできない場合は、先に進む前にシステム管理者にお問い合わせ [!DNL Experience Platform]ください。

データ取り込みAPIを使用してデータを取り込む場合は、まず [バッチ取り込み開発ガイドを読んでください](../batch-ingestion/api-overview.md)。

## Datasetsワークスペース

The Datasets workspace within [!DNL Experience Platform] allows you to view and manage all of the datasets that your IMS organization has made, as well as create new ones.

左側のナビゲーションで「**[!UICONTROL データセット]**」をクリックして、「データセット」ワークスペースを表示します。The Datasets workspace contains a list of datasets, including columns showing _[!UICONTROL Name]_,_[!UICONTROL  Created]_ (date and time), _[!UICONTROL Source]_,_[!UICONTROL  Schema]_, and _[!UICONTROL Last Batch Status]_, as well as the date and time the dataset was_[!UICONTROL  Last Updated]_.

>[!NOTE]
>
>検索バーの横にあるフィルターアイコンをクリックして、フィルタリング機能を使用し、有効になっているデータセットのみを表示 [!DNL Profile]します。

![すべてのデータセットの表示](../images/tutorials/ingest-batch-data/datasets_workspace.png)

## データセットの作成

データセットを作成するには、Datasetsワークスペースの右上隅にある **[!UICONTROL 「データセットを作成]** 」をクリックします。

データセットを **[!UICONTROL 作成]** 画面で、「スキーマからデータセットを[!UICONTROL 作成]」を選択するか、「CSVファイルからデータセットを[!UICONTROL 作成]」を選択します。

このチュートリアルでは、スキーマを使用してデータセットを作成します。 「 **[!UICONTROL スキーマからデータセットを]** 作成」をクリックして続行します。

![データソースの選択](../images/tutorials/ingest-batch-data/create_dataset.png)

## データセットスキーマの選択

スキーマ **[!UICONTROL を選択画面で、使用するスキーマの横にあるラジオボタンをクリックして、スキーマを選択します]** 。 このチュートリアルでは、データセットはLoyality Membersスキーマを使用して作成されます。 検索バーを使用してスキーマをフィルタすると、探している正確なスキーマを見つけるのに役立ちます。

使用するスキーマの横にあるラジオボタンを選択したら、「 **[!UICONTROL 次へ]**」をクリックします。

![スキーマの選択](../images/tutorials/ingest-batch-data/select_schema.png)

## データセットの設定

データセットを **[!UICONTROL 設定画面では、データセットに]** 名前を付ける必要があり **[!UICONTROL 、]** 説明 **** (Description)も表示されます。

**データセット名に関する注意事項：**

- データセット名は短くてわかりやすい名前にし、後でライブラリ内で簡単に見つけられるようにしてください。
- データセット名は一意にする必要があります。つまり、将来再利用できなくなるように固有の名前を付ける必要もあります。
- 説明フィールドを使用して、データセットに関する追加情報を提供することをお勧めします。これは、今後、他のユーザーがデータセットを区別する際に役立つ場合があるためです。

データセットに名前と説明が付いたら、「 **[!UICONTROL 完了]**」をクリックします。

![データセットの設定](../images/tutorials/ingest-batch-data/configure_dataset.png)

## データセットアクティビティ

空のデータセットが作成され、Datasetsワークスペースの「 **[!UICONTROL データセットアクティビティ]** 」タブに戻りました。 ワークスペースの左上隅に、データセットの名前と、「バッチが追加されていません」という通知が表示されます。 これは、まだこのデータセットにバッチを追加していないので、予想されることです。

Workspaceの右側には、新しいデータセットに関連する情報を含む「 **[!UICONTROL 情報]** 」 _[!UICONTROL タブが表示されます。例えば、名前名、名前、説明、説明、、]_スキーマ名、説明、ストリーミング、元データセットなど____________です。 「情報」タブには、データセットがいつ_[!UICONTROL &#x200B;作成されたかと]_ 、 _[!UICONTROL 最終変更日に関する情報も表示されます]_。

また、「情報」タブには _[!UICONTROL プロファイル]_の切り替えが表示され、でデータセットを有効にする際に使用[!DNL Real-time Customer Profile]します。 この切り替えの使用[!DNL Real-time Customer Profile]と、の使用について詳しくは、以下の節で説明します。

![データセットアクティビティ](../images/tutorials/ingest-batch-data/dataset_activity.png)

## データセットの有効化 [!DNL Real-time Customer Profile]

データセットはデータをに取り込むために使用され [!DNL Experience Platform]、最終的にデータは個人を識別し、複数のソースから得られる情報を結合するために使用されます。 情報を繋ぎ合わせたものを「」と呼び [!DNL Real-Time Customer Profile]ます どの情報がに含まれ [!DNL Platform] るかを知るために、 [!DNL Real-Time Profile]プロファイル **** ・トグルを使用してデータセットを含めるようにマークできます。

デフォルトでは、この切り替えはオフです。 オンに切り替えると [!DNL Profile]、データセットに取り込まれたすべてのデータが、個人を特定し、それらをつなぎ合わせるのに役立ち [!DNL Real-Time Profile]ます。

IDの詳細およ [!DNL Real-time Customer Profile] び使用方法については、 [IDサービスのドキュメントを参照してください](../../identity-service/home.md) 。

データセットを有効にするに [!DNL Real-time Customer Profile]は、「 **[!UICONTROL 情報]** 」タブで **[!UICONTROL プロファイル]** の切り替えをクリックします。

![プロファイル切り替え](../images/tutorials/ingest-batch-data/enable_dataset_unified_profile.png)

データセットを有効にするかどうかを確認するダイアログが表示され [!DNL Real-time Customer Profile]ます。

![プロファイルダイアログを有効にする](../images/tutorials/ingest-batch-data/confirm_dataset_enable.png)

「 **[!UICONTROL 有効]** 」をクリックすると、切り替えが青に変わり、オンであることを示します。

![プロファイルが有効](../images/tutorials/ingest-batch-data/dataset_enabled.png)

## データセット追加へのデータ

データは様々な方法でデータセットに追加できます。 APIまたは [!DNL Data Ingestion] ETLパートナー（またはなど）を使用する [!DNL Unifi] ように選択でき [!DNL Informatica]ます。 このチュートリアルでは、UI内の「 **[!UICONTROL 追加Data]** 」タブを使用してデータセットにデータを追加します。

データセットへのデータの追加を開始するには、「 **[!UICONTROL 追加Data]** 」タブをクリックします。 これで、ファイルをドラッグ&amp;ドロップしたり、追加するファイルをコンピューターで参照したりできます。

>[!NOTE]
>
>[!DNL Platform] は、データ取り込み用に2種類のファイル、パーケット、JSONをサポートしています。 一度に最大5個のファイルを追加でき、各ファイルの最大ファイルサイズは10 GBです。

![「追加Data」タブ](../images/tutorials/ingest-batch-data/add_data.png)

## ファイルのアップロード

アップロードするパーケットまたはJSONファイルをドラッグ&amp;ドロップ（または参照&amp;選択）すると、すぐ [!DNL Platform] にファイルの処理が開始され、「 **[!UICONTROL データ]** 」タブに **** アップロードダイアログが表示され、ファイルのアップロードの進行状況が示されます。

![アップロードダイアログ](../images/tutorials/ingest-batch-data/uploading.png)

## データセット指標

ファイルのアップロードが完了した後、「 **[!UICONTROL データセットアクティビティ]** 」タブに「バッチが追加されていません」と表示されることはなくなりました。 代わりに、「 *[!UICONTROL データセットアクティビティ]* 」タブにデータセット指標が表示されるようになりました。 バッチがまだ読み込まれていないので、すべての指標には「0」がこの段階で表示されます。

タブの下部には、「データセットへのデータ」 _[!UICONTROL 処理で取り込まれたデータの]_バッチID[(バッチID](#add-data-to-dataset))を示すリストが表示されます。 また、取り込まれた_[!UICONTROL &#x200B;日付、取り込まれた]_ 記録数 _[!UICONTROL 、現在のバッチ]___状態など、バッチに関する情報も含まれます。

![データセット指標](../images/tutorials/ingest-batch-data/batch_loading.png)

## バッチの詳細

「 _[!UICONTROL バッチID]_」をクリックして、バッチに関する追加の詳細を表示し、**[!UICONTROL &#x200B;バッチの概要を表示します&#x200B;]**。 バッチの読み込みが完了すると、バッチに関する情報が更新され、取り込まれた_[!UICONTROL &#x200B;レコード数]_ と _[!UICONTROL ファイルサイズが表示されます]_。 また、_[!UICONTROL &#x200B;ステータス]_ は「成功」または「失敗」に変わります。 バッチが失敗した場合は、 _[!UICONTROL エラーコード]_(Error Code)セクションに、取り込み中のエラーに関する詳細が含まれます。

バッチ取り込みに関する詳細とよくある質問については、『 [バッチ取り込みのトラブルシューティングガイド](../batch-ingestion/troubleshooting.md)』を参照してください。

データ **[!UICONTROL セットアクティビティ]** 画面に戻るには、階層リンクでデータセットの名前(_[!UICONTROL Loyality Details]_)をクリックします。

![バッチの概要](../images/tutorials/ingest-batch-data/batch_overview.png)

## プレビューデータセット

データセットの準備が整うと、「データセットのアクティビティ **[!UICONTROL 」タブの上部に、]** プレビューデータセット **[!UICONTROL (Dataset Dataset]** )に関するオプションが表示されます。

「 **[!UICONTROL プレビューデータセット]** 」をクリックしてダイアログを開き、データセット内のサンプルデータを表示します。 データセットがスキーマを使用して作成された場合は、プレビューの左側にデータセットスキーマの詳細が表示されます。 矢印を使用してスキーマを展開すると、スキーマ構造を確認できます。 プレビューデータ内の各列見出しは、データセット内のフィールドを表します。

![データセットの詳細](../images/tutorials/ingest-batch-data/dataset_details.png)

## 次の手順とその他のリソース

データセットを作成し、データをに正しく取り込めたので [!DNL Experience Platform]、これらの手順を繰り返して新しいデータセットを作成するか、既存のデータセットにデータを取り込みます。

バッチ取り込みの詳細については、 [バッチ取り込みの概要を読み](../batch-ingestion/overview.md) 、次のビデオを参照して学習を補ってください。

>[!WARNING]
>
> 次のビデオに示す [!DNL Platform] UIは古いです。 最新のUIのスクリーンショットと機能については、上記のドキュメントを参照してください。

>[!VIDEO](https://video.tv.adobe.com/v/27269?quality=12&learn=on)
