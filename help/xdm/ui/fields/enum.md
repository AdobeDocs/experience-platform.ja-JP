---
keywords: Experience Platform；ホーム；人気のあるトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；列挙；フィールド；
solution: Experience Platform
title: UI での列挙フィールドの定義
description: ユーザーインターフェイスで enum フィールドを定義するExperience Platformについて説明します。
topic-legacy: user guide
exl-id: 67ec5382-31de-4f8d-9618-e8919bb5a472
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '251'
ht-degree: 0%

---

# UI での列挙フィールドの定義

エクスペリエンスデータモデル (XDM) の列挙フィールドは、許容可能な値の事前定義済みのリストに制限されるフィールドを表します。

Adobe Experience Platformユーザーインターフェイスで新しいフィールド ](./overview.md#define) を定義する場合は、右側のパネルで「**[!UICONTROL Enum]**」チェックボックスを選択して、列挙フィールドとして設定できます。[

![](../../images/ui/fields/special/enum.png)

このチェックボックスを選択すると、追加のコントロールが表示され、列挙の値の制約を指定できます。 **[!UICONTROL 値]** 列で、フィールドの制約先となる正確な値を指定する必要があります。 この値は、列挙フィールドに選択した [!UICONTROL  型 ] に従う必要があります。 オプションで、人にわかりやすい制約の **[!UICONTROL ラベル]** を指定することもできます。

列挙に制約を追加するには、「**[!UICONTROL 行を追加]**」を選択します。

![](../../images/ui/fields/special/enum-add-row.png)

引き続き、必要な制約とオプションのラベルを列挙に追加します。 終了したら、「**[!UICONTROL 適用]**」を選択して、変更をスキーマに適用します。

![](../../images/ui/fields/special/enum-configured.png)

キャンバスが更新され、変更が反映されます。 このスキーマを将来調査する際は、右側のパネル内の列挙フィールドの制約を表示して編集できます。

![](../../images/ui/fields/special/enum-applied.png)

## 次の手順

このガイドでは、UI で列挙フィールドを定義する方法について説明しました。 [!DNL Schema Editor] で他の XDM フィールドタイプを定義する方法については、[UI でのフィールドの定義 ](./overview.md#special) の概要を参照してください。
