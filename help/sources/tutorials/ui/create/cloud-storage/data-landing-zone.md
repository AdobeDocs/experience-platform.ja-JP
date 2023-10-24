---
keywords: Experience Platform；ホーム；人気の高いトピック；データランディングゾーン；データランディングゾーン
title: UI を使用したデータランディングゾーンの Platform への接続
description: Platform ユーザーインターフェイスを使用してデータランディングゾーンソースコネクタを作成する方法を説明します。
exl-id: 653c9958-5d89-4b0c-af3d-a3e74aa47a08
source-git-commit: 9cffd508c1bff7ce133f84ca686c414e997343b8
workflow-type: tm+mt
source-wordcount: '672'
ht-degree: 20%

---

# UI を使用した [!DNL Data Landing Zone] の Platform への接続

>[!IMPORTANT]
>
>このページは、 [!DNL Data Landing Zone] *ソース* コネクタのExperience Platform に接続する方法については、 [!DNL Data Landing Zone] *宛先* コネクタ ( [[!DNL Data Landing Zone] 宛先のドキュメントページ](/help/destinations/catalog/cloud-storage/data-landing-zone.md).

[!DNL Data Landing Zone] は、ファイルをAdobe Experience Platformに取り込む、クラウドベースの安全なファイルストレージ機能です。 データは [!DNL Data Landing Zone] 7 日後

このチュートリアルでは、 [!DNL Data Landing Zone] Platform ユーザーインターフェイスを使用したソース接続

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md)：Experience Platform を使用すると、データを様々なソースから取得しながら、Platform サービスを使用して受信データの構造化、ラベル付け、拡張を行うことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform には、単一の Platform インスタンスを別々の仮想環境に分割し、デジタルエクスペリエンスアプリケーションの開発と発展に役立つ仮想サンドボックスが用意されています。

## ファイルの取り込み元 [!DNL Data Landing Zone] Platform へ

Platform の UI で、左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、検索バーを使用して、利用したい特定のソースを見つけることもできます。

の下 [!UICONTROL クラウドストレージ] カテゴリ、選択 [!DNL Data Landing Zone] 次に、「 **[!UICONTROL データを追加]**.

![データランディングゾーンが選択されたソースカタログ。](../../../../images/tutorials/create/dlz/catalog.png)

The [!UICONTROL データを追加] の手順が表示され、Platform に取り込むデータを選択およびプレビューするためのインターフェイスが提供されます。

* インターフェイスの左側はフォルダーブラウザーで、コンテナからファイルのリストを提供します。このリストを使用して、Platform に取り込むことができます。
* インターフェイスの右側では、互換性のあるファイルから最大 100 行のデータをプレビューできます。

Experience Platformにするファイルを選択し、しばらくの間、適切なインターフェイスがプレビュー画面に更新されます。

![「ソース」ワークスペースの「データを追加」インターフェイス。](../../../../images/tutorials/create/dlz/add-data.png)

>[!TIP]
>
>Platform は、ファイルのデータ形式、指定された列の区切り文字、圧縮タイプに関する情報など、選択したファイルのプロパティ情報を自動検出します。

プレビューインターフェイスを使用すると、ファイルの内容と構造を検査できます。 デフォルトでは、プレビューインターフェイスには、選択したフォルダー内の最初のファイルが表示されます。

別のファイルをプレビューするには、検査するファイル名の横にあるプレビューアイコンを選択します。

終了したら、「**[!UICONTROL 次へ]**」を選択します。

![ソースワークスペースのデータプレビューページ。](../../../../images/tutorials/create/dlz/file-detection.png)

クラウドストレージソースのデータフローを作成する手順に関する詳細なガイドについては、 [データを Platform に取り込むためのクラウドストレージのデータフローの作成](../../dataflow/batch/cloud-storage.md).

## を取得して更新します。 [!DNL Data Landing Zone] 資格情報

[!DNL Data Landing Zone] は、Adobe Experience Platform Sources ライセンスに付属している標準のソースです。 [!DNL Data Landing Zone] は、SAS URI と SAS トークンベースの認証を使用します。 認証資格情報は、 [!UICONTROL ソースカタログ] ページに貼り付けます。

Adobe Analytics の [!UICONTROL ソースカタログ]、 [!UICONTROL クラウドストレージ] カテゴリの中から省略記号 (**...**) を **[!UICONTROL データランディングゾーン]** カード。 表示されるドロップダウンメニューから、「 」を選択します。 **[!UICONTROL 資格情報を表示]**.

![データランディングゾーンの表示オプションのリスト。](../../../../images/tutorials/create/dlz/options.png)

ポップオーバーが表示され、コンテナ名、SAS トークン、ストレージアカウント名、SAS URI、有効期限が表示されます。

選択 **[!UICONTROL 資格情報を更新]** 更新された資格情報が処理されるまで数秒間待ちます。

>[!TIP]
>
>お使いの [!DNL Data Landing Zone] 資格情報は 90 日後に自動期限切れに設定されています。新しい資格情報を使用してに再接続する必要があります。 [!DNL Data Landing Zone] 有効期限後 Platform のデータフローは、期限が切れる資格情報の影響を受けません。新しい資格情報を使用して、新しい既存のデータフローを引き続き使用できます。

![特定のデータランディングゾーンアカウントに関連付けられた資格情報。](../../../../images/tutorials/create/dlz/view-credentials.png)

## 次の手順

このチュートリアルに従うことで、 [!DNL Data Landing Zone] コンテナに追加され、資格情報を取得して更新する方法を学習しました。 次のチュートリアル ( [データフローを作成して、クラウドストレージから Platform にデータを取り込む](../../dataflow/batch/cloud-storage.md).
