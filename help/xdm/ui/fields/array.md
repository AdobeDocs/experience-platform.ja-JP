---
keywords: Experience Platform；ホーム；人気のあるトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；配列；フィールド；
solution: Experience Platform
title: UI での配列フィールドの定義
description: 配列ユーザーインターフェイスで配列フィールドを定義するExperience Platformを説明します。
topic-legacy: user guide
exl-id: 9ac55554-c29b-40b2-9987-c8c17cc2c00c
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 2%

---

# UI での配列フィールドの定義

Adobe Experience Platformユーザーインターフェイスでエクスペリエンスデータモデル (XDM) フィールドを定義する際に、そのフィールドを配列として指定できます。

配列の内容は、そのフィールドで選択した [!UICONTROL  型 ] によって異なります。 例えば、フィールドの [!UICONTROL  型 ] が&quot;[!UICONTROL String]&quot;に設定されている場合、そのフィールドを配列として設定すると、フィールドは文字列の配列として指定されます。 フィールドの [!UICONTROL  型 ] が「[!UICONTROL  郵送先住所 ]」などの複数フィールドのデータ型に設定されると、そのデータ型に準拠する郵送先住所オブジェクトの配列になります。

UI](./overview.md#define) で新しいフィールド [ を定義した後、右側のレールの「**[!UICONTROL 配列]**」チェックボックスを選択して、配列フィールドに設定できます。

![](../../images/ui/fields/special/array.png)

このチェックボックスを選択すると、右側のレールに追加のコントロールが表示され、オプションで配列をさらに制約できます。 特定の制約を適用しない場合は、このフィールドを空白のままにします。

配列の追加の設定コントロールは次のとおりです。

| Field プロパティ | 説明 |
| --- | --- |
| [!UICONTROL 最小長] | 取り込みを正常におこなうために配列に含める必要がある項目の最小数。 |
| [!UICONTROL 最大長] | 取り込みを正常に実行するために配列に含める必要がある項目の最大数。 |
| [!UICONTROL 一意の項目のみ] | 「[!UICONTROL True]」に設定した場合、取得を正常におこなうには、配列内の各項目が一意である必要があります。 |

{style=&quot;table-layout:auto&quot;}

フィールドの設定が完了したら、「**[!UICONTROL 適用]**」を選択して、変更をスキーマに適用します。

![](../../images/ui/fields/special/array-config.png)

キャンバスが更新され、フィールドに加えた変更が反映されます。 キャンバス内のフィールド名の横に表示されるデータ型には、角括弧 (`[]`) で囲まれた一対のデータ型が付加され、そのデータ型の配列を表すフィールドが示されます。

![](../../images/ui/fields/special/array-applied.png)

## 次の手順

このガイドでは、UI で配列フィールドを定義する方法について説明しました。 [!DNL Schema Editor] で他の XDM フィールドタイプを定義する方法については、[UI でのフィールドの定義 ](./overview.md#special) の概要を参照してください。
