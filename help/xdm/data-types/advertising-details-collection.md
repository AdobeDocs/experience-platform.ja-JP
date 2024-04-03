---
title: 広告の詳細コレクションのデータタイプ
description: Advertising Details Collection Experience Data Model(XDM) データタイプについて説明します。
source-git-commit: fe239bee3c853d43c04200092f59537dfeb00c87
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 11%

---

# [!UICONTROL 広告の詳細] コレクションデータタイプ

[!UICONTROL 広告の詳細] コレクションは、広告に関連する主要な属性を取り込む、標準の Experience Data Model(XDM) データ型です。 これには、広告 ID、広告主とキャンペーン ID、長さ、シーケンス内の位置、広告をレンダリングするプレーヤーに関する詳細などの情報が含まれます。 このデータ型を使用すると、広告のパフォーマンスとエンゲージメントの様々な側面を追跡および分析し、オーディエンスが様々な広告とどのようにやり取りし、対応するかに関するインサイトを提供できます。 指定した情報は、ストリーミングデータのトラッキングに使用されます。

+++「 」を選択して、Advertising Details Collection データタイプのダイアグラムを表示します。
![Advertising Details Collection データタイプの図です。](../images/data-types/advertising-details-collection.png)
+++

>[!NOTE]
>
>各ディスプレイ名には、オーディオおよびビデオパラメーターに関する詳細情報へのリンクが含まれます。 リンクされたページには、Adobe、実装値、ネットワークパラメーター、レポートおよび重要な考慮事項によって収集されるビデオ広告データの詳細が含まれます。

| 表示名 | プロパティ | データタイプ | 必須 | 説明 |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|-----------|----------------------------------------------------------------------------------------------------------------------------------|
| [[!UICONTROL 広告主]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html#advertiser) | `advertiser` | 文字列 | × | 広告で商品が取り上げられる会社またはブランド。 |
| [[!UICONTROL 広告キャンペーン]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html#campaign-id) | `campaignID` | 文字列 | × | 広告キャンペーンの ID。 |
| [[!UICONTROL 広告クリエイティブ ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html#creative-id) | `creativeID` | 文字列 | × | 広告クリエイティブの ID。 |
| [[!UICONTROL 広告クリエイティブ URL]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html#creative-url) | `creativeURL` | 文字列 | × | 広告クリエイティブの URL。 |
| [[!UICONTROL ポッド位置の広告（広告開始）]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html#ad-start) | `podPosition` | 整数 | ○ | 親広告開始内の広告のインデックス。例えば、最初の広告のインデックスは 0 で、2 番目の広告のインデックスは 1 です。 |
| [[!UICONTROL 広告の長さまたは期間]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html#ad-length) | `length` | 整数 | ○ | ビデオ広告の長さ（秒）。 |
| [[!UICONTROL 広告名]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html#ad-name) | `friendlyName` | 文字列 | ○ | 人間が読み取れる広告の名前。 レポートでは、「広告名」が分類、「広告名（変数）」が eVar です。 |
| [[!UICONTROL 広告プレースメント ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html#placement-id) | `placementID` | 文字列 | × | 広告のプレースメント ID。 |
| [[!UICONTROL 広告プレーヤー名]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html#ad-player-name) | `playerName` | 文字列 | ○ | 広告のレンダリングをおこなうプレーヤーの名前。 |
| [[!UICONTROL 広告サイト ID]](https://experienceleague.adobe.com/docs/media-analytics/using/implementation/variables/ad-parameters.html#site-id) | `siteID` | 文字列 | × | 広告サイトの ID。 |
