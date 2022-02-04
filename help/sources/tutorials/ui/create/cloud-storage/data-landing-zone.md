---
keywords: Experience Platform；ホーム；人気の高いトピック；データランディングゾーン；データランディングゾーン
solution: Experience Platform
title: UI を使用したデータランディングゾーンの Platform への接続
topic-legacy: overview
type: Tutorial
description: Platform ユーザーインターフェイスを使用してデータランディングゾーンソースコネクタを作成する方法を説明します。
exl-id: 653c9958-5d89-4b0c-af3d-a3e74aa47a08
source-git-commit: b007cdf92811b453df5b5d005456a05cd845b769
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 9%

---

# 接続 [!DNL Data Landing Zone] UI を使用して Platform に接続

[!DNL Data Landing Zone] は、Adobe Experience Platformでプロビジョニングされた一時ファイルストレージのクラウドベースのデータストレージ機能です。 データは [!DNL Data Landing Zone] 7 日後

このチュートリアルでは、 [!DNL Data Landing Zone] Platform ユーザーインターフェイスを使用したソース接続

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platform サービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

## ファイルの取り込み元 [!DNL Data Landing Zone] Platform へ

Platform UI で、「 **[!UICONTROL ソース]** 左側のナビゲーションから [!UICONTROL ソース] ワークスペース。 この [!UICONTROL カタログ] 画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、作業対象の特定のソースを見つけることもできます。

以下 [!UICONTROL クラウドストレージ] カテゴリ、選択 [!DNL Data Landing Zone] 次に、 **[!UICONTROL データを追加]**.

![カタログ](../../../../images/tutorials/create/dlz/catalog.png)

この [!UICONTROL データを追加] の手順が表示され、Platform に取り込むデータを選択およびプレビューするためのインターフェイスが提供されます。

![add-data](../../../../images/tutorials/create/dlz/add-data.png)

クラウドストレージソースのデータフローを作成する手順に関する詳細なガイドについては、 [データを Platform に取り込むためのクラウドストレージのデータフローの作成](../../dataflow/batch/cloud-storage.md).

## を取得して更新します。 [!DNL Data Landing Zone] 資格情報

[!DNL Data Landing Zone] は、Adobe Experience Platform Sources ライセンスに付属している標準のソースです。 [!DNL Data Landing Zone] は、SAS URI と SAS トークンベースの認証を使用します。 認証資格情報は、 [!UICONTROL ソースカタログ] ページ。

内 [!UICONTROL ソースカタログ]、 [!UICONTROL クラウドストレージ] カテゴリの中から省略記号 (**...**) を **[!UICONTROL データランディングゾーン]** カード。 表示されるドロップダウンメニューから、「 」を選択します。 **[!UICONTROL 資格情報を表示]**.

![options](../../../../images/tutorials/create/dlz/options.png)

ポップオーバーが表示され、コンテナ名、SAS トークン、ストレージアカウント名、SAS URI が表示されます。

選択 **[!UICONTROL 資格情報を更新]** 更新された資格情報が処理されるまで数秒間待ちます。

>[!TIP]
>
>お使いの [!DNL Data Landing Zone] 資格情報は 90 日後に自動期限切れに設定されています。新しい資格情報を使用してに再接続する必要があります。 [!DNL Data Landing Zone] 有効期限後 Platform のデータフローは、期限が切れる資格情報の影響を受けません。新しい資格情報を使用して、新しい既存のデータフローを引き続き使用できます。

![view-credentials](../../../../images/tutorials/create/dlz/credentials.png)

## 次の手順

このチュートリアルに従うことで、 [!DNL Data Landing Zone] コンテナに追加され、資格情報を取得して更新する方法を学習しました。 次のチュートリアル ( [データフローを作成して、クラウドストレージから Platform にデータを取り込む](../../dataflow/batch/cloud-storage.md).
