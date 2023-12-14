---
title: 広告ポッドの詳細情報データタイプ
description: 広告ポッドの詳細情報エクスペリエンスデータモデル (XDM) データタイプについて説明します。
source-git-commit: 65f3dcf1cacfbc4e8a598244810d238bd88f64bd
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 5%

---

# [!UICONTROL 広告ポッドの詳細情報] データタイプ

[!UICONTROL 広告ポッドの詳細情報] は、標準の Experience Data Model(XDM) データタイプです。 通常、コンテンツの時間中に連続して再生される広告のシーケンスまたはグループを定義します。 以下を使用します。 [!UICONTROL 広告ポッドの詳細情報] 広告ブレーク ID、広告ブレークのわかりやすい名前、広告ブレーク内の広告のインデックス、コンテンツのタイムライン内の広告ブレークのオフセット（秒）などの詳細を取り込むデータ型です。

![広告ポッドの詳細情報データタイプの図です。](../images/data-types/advertising-pod-details-information.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------------------|------------------------|-----------|-------------------------------------------------------|
| [!UICONTROL 広告ブレーク ID] | `ID` | 文字列 | 広告ブレークの ID。 |
| [!UICONTROL ポッドのわかりやすい名前] | `friendlyName` | 文字列 | 広告ブレークのわかりやすい名前。 |
| [!UICONTROL ポッド位置の広告] | `index` | 整数 | 親広告ブレーク開始内の広告のインデックス。 |
| [!UICONTROL ポッドオフセット] | `offset` | 整数 | **必須** コンテンツ内の広告ブレークのオフセット（秒）。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/datatypes/advertisingpoddetails.schema.json)
