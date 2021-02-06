---
keywords: Experience Platform；ホーム；人気の高いトピック；顧客属性
solution: Experience Platform
title: UIでの顧客属性ソース接続の作成
topic: overview
type: Tutorial
description: 顧客属性プロファイルデータをAdobe Experience Platformに収集するためのUIでソース接続を作成する方法を説明します。
translation-type: tm+mt
source-git-commit: c7fb0d50761fa53c1fdf4dd70a63c62f2dcf6c85
workflow-type: tm+mt
source-wordcount: '377'
ht-degree: 6%

---


# UIでの顧客属性ソース接続の作成

このチュートリアルでは、顧客属性プロファイルデータをAdobe Experience Platformに収集するためのソース接続をUIで作成する手順を説明します。 顧客属性について詳しくは、[概要ドキュメント](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/attributes.html)を参照してください。

## ソース接続の作成

[Adobe Experience Platform](https://platform.adobe.com)にログインし、左のナビゲーションバーで「**[!UICONTROL ソース]**」を選択してソースワークスペースにアクセスします。 **[!UICONTROL カタログ]**&#x200B;画面には、受信接続を作成するために使用できるソースが表示され、各ソースには、それらに関連付けられた既存の接続数が表示されます。 **[!UICONTROL 顧客属性]**&#x200B;のオプションを選択し、**[!UICONTROL 追加data]**&#x200B;を選択します。 接続が確立されるまでしばらく時間をおくと、接続が正常に確立された場合にリダイレクトされます。

>[!NOTE]
>
>顧客属性プロファイルデータのソースコネクタを既に確立している場合、ソースとの接続オプションは無効になります。

![](../../../../images/tutorials/create/customer-attributes/catalog.png)

**ソースアクティビティ**&#x200B;画面には、顧客属性プロファイルデータに対して以前に確立した接続がすべてリストされます。[**データの選択**]をクリックすると、新しい接続を作成できます。

>[!NOTE]
>
>異なるデータを取り込むために、1つのソースに対して複数の受信接続を作成できます。

![](../../../../images/tutorials/create/customer-attributes/source_activity.png)

使用可能な顧客属性プロファイルデータセットのリストから、[!DNL Platform]に取り込む顧客属性データセットを選択し、**次へ**&#x200B;をクリックします。

>[!NOTE]
>
>顧客属性ソース接続ごとに選択できるデータセットは1つだけです。

![](../../../../images/tutorials/create/customer-attributes/select_data.png)

**確認**&#x200B;の手順が表示され、新しい受信接続を作成前に確認できます。 接続の詳細は、次のようなカテゴリ別にグループ化されます。

* **ソースの詳細**:ソース接続の種類と選択したソースデータが表示されます。
* **ターゲットの詳細**:その他のソースコネクタを作成する場合、このコンテナには、データセットが適用するスキーマなど、ソースデータが取り込むデータセットが表示されます。顧客属性プロファイルデータは自動的にマッピングされ、リアルタイム顧客プロファイルに取り込まれます。

![](../../../../images/tutorials/create/customer-attributes/review.png)

## 次の手順

接続が作成されると、ターゲットスキーマとデータセットが自動的に作成され、受信データが格納されます。初期取り込みが完了すると、顧客属性プロファイルデータは、[!DNL Real-time Customer Profile]や[!DNL Segmentation Service]などの下流の[!DNL Platform]サービスで使用できます。 詳しくは、次のドキュメントを参照してください。

* [[!DNL Real-time Customer Profile] 概要](../../../../../profile/home.md)
* [[!DNL Segmentation Service] 概要](../../../../../segmentation/home.md)
