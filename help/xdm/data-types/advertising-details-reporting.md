---
title: Advertising詳細レポートデータタイプ
description: Advertising詳細レポートエクスペリエンスデータモデル（XDM）データタイプについて説明します。
exl-id: fbca5b2a-a9bd-4f76-a494-d682cb2cbfbc
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 12%

---

# [!UICONTROL Advertisingの詳細 &#x200B;] レポート データ タイプ

[!UICONTROL Advertisingの詳細 &#x200B;] レポートは、広告に関連する主要な属性をキャプチャする標準のエクスペリエンスデータモデル（XDM）データタイプです。 広告 ID、広告主 ID とキャンペーン ID、長さ、シーケンス内の位置、広告をレンダリングするプレーヤーの詳細などの情報が含まれます。 このデータタイプを使用して、広告のパフォーマンスやエンゲージメントの様々な側面を追跡および分析し、オーディエンスが様々な広告とどのようにやり取りし、対応するかについてのインサイトを提供できます。

+++オンにすると、Advertisingの詳細レポートのデータタイプを示す図が表示されます。
![Advertisingの詳細レポートデータタイプの図。](../images/data-types/advertising-details-information.png)
+++

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------------------------------|-----------------|-----------|-----------------------------------------------------------------------------------------------|
| [!UICONTROL &#x200B; 広告名 &#x200B;] | `friendlyName` | 文字列 | 人間が判読できる広告の名前。 レポートでは、「Ad Name」は分類、「Ad Name （variable）」はeVarです。 |
| [!UICONTROL &#x200B; 広告 ID] | `name` | 文字列 | 広告の ID。 任意の整数や文字の組み合わせ。 |
| [!UICONTROL &#x200B; 広告の長さまたはデュレーション &#x200B;] | `length` | 整数 | ビデオ広告の長さ（秒）。 |
| [!UICONTROL &#x200B; ポッド位置の広告（広告開始） &#x200B;] | `podPosition` | 整数 | 親広告の開始内の広告のインデックス （例：最初の広告はインデックス 0、2 番目の広告はインデックス 1）。 |
| [!UICONTROL &#x200B; 広告プレーヤー名 &#x200B;] | `playerName` | 文字列 | 広告のレンダリングを担当するプレイヤーの名前。 |
| [!UICONTROL &#x200B; 広告主 &#x200B;] | `advertiser` | 文字列 | 広告で商品が取り上げられる会社またはブランド。 |
| [!UICONTROL &#x200B; 広告キャンペーン &#x200B;] | `campaignID` | 文字列 | 広告キャンペーンの ID。 |
| [!UICONTROL &#x200B; 広告クリエイティブ ID] | `creativeID` | 文字列 | 広告クリエイティブの ID。 |
| [!UICONTROL &#x200B; 広告サイト ID] | `siteID` | 文字列 | 広告サイトの ID。 |
| [!UICONTROL &#x200B; 広告クリエイティブ URL] | `creativeURL` | 文字列 | 広告クリエイティブの URL。 |
| [!UICONTROL &#x200B; 広告プレースメント ID] | `placementID` | 文字列 | 広告のプレースメント ID。 |
| [!UICONTROL &#x200B; 広告完了 &#x200B;] | `isCompleted` | ブール値 | 広告が完了したかどうかをトラッキングします。 |
| [!UICONTROL &#x200B; 広告開始 &#x200B;] | `isStarted` | ブール値 | 広告が開始されたかどうかをトラッキングします。 |
| [!UICONTROL &#x200B; 広告の再生時間 &#x200B;] | `timePlayed` | 整数 | 広告の視聴に費やした合計時間（秒単位）（再生秒数）。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[ 公開 XDM リポジトリ ](https://github.com/adobe/xdm/blob/master/components/datatypes/advertisingdetails.schema.json) を参照してください。
