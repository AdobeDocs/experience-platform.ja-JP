---
title: 結合 ID をリセット
description: 同じページで呼び出されるイベントを分離できる、非推奨（廃止予定）のアクション。
source-git-commit: c55e425f146e4afdb2314b432c9dc48391e02e63
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 0%

---

# 結合 ID をリセット

>[!IMPORTANT]
>
>このアクションは非推奨（廃止予定）です。 代わりに、[&#x200B; 内部リンククリック数を収集 &#x200B;](../configure/data-collection.md#collect-internal-link-clicks) 設定を使用します。

**[!UICONTROL Reset merge ID]** アクションタイプを使用すると、同じページで呼び出されるイベントを分割できます。 通常、Adobeに送信する複数のペイロードがある可能性のある内部リンクシナリオで使用されます。 この操作により、イベントの結合 ID をリセットして、Edge Networkへの到着後に同じイベントに含まれると見なされないようにすることができます。

同じページ上の複数のイベントの分離または統合の方法を制御する場合、タグ拡張機能の設定時に [&#x200B; 内部リンククリック数を収集 &#x200B;](../configure/data-collection.md#collect-internal-link-clicks) オプションを使用します。
