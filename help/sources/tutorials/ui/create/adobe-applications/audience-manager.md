---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UI での Adobe Audience Manager ソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: f09ff4d1b159a6989868c5cfc35b361cfb640a99
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 13%

---


# UI での Adobe Audience Manager ソースコネクタの作成

このチュートリアルでは、ユーザーインターフェイスを使用してコンシューマーエクスペリエンスのイベントデータをPlatformに取り込むためのAdobe Audience Manager用のソースコネクタを作成する手順を説明します。

## Adobe Audience Managerを使用したソース接続の作成

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし</a> 、左のナビゲーションバーで「 **ソース** 」を選択してソースワークスペースにアクセスします。 [ *カタログ* ]画面には、ソース接続を作成できる様々なソースが表示され、各ソースには、それらに関連付けられた既存の接続数が表示されます。

「 *Adobeアプリ* 」カテゴリの下で、「 **Adobe Audience Manager** 」を選択して、画面の右側に情報バーを表示します。 情報バーには、選択したソースの簡単な説明と、ドキュメントの表示やソースへの接続に関するオプションが表示されます。

Adobe Audience Manager用の新しいソースコネクタを作成するには、[ **接続元**]をクリックします。

![](../../../../images/tutorials/create/aam/aam_catalog.png)

ダイアログボックスが表示されます。「 **接続** 」をクリックして接続を作成します。

![](../../../../images/tutorials/create/aam/aam_connect_full.png)

Adobe Audience Managerとのソース接続が確立されると、Audience Managerコネクタの *ソースアクティビティ* ページが表示されます。

![](../../../../images/tutorials/create/aam/aam_flow.png)

受信Audience Managerデータを一時停止する場合は、データフローリストをクリックし、右の「 *プロパティ**」列から「ステータス* 」を切り替えます。

![](../../../../images/tutorials/create/aam/aam_flow_disable.png)

## 次の手順

Audience Managerデータフローがアクティブな間、受信データは自動的にリアルタイム顧客プロファイルに取り込まれます。 この受信データを利用して、Platformセグメントサービスを使用してオーディエンスセグメントを作成できるようになりました。 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../../../profile/home.md)
- [セグメント化サービスの概要](../../../../../segmentation/home.md)