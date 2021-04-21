---
keywords: Experience Platform；ホーム；人気のあるトピック；API;API;XDM;XDMシステム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；配列；フィールド；
solution: Experience Platform
title: UIでの配列フィールドの定義
description: Experience Platformユーザーインターフェイスで配列フィールドを定義する方法を説明します。
topic-legacy: user guide
exl-id: 9ac55554-c29b-40b2-9987-c8c17cc2c00c
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 1%

---

# UIでの配列フィールドの定義

Adobe Experience PlatformユーザーインターフェイスでExperience Data Model(XDM)フィールドを定義する場合、そのフィールドを配列として指定できます。

配列の内容は、そのフィールドで選択された[!UICONTROL Type]によって異なります。 例えば、フィールドの[!UICONTROL Type]が&quot;[!UICONTROL String]&quot;に設定されている場合、そのフィールドを配列として設定すると、フィールドが文字列の配列として指定されます。 フィールドの[!UICONTROL Type]が「[!UICONTROL 住所]」などの複数フィールドのデータ型に設定されると、そのデータ型に適合する住所オブジェクトの配列になります。

UI](./overview.md#define)で[新しいフィールドを定義した後、右側のナビゲーションバーの&#x200B;**[!UICONTROL Array]**&#x200B;チェックボックスを選択して、配列フィールドに設定できます。

![](../../images/ui/fields/special/array.png)

チェックボックスを選択すると、右側のレールに追加のコントロールが表示され、必要に応じて配列をさらに拘束できます。 特定の制約を適用しない場合は、このフィールドを空欄にします。

配列の追加の設定コントロールは次のとおりです。

| Fieldプロパティ | 説明 |
| --- | --- |
| [!UICONTROL 最小長] | 取り込みを正常に行うためにアレイに含める必要がある最小項目数。 |
| [!UICONTROL 最大長] | 取り込みを正常に行うためにアレイに含める必要がある最大アイテム数。 |
| [!UICONTROL 一意の項目のみ] | &quot;[!UICONTROL True]&quot;に設定した場合、取り込みを正常に行うためには、アレイ内の各項目が一意である必要があります。 |

フィールドの設定が完了したら、「**[!UICONTROL 適用]**」を選択して、変更をスキーマに適用します。

![](../../images/ui/fields/special/array-config.png)

カンバスが更新され、フィールドに対して行われた変更が反映されます。 キャンバス内のフィールド名の横に表示されるデータタイプには、角括弧(`[]`)のペアが付いています。これは、そのデータタイプの配列を表すフィールドであることを示しています。

![](../../images/ui/fields/special/array-applied.png)

## 次の手順

このガイドでは、UIで配列フィールドを定義する方法を説明します。 [!DNL Schema Editor]で他のXDMフィールドタイプを定義する方法については、[UI](./overview.md#special)でのフィールドの定義の概要を参照してください。
