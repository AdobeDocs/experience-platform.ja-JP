---
title: Advertisingの詳細コレクションのデータタイプ
description: Advertising詳細収集エクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: 3f6bf1f9-c728-46af-804a-cb41eb29951b
source-git-commit: 9350cfc299c20bd63a2a559c177b3af02739e5b9
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 14%

---

# [!UICONTROL Advertisingの詳細 &#x200B;] コレクション データ型

[!UICONTROL Advertisingの詳細 &#x200B;] コレクションは、広告に関連する主要な属性をキャプチャする標準の Experience Data Model （XDM）データタイプです。 広告 ID、広告主 ID とキャンペーン ID、長さ、シーケンス内の位置、広告をレンダリングするプレーヤーの詳細などの情報が含まれます。 このデータタイプを使用して、広告のパフォーマンスやエンゲージメントの様々な側面を追跡および分析し、オーディエンスが様々な広告とどのようにやり取りし、対応するかについてのインサイトを提供できます。 指定したこの情報は、ストリーミングデータのトラックに使用されます。

+++オンにすると、Advertisingの詳細収集データタイプの図が表示されます。
![Advertisingの詳細コレクションのデータタイプを示す図。](../images/data-types/advertising-details-collection.png)
+++

>[!NOTE]
>
>各表示名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれています。 リンク先のページには、Adobeで収集された動画広告データの詳細、実装値、ネットワークパラメーター、レポート、重要な検討事項が含まれています。

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|-----------|----------|-----------------------------------------------------------------------------------------------------------------------|
| [[!UICONTROL &#x200B; 広告主 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#advertiser) | `advertiser` | 文字列 | × | 広告で商品が取り上げられる会社またはブランド。 |
| [[!UICONTROL &#x200B; 広告キャンペーン &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#campaign-id) | `campaignID` | 文字列 | × | 広告キャンペーンの ID。 |
| [[!UICONTROL &#x200B; 広告クリエイティブ ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#creative-id) | `creativeID` | 文字列 | × | 広告クリエイティブの ID。 |
| [[!UICONTROL &#x200B; 広告クリエイティブ URL]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#creative-url) | `creativeURL` | 文字列 | × | 広告クリエイティブの URL。 |
| [[!UICONTROL &#x200B; ポッド位置の広告（広告開始） &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#ad-start) | `podPosition` | 整数 | ○ | 親広告の開始内の広告のインデックス （例：最初の広告はインデックス 0、2 番目の広告はインデックス 1）。 |
| [[!UICONTROL &#x200B; 広告の長さまたはデュレーション &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#ad-length) | `length` | 整数 | ○ | ビデオ広告の長さ（秒）。 |
| [[!UICONTROL &#x200B; 広告名 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#ad-name) | `friendlyName` | 文字列 | ○ | 人間が判読できる広告の名前。 レポートでは、「Ad Name」は分類、「Ad Name （variable）」はeVarです。 |
| [[!UICONTROL &#x200B; 広告プレースメント ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#placement-id) | `placementID` | 文字列 | × | 広告のプレースメント ID。 |
| [[!UICONTROL &#x200B; 広告プレーヤー名 &#x200B;]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#ad-player-name) | `playerName` | 文字列 | ○ | 広告のレンダリングを担当するプレイヤーの名前。 |
| [[!UICONTROL &#x200B; 広告サイト ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#site-id) | `siteID` | 文字列 | × | 広告サイトの ID。 |

{style="table-layout:auto"}
