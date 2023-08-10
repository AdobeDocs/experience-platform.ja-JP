---
keywords: Experience Platform；ホーム；人気のトピック；API;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；必須；フィールド；
title: UI での必須フィールドの定義
description: Experience Platformユーザーインターフェイスで必須の XDM フィールドを定義する方法を説明します。
exl-id: 3a5885a0-6f07-42f3-b521-053083d5b556
source-git-commit: b66a50e40aaac8df312a2c9a977fb8d4f1fb0c80
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# UI での必須フィールドの定義

Experience Data Model(XDM) の必須フィールドは、データの取り込み中に特定のレコードまたは時系列イベントを受け入れるために、有効な値を指定する必要があることを示します。 必須フィールドの一般的な使用例には、ユーザー ID 情報やタイムスタンプが含まれます。

>[!IMPORTANT]
>
>スキーマフィールドが必須かどうかに関係なく、Platform は受け入れません `null` 取り込まれたフィールドの空の値。 レコードまたはイベントの特定のフィールドに値がない場合、そのフィールドのキーを取り込みペイロードから除外する必要があります。

条件 [新しいフィールドの定義](./overview.md#define) Adobe Experience Platformユーザーインターフェイスで、「 **[!UICONTROL 必須]** 」チェックボックスをオンにします。 選択 **[!UICONTROL 適用]** をクリックして、スキーマに変更を適用します。

![必須チェックボックス](../../images/ui/fields/required/root.png)

フィールドがテナント ID オブジェクトの下のルートレベル属性の場合、そのパスはすぐにの下に表示されます。 **[!UICONTROL 必須フィールド]** をクリックします。

![ルートレベルの必須フィールド](../../images/ui/fields/required/applied.png)

ただし、必須フィールドが必須とマークされていないオブジェクト内にネストされている場合は、ネストされたフィールドはの下に表示されません。 **[!UICONTROL 必須フィールド]** をクリックします。

次の例では、 `internalSKU` フィールドは必須として設定されていますが、親オブジェクトです `SKUs` がではありません。 この場合、 `SKUs` は、データの取り込み時に子フィールドを含めても除外されます `internalSKU` が必要としてマークされています。 言い換えれば、 `SKUs` オプションの場合は、次を含める必要があります。 `internalSKU` フィールドに含まれるイベントのフィールド。

![入れ子の必須フィールド](../../images/ui/fields/required/nested.png)

スキーマ内でネストされたフィールドを常に必須にする場合は、すべての親フィールドを必須に設定する必要もあります（テナント ID オブジェクトを除く）。

![親および子の必須フィールド](../../images/ui/fields/required/parent-and-child.png)

## 次の手順

このガイドでは、UI で必須フィールドを定義する方法を説明しました。 概要については、 [UI でのフィールドの定義](./overview.md#special) で他の XDM フィールドタイプを定義する方法を学ぶには、以下を実行します。 [!DNL Schema Editor].
