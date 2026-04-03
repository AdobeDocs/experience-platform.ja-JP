---
title: 「ネットワーク」タブ
description: Adobe Experience Platform Debuggerの「ネットワーク」タブの使用方法を説明します。
keywords: デバッガー；experience Platform デバッガー拡張機能；chrome；拡張機能；ネットワーク；情報
seo-description: Experience Platform Debugger Network screen
seo-title: Network Tab
uuid: 839686c9-6e4f-4661-acf6-150ea24dc47f
exl-id: ed0579ef-ec26-43df-9453-a395c105038a
source-git-commit: e4ee4accdb28dafda7e37625eb84062bb6e53644
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 60%

---

# 「ネットワーク」タブ

「**Network**」タブには、ページで行われたすべてのAdobe Experience Cloud ソリューション呼び出しが集約され、左から右に順に表示されます。 標準パラメーターは、わかりやすい名前で自動的にラベル付けされ、同じ役割の共通パラメーターにグループ化されて配置されます。

![](images/network.jpg)

この画面は、複数のヒットをまたいでキーと値のペアを比較する場合に便利です。Experience Cloud 訪問者 ID や追加データ IDなど、統合に使用されたパラメーターが統合内で一貫していることを確認できます。

>[!NOTE]
>
>この時点では、ソリューション呼び出し（例えば、Analytics コンテキスト変数、Target カスタムパラメーター、Experience Cloud ID サービス顧客 ID）に渡されたすべてのパラメーターがネットワーク画面に表示されているわけではありません。

情報をソリューションで変更するには、左側のナビゲーションのリストから表示するソリューションを選択します。次の例は、Analytics のみを表示するようにフィルターされています。

![](images/network-analytics.jpg)

すべての解決策の表示に戻るには、**[!UICONTROL Network]**&#x200B;を選択します

ネットワークビューで項目を選択すると、展開されたビューが表示されます。 展開された表示ウィンドウから、表示された情報をクリップボードにコピーできます。

![](images/network-expand.jpg)

<!--
Use the icon at the top of each column to copy the server call URL to your clipboard, where you can paste it into another document for reference or debugging purposes.

![](images/copy.jpg)
-->

リストをクリアするには、**[!UICONTROL Remove Events]**&#x200B;を選択します。

この画面の情報を含むExcel ファイルをダウンロードするには、**[!UICONTROL Download]**&#x200B;を選択します。
