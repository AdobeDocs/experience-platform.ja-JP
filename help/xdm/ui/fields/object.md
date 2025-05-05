---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；オブジェクト；フィールド；
solution: Experience Platform
title: UI でのオブジェクトフィールドの定義
description: Experience Platformユーザーインターフェイスでオブジェクトタイプフィールドを定義する方法を説明します。
exl-id: 5b7b3cf0-7f11-4e15-af87-09127f4423a5
source-git-commit: b66a50e40aaac8df312a2c9a977fb8d4f1fb0c80
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 0%

---

# UI でのオブジェクトフィールドの定義

Adobe Experience Platformを使用すると、カスタムエクスペリエンスデータモデル（XDM）クラス、スキーマフィールドグループおよびデータタイプの構造を完全にカスタマイズできます。 カスタム XDM リソースの関連フィールドを整理しネストするために、追加のサブフィールドを含めることができるオブジェクトタイプフィールドを定義できます。

Adobe Experience Platform ユーザーインターフェイスで [ 新しいフィールドを定義 ](./overview.md#define) する際に、**[!UICONTROL タイプ]** ドロップダウンを使用し、リストから「[!UICONTROL &#x200B; オブジェクト &#x200B;]」を選択します。

![](../../images/ui/fields/special/object.png)

「**[!UICONTROL 適用]**」を選択して、オブジェクトをスキーマに追加します。 キャンバスが更新され、[!UICONTROL &#x200B; オブジェクト &#x200B;] データタイプが適用された新しいフィールドが表示されます。これには、サブフィールドを編集およびオブジェクトに追加するためのコントロールも含まれます。

![](../../images/ui/fields/special/object-applied.png)

サブフィールドを追加するには、キャンバスのオブジェクトフィールドの横にある **プラス（+）** アイコンを選択します。 オブジェクトの下に新しいフィールドが表示され、右側のパネルにサブフィールドを設定するためのコントロールが表示されます。

![](../../images/ui/fields/special/object-add-field.png)

サブフィールドを設定し、「**[!UICONTROL 適用]** を選択したら、同じプロセスを使用して、引き続きフィールドをオブジェクトに追加できます。 また、オブジェクト自体であるサブフィールドを追加して、フィールドを好きなだけ深くネストすることもできます。

オブジェクトの作成が完了したら、その構造を様々なクラスやフィールドグループで再利用する場合があります。 この場合、オブジェクトをデータタイプに変換することを選択できます。 詳しくは、データタイプ UI ガイドの [ オブジェクトをデータタイプに変換する ](../resources/data-types.md#convert) の節を参照してください。

## 次の手順

このガイドでは、UI でオブジェクトフィールドを定義する方法について説明しました。 [!DNL Schema Editor] で他の XDM フィールドタイプを定義する方法については、[UI でのフィールドの定義 ](./overview.md#special) の概要を参照してください。
