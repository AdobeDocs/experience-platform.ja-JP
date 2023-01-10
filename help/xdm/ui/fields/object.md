---
keywords: Experience Platform；ホーム；人気のトピック；API;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；オブジェクト；フィールド；
solution: Experience Platform
title: UI でのオブジェクトフィールドの定義
description: Experience Platformユーザーインターフェイスでオブジェクトタイプフィールドを定義する方法を説明します。
exl-id: 5b7b3cf0-7f11-4e15-af87-09127f4423a5
source-git-commit: 5caa4c750c9f786626f44c3578272671d85b8425
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---

# UI でのオブジェクトフィールドの定義

Adobe Experience Platformでは、カスタムの Experience Data Model(XDM) クラス、スキーマフィールドグループ、データタイプの構造を完全にカスタマイズできます。 関連するフィールドをカスタム XDM リソースで整理およびネストするには、追加のサブフィールドを含めることができるオブジェクトタイプのフィールドを定義します。

条件 [新しいフィールドの定義](./overview.md#define) Adobe Experience Platformユーザーインターフェイスで、 **[!UICONTROL タイプ]** ドロップダウンと選択[!UICONTROL オブジェクト]」をリストから削除します。

![](../../images/ui/fields/special/object.png)

選択 **[!UICONTROL 適用]** をクリックして、スキーマにオブジェクトを追加します。 キャンバスが更新され、新しいフィールドが [!UICONTROL オブジェクト] サブフィールドを編集し、オブジェクトに追加するコントロールなど、適用されるデータタイプ。

![](../../images/ui/fields/special/object-applied.png)

サブフィールドを追加するには、 **プラス (+)** アイコンをクリックします。 オブジェクトの下に新しいフィールドが表示され、右側のレールでサブフィールドを設定するためのコントロールが表示されます。

![](../../images/ui/fields/special/object-add-field.png)

サブフィールドを設定し、「 」を選択したら、 **[!UICONTROL 適用]**&#x200B;を使用すると、同じ処理を使用して引き続きフィールドをオブジェクトに追加できます。 また、オブジェクト自体であるサブフィールドを追加して、必要に応じてフィールドをネストすることもできます。

オブジェクトの作成が完了したら、別のクラスやフィールドグループでその構造を再利用する場合があります。 この場合、オブジェクトをデータ型に変換できます。 詳しくは、 [変換，オブジェクトをデータ型に](../resources/data-types.md#convert) （データタイプ UI ガイド）を参照してください。

## 次の手順

このガイドでは、UI でオブジェクトフィールドを定義する方法について説明しました。 概要については、 [UI でのフィールドの定義](./overview.md#special) で他の XDM フィールドタイプを定義する方法を学ぶには、以下を実行します。 [!DNL Schema Editor].
