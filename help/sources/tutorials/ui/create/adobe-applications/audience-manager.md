---
keywords: Experience Platform;home;popular topics;Audience manager source connector;Audience Manager;audience manager connector
solution: Experience Platform
title: UI での Adobe Audience Manager ソースコネクタの作成
topic: overview
type: Tutorial
description: このチュートリアルでは、ユーザーインターフェイスを使用してコンシューマーエクスペリエンスイベントデータをプラットフォームに取り込むための、Adobe Audience Manager用のソースコネクタを作成する手順を説明します。
translation-type: tm+mt
source-git-commit: 8c94d3631296c1c3cc97501ccf1a3ed995ec3cab
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 11%

---


# UI での Adobe Audience Manager ソースコネクタの作成

このチュートリアルでは、ユーザーインターフェイスを使用してコンシューマーエクスペリエンスイベントデータをプラットフォームに取り込むための、Adobe Audience Manager用のソースコネクタを作成する手順を説明します。

## Adobe Audience Managerとのソース接続の作成

[Adobe Experience Platformにログインし、左のナビゲーションバーで「](https://platform.adobe.com) ソース **** 」を選択してソースワークスペースにアクセスします。 [ **カタログ** ]画面には、ソース接続を作成できる様々なソースが表示され、各ソースには、それらに関連付けられた既存の接続数が表示されます。

[ **Adobeアプリ** ]カテゴリの下で、[ **Adobe Audience Manager** ]を選択して情報バーを画面の右側に表示します。 情報バーには、選択したソースの簡単な説明と、ドキュメントの表示やソースへの接続に関するオプションが表示されます。

Adobe Audience Manager用の新しいソースコネクタを作成するには、 **追加data**.

![](../../../../images/tutorials/create/aam/catalog.png)

ダイアログボックスが表示されます。「 **接続** 」をクリックして接続を作成します。

![](../../../../images/tutorials/create/aam/connect_full.png)

Adobe Audience Managerとのソース接続が確立されると、Audience Managerコネクタの **ソースアクティビティ** ページが表示されます。

![](../../../../images/tutorials/create/aam/flow.png)

受信Audience Managerデータを一時停止する場合は、データフローリストをクリックし、右の「 *プロパティ**」列から「ステータス* 」を切り替えます。

![](../../../../images/tutorials/create/aam/flow_disable.png)

## 次の手順

Audience Managerデータフローがアクティブな間、受信データは自動的にリアルタイム顧客プロファイルに取り込まれます。 この受信データを利用して、Platform Segmentation Serviceを使用してオーディエンスセグメントを作成できるようになりました。 詳しくは、次のドキュメントを参照してください。

- [リアルタイム顧客プロファイルの概要](../../../../../profile/home.md)
- [セグメント化サービスの概要](../../../../../segmentation/home.md)