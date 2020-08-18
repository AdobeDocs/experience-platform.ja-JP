---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: UIで顧客属性ソースコネクタを作成する
topic: overview
translation-type: tm+mt
source-git-commit: 999ff0e9525d7d9f67298862c68bb59ddc35e558
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 9%

---


# UIで顧客属性ソースコネクタを作成する

このチュートリアルでは、顧客属性プロファイルデータをAdobe Experience Platformに収集するためのソースコネクタをUIで作成する手順を説明します。 顧客属性について詳しくは、 [概要ドキュメントを参照してください](https://docs.adobe.com/content/help/ja-JP/core-services/interface/customer-attributes/attributes.html)。

## ソース接続の作成

[Adobe Experience Platformにログインし、左のナビゲーションバーで「](https://platform.adobe.com) ソース **** 」を選択してソースワークスペースにアクセスします。 カ **[!UICONTROL タログ]** 画面には、受信接続を作成するために使用できるソースが表示され、各ソースには、それらに関連付けられた既存の接続数が表示されます。 「 **[!UICONTROL Customer Attributes]** 」のオプションを選択し、「 **[!UICONTROL Data]**」を選択します。 接続が確立されるまでしばらく時間をおくと、接続が正常に確立された場合にリダイレクトされます。

>[!NOTE]
>
>顧客属性プロファイルデータのソースコネクタを既に確立している場合、ソースとの接続オプションは無効になります。

![](../../../../images/tutorials/create/customer-attributes/catalog.png)

[ *ソースアクティビティ* ]画面には、顧客属性プロファイルデータに対して以前に確立されたすべての接続がリストされます。[データの **選択**]をクリックして新しい接続を作成できます。

>[!NOTE]
>
>異なるデータを取り込むために、1つのソースに対して複数の受信接続を作成できます。

![](../../../../images/tutorials/create/customer-attributes/source_activity.png)

利用可能な顧客属性プロファイルデータセットのリストから、に取り込むデータセットを選択し、「 [!DNL Platform] Next ****」をクリックします。

>[!NOTE]
>
>顧客属性ソース接続ごとに選択できるデータセットは1つだけです。

![](../../../../images/tutorials/create/customer-attributes/select_data.png)

「 *レビュー* 」手順が表示され、作成前に新しい受信接続を確認できます。 接続の詳細は、次のようなカテゴリ別にグループ化されます。

* *ソースの詳細*:ソース接続の種類と選択したソースデータが表示されます。
* *ターゲットの詳細*:その他のソースコネクタを作成する場合、このコンテナには、データセットが適用するスキーマなど、ソースデータが取り込むデータセットが表示されます。 顧客属性プロファイルデータは自動的にマッピングされ、リアルタイム顧客プロファイルに取り込まれます。

![](../../../../images/tutorials/create/customer-attributes/review.png)

## 次の手順

接続が作成されると、ターゲットスキーマとデータセットが自動的に作成され、受信データが格納されます。初回取り込みが完了すると、およ [!DNL Platform] びなどのダウンストリームサービスで顧客属性プロファイルデータを使用でき [!DNL Real-time Customer Profile][!DNL Segmentation Service]ます。 詳しくは、次のドキュメントを参照してください。

* [[!DNL Real-time Customer Profile] 概要](../../../../../profile/home.md)
* [[!DNL Segmentation Service] 概要](../../../../../segmentation/home.md)
