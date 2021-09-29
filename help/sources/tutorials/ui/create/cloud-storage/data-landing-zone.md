---
keywords: Experience Platform；ホーム；人気のあるトピック；データランディングゾーン；データランディングゾーン
solution: Experience Platform
title: UIを使用したデータランディングゾーンのプラットフォームへの接続
topic-legacy: overview
type: Tutorial
description: Platformユーザーインターフェイスを使用してデータランディングゾーンソースコネクタを作成する方法を説明します。
exl-id: 653c9958-5d89-4b0c-af3d-a3e74aa47a08
source-git-commit: ca7197036283ee15dbf60c113d361a5ea34d65c1
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 9%

---

# UIを使用して[!DNL Data Landing Zone]をPlatformに接続します

[!DNL Data Landing Zone] は、Adobe Experience Platformでプロビジョニングされた一時ファイルストレージのクラウドベースのデータストレージ機能です。[!DNL Data Landing Zone] は、Platformに対するデータの出入りにのみ使用されます。データは、7日後に[!DNL Data Landing Zone]から自動的に削除されます。

このチュートリアルでは、Platformユーザーインターフェイスを使用して[!DNL Data Landing Zone]ソース接続を作成する手順を説明します。

## はじめに

このチュートリアルは、Adobe Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [ソース](../../../../home.md):Experience Platformを使用すると、様々なソースからデータを取り込みながら、Platformサービスを使用して、受信データの構造化、ラベル付け、拡張をおこなうことができます。
* [サンドボックス](../../../../../sandboxes/home.md)：Experience Platform は、単一の Platform インスタンスを別々の仮想環境に分割して、デジタルエクスペリエンスアプリケーションの開発と発展を支援する仮想サンドボックスを提供します。

## ファイルを[!DNL Data Landing Zone]からPlatformに移動します

プラットフォームUIで、左側のナビゲーションから「**[!UICONTROL ソース]**」を選択して、「[!UICONTROL ソース]」ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択できます。 または、検索バーを使用して、目的の特定のソースを見つけることができます。

「[!UICONTROL クラウドストレージ]」カテゴリで「[!DNL Data Landing Zone]」を選択し、「**[!UICONTROL データを追加]**」を選択します。

![カタログ](../../../../images/tutorials/create/dlz/catalog.png)

[!UICONTROL データの追加]手順が表示され、Platformに取り込むデータを選択およびプレビューするインターフェイスが表示されます。

![add-data](../../../../images/tutorials/create/dlz/add-data.png)

クラウドストレージソースのデータフローを作成する手順を示す詳細なガイドについては、[クラウドストレージデータフローを作成してデータをPlatformに送信する方法のチュートリアル](../../dataflow/batch/cloud-storage.md)を参照してください。

## [!DNL Data Landing Zone]資格情報を取得して更新します

[!DNL Data Landing Zone] は、Adobe Experience Platform Sourcesライセンスに付属している標準のソースです。[!DNL Data Landing Zone] は、SAS URIとSASトークンベースの認証を使用します。[!UICONTROL ソースカタログ]ページから認証資格情報を取得して更新できます。

[!UICONTROL ソースカタログ]の「[!UICONTROL クラウドストレージ]」カテゴリで、省略記号(**...**)を&#x200B;**[!UICONTROL データランディングゾーン]**&#x200B;カードから削除します。 表示されるドロップダウンメニューから、「**[!UICONTROL View credentials]**」を選択します。

![options](../../../../images/tutorials/create/dlz/options.png)

ポップオーバーが表示され、コンテナ名、SASトークン、ストレージアカウント名、SAS URIが表示されます。

「**[!UICONTROL 資格情報を更新]**」を選択し、更新された資格情報が処理されるまで数秒間待ちます。

![view-credentials](../../../../images/tutorials/create/dlz/credentials.png)

## 次の手順

このチュートリアルでは、[!DNL Data Landing Zone]コンテナにアクセスし、資格情報を取得して更新する方法を学習しました。 次の[データフローの作成に関するチュートリアルに進み、クラウドストレージからPlatform](../../dataflow/batch/cloud-storage.md)にデータを取り込むことができます。
