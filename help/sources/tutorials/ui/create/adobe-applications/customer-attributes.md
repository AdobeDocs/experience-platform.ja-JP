---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIでの顧客属性ソースコネクタの作成
topic: overview
translation-type: tm+mt
source-git-commit: f09ff4d1b159a6989868c5cfc35b361cfb640a99

---


# UIでの顧客属性ソースコネクタの作成

このチュートリアルでは、顧客属性のプロファイルデータをAdobe Experience Platformに収集するためのソースコネクタをUIで作成する手順を説明します。 顧客属性について詳しくは、概要ドキュメント [](https://docs.adobe.com/content/help/ja-JP/core-services/interface/customer-attributes/attributes.html)。

## ソース接続の作成

<a href="https://platform.adobe.com" target="_blank">Adobe Experience Platformにログインし</a> 、左のナビゲーションバーから「 **Sources** 」を選択して、ソースワークスペースにアクセスします。 カタロ *グ画面には* 、受信接続を作成するために使用可能なソースが表示され、各ソースには、それらに関連付けられた既存の接続の数が表示されます。 「顧客属性」のオプション **を選択し** 、「ソースを接続」をク **リックします**。 接続が確立されるまでしばらく時間をかけておくと、接続が正常に確立された場合にリダイレクトされます。

>[!NOTE] 顧客属性プロファイルデータのソースコネクタを既に確立している場合は、ソースへの接続オプションが無効になります。

![](../../../../images/tutorials/create/customer-attributes/CA-sources_catalog.png)

ソース *アクティビティ* 画面では、顧客属性プロファイルデータに対して以前に確立されたすべての接続がリストされ、「データを選択」をクリックして新しい接続を作 **成できます**。

>[!NOTE] 異なるデータを取り込むために、1つのソースへの複数の受信接続を作成できます。

![](../../../../images/tutorials/create/customer-attributes/CA-source_activity.png)

利用可能な顧客属性プロファイルデータセットのリストから、プラットフォームに取り込むデータセットを選択し、「 **Next**」をクリックします。

>[!NOTE] 顧客属性のソース接続ごとに1つのデータセットのみを選択できます。

![](../../../../images/tutorials/create/customer-attributes/CA-select_data.png)

レビュー *手順が表示され* 、新しい受信接続を作成する前に確認できます。 接続の詳細は、次のようなカテゴリでグループ化されます。

* *ソースの詳細*:ソース接続の種類と選択したソースデータを表示します。
* *ターゲットの詳細*:他のソースコネクタを作成する場合、このコンテナには、データセットが適合するスキーマなど、ソースデータが取り込まれるデータセットが表示されます。 顧客属性プロファイルデータは、自動的にマッピングされ、リアルタイム顧客プロファイルに取り込まれます。

![](../../../../images/tutorials/create/customer-attributes/CA-review.png)

## 次の手順

接続が作成されると、ターゲットスキーマとデータセットが自動的に作成され、受信データが格納されます。 初回の取り込みが完了すると、顧客属性プロファイルデータは、リアルタイム顧客プロファイルやセグメント化サービスなど、下流のプラットフォームサービスで使用できます。 詳しくは、次のドキュメントを参照してください。

* [リアルタイム顧客プロファイルの概要](../../../../../profile/home.md)
* [セグメント化サービスの概要](../../../../../segmentation/home.md)
