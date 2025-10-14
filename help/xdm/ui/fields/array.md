---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；配列；フィールド；
solution: Experience Platform
title: UI での配列フィールドの定義
description: Experience Platformユーザーインターフェイスで配列フィールドを定義する方法を説明します。
exl-id: 9ac55554-c29b-40b2-9987-c8c17cc2c00c
source-git-commit: b66a50e40aaac8df312a2c9a977fb8d4f1fb0c80
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---

# UI での配列フィールドの定義

Adobe Experience Platform ユーザーインターフェイスでエクスペリエンスデータモデル（XDM）フィールドを定義する際に、そのフィールドを配列として指定できます。

配列の内容は、そのフィールドに対して選択された [!UICONTROL &#x200B; タイプ &#x200B;] によって異なります。 例えば、フィールドの [!UICONTROL &#x200B; タイプ &#x200B;] が「[!UICONTROL &#x200B; 文字列 &#x200B;]」に設定されている場合、そのフィールドを配列として設定すると、フィールドは文字列の配列として指定されます。 フィールドの [!UICONTROL &#x200B; タイプ &#x200B;] が複数フィールドのデータタイプ（「[!UICONTROL &#x200B; 郵送先住所 &#x200B;]」など）に設定されている場合、データタイプに準拠する郵送先住所オブジェクトの配列になります。

[UI で新しいフィールドを定義 &#x200B;](./overview.md#define) した後、右側のパネルの「**[!UICONTROL 配列]**」チェックボックスを選択して、配列フィールドとして設定できます。

![](../../images/ui/fields/special/array.png)

チェックボックスを選択すると、右側のパネルに追加のコントロールが表示され、オプションで配列をさらに制限できます。 特定の制約を適用しない場合は、このフィールドを空白のままにします。

アレイのその他の構成コントロールは次のとおりです。

| フィールドプロパティ | 説明 |
| --- | --- |
| [!UICONTROL &#x200B; 最小長 &#x200B;] | 取り込みを正常に行うには、配列に含める必要がある項目の最小数。 |
| [!UICONTROL &#x200B; 最大長 &#x200B;] | 取り込みを正常に行うには、配列に含める必要がある項目の最大数です。 |
| [!UICONTROL &#x200B; 一意の項目のみ &#x200B;] | 「[!UICONTROL True]」に設定した場合、取り込みを正常に行うには、配列内の各項目が一意である必要があります。 |

{style="table-layout:auto"}

フィールドの設定が完了したら、「**[!UICONTROL 適用]**」を選択して、スキーマに変更を適用します。

![](../../images/ui/fields/special/array-config.png)

キャンバスが更新され、フィールドに加えられた変更が反映されます。 キャンバスのフィールド名の横に表示されるデータタイプには、1 組の角括弧（`[]`）が追加され、フィールドがそのデータタイプの配列を表していることを示します。

![](../../images/ui/fields/special/array-applied.png)

## 次の手順

このガイドでは、UI で配列フィールドを定義する方法について説明しました。 [!DNL Schema Editor] で他の XDM フィールドタイプを定義する方法については、[UI でのフィールドの定義 &#x200B;](./overview.md#special) の概要を参照してください。
