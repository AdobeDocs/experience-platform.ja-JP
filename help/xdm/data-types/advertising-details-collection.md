---
title: Advertising Details Collection Data Type
description: Advertising Details Collection Experience Data Model （XDM）データタイプについて説明します。
exl-id: 3f6bf1f9-c728-46af-804a-cb41eb29951b
source-git-commit: 58f69a78fb3c622c8741d7a1618f15509c160a5b
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 8%

---

# [!UICONTROL Advertising Details] コレクション データ型

[!UICONTROL Advertising Details] コレクションは、広告に関連する主要な属性をキャプチャする標準のExperience Data Model （XDM） データ型です。 広告ID、広告主およびキャンペーン ID、長さ、シーケンス内の位置、広告をレンダリングするプレーヤーに関する詳細などの情報が含まれます。 このタイプのデータを利用することで、広告のパフォーマンスやエンゲージメントのさまざまな側面を追跡および分析し、オーディエンスがさまざまな広告とどのように関わり、反応するかについてのインサイトを得ることができます。 この情報は、ストリーミングデータの追跡に使用されます。

+++を選択すると、Advertisingの詳細コレクションのデータタイプのダイアグラムが表示されます。
![Advertising Details Collection データタイプのダイアグラム。](../images/data-types/advertising-details-collection.png)
+++

>[!NOTE]
>
>各表示名には、オーディオおよびビデオのパラメーターに関する詳細情報へのリンクが含まれています。 リンクされたページには、Adobeによって収集されたビデオとデータ、実装値、ネットワークパラメーター、レポート、および重要な考慮事項の詳細が含まれています。

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|-----------|----------|-----------------------------------------------------------------------------------------------------------------------|
| [[!UICONTROL Ad Advertiser]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#advertiser) | `advertiser` | 文字列 | × | 広告で商品が取り上げられる企業やブランド。 |
| [[!UICONTROL Ad Campaign]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#campaign-id) | `campaignID` | 文字列 | × | 広告キャンペーンのID。 |
| [[!UICONTROL Ad Creative ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#creative-id) | `creativeID` | 文字列 | × | 広告クリエイティブのID。 |
| [[!UICONTROL Ad Creative URL]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#creative-url) | `creativeURL` | 文字列 | × | 広告クリエイティブのURL。 |
| [[!UICONTROL Ad In Pod Position (Ad Start)]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#ad-start) | `podPosition` | 整数 | ○ | 例えば、親広告内の広告のインデックスは、最初の広告のインデックスが0で、2番目の広告のインデックスが1で始まります。 |
| [[!UICONTROL Ad Length Or Duration]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#ad-length) | `length` | 整数 | ○ | 動画広告の長さ（秒単位）。 |
| [[!UICONTROL Ad Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#ad-name) | `friendlyName` | 文字列 | ○ | 人間が読み取れる広告の名前。 レポートでは、「Ad Name」は分類であり、「Ad Name （variable）」はeVarです。 |
| [[!UICONTROL Ad Placement ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#placement-id) | `placementID` | 文字列 | × | 広告のプレースメント ID。 |
| [[!UICONTROL Ad Player Name]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#ad-player-name) | `playerName` | 文字列 | ○ | 広告のレンダリングを担当するプレーヤーの名前。 |
| [[!UICONTROL Ad Site ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html?lang=ja#site-id) | `siteID` | 文字列 | × | 広告サイトのID。 |

{style="table-layout:auto"}
