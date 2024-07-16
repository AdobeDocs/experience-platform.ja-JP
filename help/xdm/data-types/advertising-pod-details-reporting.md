---
title: Advertising ポッドの詳細レポートデータタイプ
description: Advertising ポッド詳細レポートの Experience Data Model （XDM）データタイプについて説明します。
exl-id: 5164520f-8c48-4eb0-a0b0-66dc10b68356
source-git-commit: 799a384556b43bc844782d8b67416c7eea77fbf0
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 6%

---

# [!UICONTROL Advertising ポッド詳細レポート ] データタイプ

[!UICONTROL Advertising ポッド詳細レポート ] は、標準のエクスペリエンスデータモデル（XDM）データタイプです。 通常、コンテンツブレーク中に連続して再生される広告のシーケンスまたはグループを定義します。 [!UICONTROL Advertising ポッド詳細レポート ] データタイプを使用して、広告ブレーク ID、広告ブレークのわかりやすい名前、ブレーク内の広告のインデックス、コンテンツのタイムライン内の広告ブレークのオフセット （秒）などの詳細をキャプチャします。

![Advertising ポッド詳細レポートデータタイプの図。](../images/data-types/advertising-pod-details-information.png)

| 表示名 | プロパティ | データタイプ | 説明 |
|----------------------------|------------------------|-----------|-------------------------------------------------------|
| [!UICONTROL  広告ブレーク ID] | `ID` | 文字列 | 広告ブレークの ID。 |
| [!UICONTROL  ポッドのわかりやすい名前 ] | `friendlyName` | 文字列 | 広告ブレークのわかりやすい名前。 |
| [!UICONTROL  ポッド位置の広告 ] | `index` | 整数 | 親広告の改ページ内の広告のインデックスが開始されます。 |
| [!UICONTROL  ポッド オフセット ] | `offset` | 整数 | **必須** コンテンツ内の広告ブレークのオフセット （秒単位）。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[ 公開 XDM リポジトリ ](https://github.com/adobe/xdm/blob/master/components/datatypes/advertisingpoddetails.schema.json) を参照してください。
