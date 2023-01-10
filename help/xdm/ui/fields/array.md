---
keywords: Experience Platform；ホーム；人気のトピック；API;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；配列；フィールド；
solution: Experience Platform
title: UI で配列フィールドを定義
description: Experience Platformユーザーインターフェイスで配列フィールドを定義する方法を説明します。
exl-id: 9ac55554-c29b-40b2-9987-c8c17cc2c00c
source-git-commit: 5caa4c750c9f786626f44c3578272671d85b8425
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 2%

---

# UI で配列フィールドを定義する

Adobe Experience Platformユーザーインターフェイスで Experience Data Model(XDM) フィールドを定義する際に、そのフィールドを配列として指定できます。

配列の内容は、 [!UICONTROL タイプ] を選択します。 例えば、フィールドの [!UICONTROL タイプ] が&quot;に設定されている[!UICONTROL 文字列]」の場合、そのフィールドを配列として設定すると、フィールドが文字列の配列として指定されます。 フィールドの [!UICONTROL タイプ] が「[!UICONTROL 郵送先住所]「」の場合、データ型に準拠する郵送先住所オブジェクトの配列になります。

次の条件が満たされた後 [UI で新しいフィールドを定義](./overview.md#define)を使用する場合は、 **[!UICONTROL 配列]** 」チェックボックスをオンにします。

![](../../images/ui/fields/special/array.png)

チェックボックスを選択すると、右側のレールに追加のコントロールが表示され、オプションで配列をさらに制約できます。 特定の制約を適用しない場合は、このフィールドを空白のままにします。

配列の追加の設定コントロールは次のとおりです。

| Field プロパティ | 説明 |
| --- | --- |
| [!UICONTROL 最小長] | 取り込みを正常におこなうために配列に含める必要がある項目の最小数。 |
| [!UICONTROL 最大長] | 取り込みを正常におこなうために配列に含める必要がある項目の最大数。 |
| [!UICONTROL 一意の項目のみ] | &quot;[!UICONTROL True]「」と入力する場合、取り込みを正常におこなうには、配列内の各項目が一意である必要があります。 |

{style=&quot;table-layout:auto&quot;}

フィールドの設定が完了したら、「 **[!UICONTROL 適用]** をクリックして、スキーマに変更を適用します。

![](../../images/ui/fields/special/array-config.png)

キャンバスが更新され、フィールドに加えた変更が反映されます。 キャンバス内のフィールド名の横に表示されるデータタイプには、角括弧 (`[]`) の場合、フィールドがそのデータ型の配列を表していることを示します。

![](../../images/ui/fields/special/array-applied.png)

## 次の手順

このガイドでは、UI で配列フィールドを定義する方法について説明しました。 概要については、 [UI でのフィールドの定義](./overview.md#special) で他の XDM フィールドタイプを定義する方法を学ぶには、以下を実行します。 [!DNL Schema Editor].
