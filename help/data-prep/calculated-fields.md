---
keywords: Experience Platform;ホーム;人気のトピック;CSV のマップ;CSV ファイルのマップ;xdm への CSV ファイルのマップ;xdm への CSV のマップ;ui ガイド;マッパー;マッピング;data prep;データ準備;データの準備;
solution: Experience Platform
title: Data Prep の概要
topic-legacy: overview
description: このドキュメントでは、Adobe Experience Platform 内でのData Prep について説明します。
source-git-commit: 3235c48ec1f449e45b3f4b096585b67e14600407
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 19%

---


# 計算フィールド

計算フィールドでは、入力スキーマの属性に基づいて値を作成できます。 これらの値をターゲットスキーマの属性に割り当て、名前と説明を指定して参照を容易にできます。

計算フィールドを作成するには、「**[!UICONTROL 計算フィールドを追加]**」を選択します。

![](./images/calculated-fields/add-calculated-field.png)

**[!UICONTROL 計算済みフィールドを作成]**&#x200B;パネルが表示されます。 左側のダイアログボックスには、計算フィールドでサポートされるフィールド、関数、演算子が含まれています。 タブの1つを選択して、式エディターに関数、フィールドまたは演算子を追加します。

![](./images/calculated-fields/create-calculated-field.png)

| タブ | 説明 |
| --- | ----------- |
| 関数 | 「関数」タブには、データの変換に使用できる関数が一覧表示されます。 計算フィールド内で使用できる関数の詳細については、[データ準備（マッパー）関数](./functions.md)の使用に関するガイドを参照してください。 |
| フィールド | 「フィールド」タブには、ソーススキーマで使用できるフィールドと属性が表示されます。 |
| 演算子 | 「オペレーター」タブには、データの変換に使用できる演算子が一覧表示されます。 |

中央にある式エディターを使用して、フィールド、関数、演算子を手動で追加できます。 式の作成を開始するには、エディターを選択します。

![](./images/calculated-fields/write-calculated-field.png)

「**[!UICONTROL 保存]**」を選択して次に進みます。

マッピング画面が再び表示され、新しく作成したソースフィールドが表示されます。 対応するターゲットフィールドを適用し、「**[!UICONTROL 完了]**」を選択してマッピングを完了します。

![](./images/calculated-fields/new-calculated-field.png)