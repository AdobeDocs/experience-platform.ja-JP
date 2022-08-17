---
keywords: Experience Platform；ホーム；人気のトピック；API;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；列挙；フィールド；
solution: Experience Platform
title: UI での列挙フィールドの定義
description: Experience Platformユーザーインターフェイスで列挙フィールドを定義する方法を説明します。
topic-legacy: user guide
exl-id: 67ec5382-31de-4f8d-9618-e8919bb5a472
source-git-commit: f6fefda974d2ae6fd4b035ef3b5fe633311c9772
workflow-type: tm+mt
source-wordcount: '310'
ht-degree: 2%

---

# UI での列挙フィールドの定義 {#enum}

>[!CONTEXTUALHELP]
>id="platform_xdm_enumsuggestedvalue"
>title="列挙と推奨値"
>abstract="列挙は、文字列フィールドを制限し、事前定義された値のセットと一致するデータのみを取り込めるようにします。 または、取り込みを制限せずに、セグメント化で選択できる属性を定義して、フィールドに提案される値のセットを定義することもできます。 詳しくは、ドキュメントを参照してください。"

エクスペリエンスデータモデル (XDM) では、列挙フィールドは、許容可能な値の事前定義済みリストに制限されるフィールドを表します。

条件 [新しいフィールドの定義](./overview.md#define) Adobe Experience Platformユーザーインターフェイスで、「 **[!UICONTROL Enum]** 」チェックボックスをオンにします。

![](../../images/ui/fields/special/enum.png)

このチェックボックスを選択すると、追加のコントロールが表示され、列挙の値の制約を指定できます。 以下 **[!UICONTROL 値]** 」列には、フィールドを制限する正確な値を指定する必要があります。 この値は、 [!UICONTROL タイプ] 列挙フィールドに対して選択した値です。 オプションで、人間にやさしい **[!UICONTROL ラベル]** 制約も同様です。

列挙に制約を追加するには、「 **[!UICONTROL 行を追加]**.

![](../../images/ui/fields/special/enum-add-row.png)

引き続き、目的の制約とオプションのラベルを列挙に追加します。 終了したら、「 」を選択します。 **[!UICONTROL 適用]** をクリックして、変更をスキーマに適用します。

![](../../images/ui/fields/special/enum-configured.png)

キャンバスが更新され、変更が反映されます。 このスキーマを将来調査する際は、右側のパネル内の列挙フィールドの制約を表示および編集できます。

![](../../images/ui/fields/special/enum-applied.png)

## 次の手順

このガイドでは、UI で列挙フィールドを定義する方法について説明しました。 概要については、 [UI でのフィールドの定義](./overview.md#special) で他の XDM フィールドタイプを定義する方法を学ぶには、以下を実行します。 [!DNL Schema Editor].
