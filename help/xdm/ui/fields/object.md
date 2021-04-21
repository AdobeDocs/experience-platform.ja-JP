---
keywords: Experience Platform；ホーム；人気のあるトピック；API;API;XDM;XDMシステム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；オブジェクト；フィールド；
solution: Experience Platform
title: UIでのオブジェクトフィールドの定義
description: Experience Platformユーザーインターフェイスでオブジェクトタイプフィールドを定義する方法を説明します。
topic-legacy: user guide
exl-id: 5b7b3cf0-7f11-4e15-af87-09127f4423a5
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '306'
ht-degree: 0%

---

# UIでのオブジェクトフィールドの定義

Adobe Experience Platformでは、カスタムのExperience Data Model(XDM)クラス、ミックスイン、データ型の構造を完全にカスタマイズできます。 カスタムXDMリソース内の関連するフィールドを整理してネストするために、追加のサブフィールドを含めることができるオブジェクト型フィールドを定義できます。

Adobe Experience Platformユーザーインターフェイスで[新しいフィールド](./overview.md#define)を定義する場合は、**[!UICONTROL タイプ]**&#x200B;ドロップダウンを使用し、リストから「[!UICONTROL オブジェクト]」を選択します。

![](../../images/ui/fields/special/object.png)

「**[!UICONTROL 適用]**」を選択して、オブジェクトをスキーマに追加します。 キャンバスが更新され、オブジェクトにサブフィールドを編集および追加するコントロールなど、[!UICONTROL Object]データ型が適用された新しいフィールドが表示されます。

![](../../images/ui/fields/special/object-applied.png)

サブフィールドを追加するには、キャンバスのオブジェクトフィールドの横にある&#x200B;**プラス(+)**&#x200B;アイコンを選択します。 オブジェクトの下に新しいフィールドが表示され、右側のレールでサブフィールドを設定するためのコントロールが表示されます。

![](../../images/ui/fields/special/object-add-field.png)

サブフィールドを設定し、「**[!UICONTROL 適用]**」を選択したら、同じプロセスを使用して引き続きオブジェクトにフィールドを追加できます。 また、オブジェクトであるサブフィールドを追加して、フィールドを必要に応じてネストすることもできます。

![](../../images/ui/fields/special/object-nested.png)

オブジェクトの構築が完了したら、異なるクラスやミックスインでその構造を再利用したい場合があります。 この場合、オブジェクトをデータ型に変換できます。 詳しくは、『データタイプUI』ガイドの[データタイプへのオブジェクトの変換](../resources/data-types.md#convert)に関する節を参照してください。

## 次の手順

このガイドでは、UIでオブジェクトフィールドを定義する方法について説明します。 [!DNL Schema Editor]で他のXDMフィールドタイプを定義する方法については、[UI](./overview.md#special)でのフィールドの定義の概要を参照してください。
