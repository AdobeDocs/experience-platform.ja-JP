---
keywords: Experience Platform；ホーム；人気のあるトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；必須；フィールド；
title: UI での必須フィールドの定義
description: ユーザーインターフェイスで必須の XDM フィールドを定義するExperience Platformを説明します。
exl-id: 3a5885a0-6f07-42f3-b521-053083d5b556
source-git-commit: 1d04bf56c51506f84c5156e6d2ed6c9f58f15235
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 0%

---

# UI での必須フィールドの定義

エクスペリエンスデータモデル (XDM) の必須フィールドは、データの取り込み中に特定のレコードまたは時系列イベントを受け入れるために、有効な値を指定する必要があることを示します。 必須フィールドの一般的な使用例には、ユーザー ID 情報やタイムスタンプが含まれます。

Adobe Experience Platformユーザーインターフェイスで新しいフィールド ](./overview.md#define) を定義する場合、右側のパネルで「**[!UICONTROL 必須]**」チェックボックスを選択して、必須フィールドに設定できます。 [「**[!UICONTROL 適用]**」を選択して、変更をスキーマに適用します。

![必須チェックボックス](../../images/ui/fields/required/root.png)

フィールドがテナント ID オブジェクトの下のルートレベル属性の場合、そのパスはすぐに左側のレールの **[!UICONTROL 必須フィールド]** の下に表示されます。

![ルートレベルの必須フィールド](../../images/ui/fields/required/applied.png)

ただし、必須フィールドが、必須フィールドとしてマークされていないオブジェクト内にネストされている場合は、左側のレールの **[!UICONTROL 必須フィールド]** の下にネストされたフィールドは表示されません。

次の例では、`loyaltyId` フィールドは必要に応じて設定されますが、親オブジェクト `loyalty` は設定されません。 この場合、データの取り込み時に `loyalty` が除外された場合、子フィールド `loyaltyId` が必要としてマークされていても、検証エラーは発生しません。 つまり、 `loyalty` はオプションですが、 `loyaltyId` フィールドを含める場合は、そのフィールドを含める必要があります。

![入れ子の必須フィールド](../../images/ui/fields/required/nested.png)

ネストされたフィールドをスキーマで常に必須にする場合は、すべての親フィールドを必須に設定する必要もあります（テナント ID オブジェクトを除く）。

![親および子の必須フィールド](../../images/ui/fields/required/parent-and-child.png)

## 次の手順

このガイドでは、UI で必須フィールドを定義する方法を説明しました。 [!DNL Schema Editor] で他の XDM フィールドタイプを定義する方法については、[UI でのフィールドの定義 ](./overview.md#special) の概要を参照してください。
