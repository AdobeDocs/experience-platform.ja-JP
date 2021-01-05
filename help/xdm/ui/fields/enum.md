---
keywords: Experience Platform;home;popular topics;api;API;XDM;XDM system;experience data model;data model;ui;workspace;enum;field;
solution: Experience Platform
title: UIで列挙フィールドを定義する
description: Experience Platformユーザーインターフェイスで列挙フィールドを定義する方法を説明します。
topic: user guide
translation-type: tm+mt
source-git-commit: 2e20403122e65d28f04114af9b7e8d41874f76e2
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---


# UIで列挙フィールドを定義する

エクスペリエンスデータモデル(XDM)では、列挙フィールドは、許容値の事前定義されたリストに制限されるフィールドを表します。

Adobe Experience Platformユーザーインターフェイスで[新しいフィールド](./overview.md#define)を定義する場合、右側のレールで&#x200B;**[!UICONTROL Enum]**&#x200B;チェックボックスを選択して、それを列挙フィールドとして設定できます。

![](../../images/ui/fields/special/enum.png)

このチェックボックスを選択すると、追加のコントロールが表示され、列挙の値制約を指定できます。 **[!UICONTROL Value]**&#x200B;列の下で、フィールドに制約する正確な値を指定する必要があります。 この値は、列挙型フィールドに選択した[!UICONTROL 型]に準拠する必要があります。 オプションで、制約に人間にわかりやすい&#x200B;**[!UICONTROL ラベル]**&#x200B;を指定することもできます。

列挙に制約を追加するには、**[!UICONTROL 追加行]**&#x200B;を選択します。

![](../../images/ui/fields/special/enum-add-row.png)

必要な制約とオプションのラベルを列挙型に追加します。 終了したら、「**[!UICONTROL 適用]**」を選択して、変更をスキーマに適用します。

![](../../images/ui/fields/special/enum-configured.png)

キャンバスが更新され、変更が反映されます。 このスキーマを今後調査する場合は、右側のレール内の列挙フィールドの制約を表示および編集できます。

![](../../images/ui/fields/special/enum-applied.png)

## 次の手順

このガイドでは、UIで列挙型フィールドを定義する方法を説明します。 [!DNL Schema Editor]で他のXDMフィールドタイプを定義する方法については、[UI](./overview.md#special)でのフィールドの定義の概要を参照してください。