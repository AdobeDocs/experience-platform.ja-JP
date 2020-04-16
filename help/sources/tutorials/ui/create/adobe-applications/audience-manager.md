---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでのAdobeオーディエンスマネージャーソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: f09ff4d1b159a6989868c5cfc35b361cfb640a99

---


# UIでのAdobeオーディエンスマネージャーソースコネクタの作成

このチュートリアルでは、Adobeオーディエンスマネージャー用のソースコネクターを作成し、ユーザーインターフェイスを使用してConsumer Experienceイベントデータをプラットフォームに取り込む手順を説明します。

## Adobe Connection Managerでのソース接続のオーディエンス

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし</a> 、左のナビゲーションバーから「 **Sources** 」を選択して、ソースワークスペースにアクセスします。 カタ *ログ画面には* 、ソース接続を作成できる様々なソースが表示され、各ソースには、それらに関連付けられた既存の接続数が表示されます。

「 *Adobe applications* 」カテゴリの下で、「 **Adobe Application Manager** 」を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ドキュメントを表示したり、ソースに接続するためのオプションが表示されます。

Adobe Connector Manager用の新しいソースコネクタを作成するには、「ソースをオーディエンス」を **クリックしま**&#x200B;す。

![](../../../../images/tutorials/create/aam/aam_catalog.png)

ダイアログボックスが表示されます。[接続 **** ]をクリックして接続を作成します。

![](../../../../images/tutorials/create/aam/aam_connect_full.png)

Adobe Connection Managerとのソースアクティビティが確立されると、オーディエンスマネージャ *ーコネクタのソー* スオーディエンスページが表示されます。

![](../../../../images/tutorials/create/aam/aam_flow.png)

受信するオーディエンス・マネージャ・データを一時停止する場合は、データフロー・リストをクリックし、右の「 *Properties* 」列から「 *Status* 」を切り替えます。

![](../../../../images/tutorials/create/aam/aam_flow_disable.png)

## 次の手順

オーディエンスマネージャのデータフローがアクティブな間、受信データは自動的にリアルタイム顧客プロファイルに取り込まれます。 この受信データを利用して、Platform Segmentation Serviceを使用してオーディエンスセグメントを作成できるようになりました。 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../../../profile/home.md)
- [セグメント化サービスの概要](../../../../../segmentation/home.md)