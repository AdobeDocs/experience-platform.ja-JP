---
keywords: Experience Platform；ホーム；人気のあるトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；オブジェクト；フィールド；
solution: Experience Platform
title: UI でのオブジェクトフィールドの定義
description: Experience Platformユーザーインターフェイスでオブジェクトタイプフィールドを定義する方法を説明します。
topic-legacy: user guide
exl-id: 5b7b3cf0-7f11-4e15-af87-09127f4423a5
source-git-commit: d425dcd9caf8fccd0cb35e1bac73950a6042a0f8
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---

# UI でのオブジェクトフィールドの定義

Adobe Experience Platformでは、カスタムの Experience Data Model(XDM) クラス、スキーマフィールドグループおよびデータ型の構造を完全にカスタマイズできます。 関連するフィールドをカスタム XDM リソースに整理してネストするには、追加のサブフィールドを含めることができるオブジェクトタイプのフィールドを定義します。

Adobe Experience Platformユーザーインターフェイスで新しいフィールド ](./overview.md#define) を定義する場合は、**[!UICONTROL タイプ]** ドロップダウンを使用し、リストから「[!UICONTROL  オブジェクト ]」を選択します。[

![](../../images/ui/fields/special/object.png)

「**[!UICONTROL 適用]**」を選択して、オブジェクトをスキーマに追加します。 キャンバスが更新され、[!UICONTROL Object] データ型が適用された新しいフィールドが表示されます。このデータ型には、サブフィールドを編集したり、オブジェクトに追加したりするためのコントロールが含まれます。

![](../../images/ui/fields/special/object-applied.png)

サブフィールドを追加するには、キャンバスのオブジェクトフィールドの横にある **プラス (+)** アイコンを選択します。 オブジェクトの下に新しいフィールドが表示され、右側のパネルでサブフィールドを設定するためのコントロールが表示されます。

![](../../images/ui/fields/special/object-add-field.png)

サブフィールドを設定し、「**[!UICONTROL 適用]**」を選択したら、同じ手順で引き続きフィールドをオブジェクトに追加できます。 また、オブジェクト自体であるサブフィールドを追加して、フィールドを好きなだけネストすることもできます。

![](../../images/ui/fields/special/object-nested.png)

オブジェクトの作成が完了したら、異なるクラスやフィールドグループで構造を再利用することができます。 この場合、オブジェクトをデータ型に変換できます。 詳しくは、データ型 UI ガイドの [ オブジェクトからデータ型 ](../resources/data-types.md#convert) への変換に関する節を参照してください。

## 次の手順

このガイドでは、UI でオブジェクトフィールドを定義する方法について説明しました。 [!DNL Schema Editor] で他の XDM フィールドタイプを定義する方法については、[UI でのフィールドの定義 ](./overview.md#special) の概要を参照してください。
