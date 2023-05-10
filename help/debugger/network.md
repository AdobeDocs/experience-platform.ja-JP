---
title: 「ネットワーク」タブ
description: Adobe Experience Platform Debugger の「ネットワーク」タブの使用方法について説明します。
keywords: debugger;experience Platform Debugger 拡張機能；chrome；拡張機能；ネットワーク；情報
seo-description: Experience Platform Debugger Network screen
seo-title: Network Tab
uuid: 839686c9-6e4f-4661-acf6-150ea24dc47f
exl-id: ed0579ef-ec26-43df-9453-a395c105038a
source-git-commit: df1a67e4b6f3d2eaeaba2b8d3c9b1588ee0b1461
workflow-type: tm+mt
source-wordcount: '219'
ht-degree: 59%

---

# 「ネットワーク」タブ

この **ネットワーク** 「 」タブは、ページでおこなわれたすべてのAdobe Experience Cloudソリューション呼び出しを集計し、左から右の順に表示します。 標準パラメーターは、わかりやすい名前で自動的にラベル付けされ、同じ役割の共通パラメーターにグループ化されて配置されます。

![](images/network.jpg)

この画面は、複数のヒットをまたいでキーと値のペアを比較する場合に便利です。Experience Cloud 訪問者 ID や追加データ IDなど、統合に使用されたパラメーターが統合内で一貫していることを確認できます。

>[!NOTE]
>
>この時点では、ソリューション呼び出し（例えば、Analytics コンテキスト変数、Target カスタムパラメーター、Experience Cloud ID サービス顧客 ID）に渡されたすべてのパラメーターがネットワーク画面に表示されているわけではありません。

情報をソリューションで変更するには、左側のナビゲーションのリストから表示するソリューションを選択します。次の例は、Analytics のみを表示するようにフィルターされています。

![](images/network-analytics.jpg)

すべてのソリューションの表示に戻るには、「 」を選択します。 **[!UICONTROL ネットワーク]**

ネットワーク表示の項目を選択すると、展開された表示が表示されます。 展開された表示ウィンドウから、表示された情報をクリップボードにコピーできます。

![](images/network-expand.jpg)

<!--Use the icon at the top of each column to copy the server call URL to your clipboard, where you can paste it into another document for reference or debugging purposes.

![](images/copy.jpg)-->

リストを消去するには、 **[!UICONTROL イベントを削除]**.

この画面に関する情報を含む Excel ファイルをダウンロードするには、「 **[!UICONTROL ダウンロード]**.
