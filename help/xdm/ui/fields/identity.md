---
keywords: Experience Platform；ホーム；人気のあるトピック；API;API;XDM;XDMシステム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；ID；フィールド；
solution: Experience Platform
title: UIでのIDフィールドの定義
description: Experience PlatformユーザーインターフェイスでIDフィールドを定義する方法を説明します。
topic-legacy: user guide
exl-id: 11a53345-4c3f-4537-b3eb-ee7a5952df2a
translation-type: tm+mt
source-git-commit: d425dcd9caf8fccd0cb35e1bac73950a6042a0f8
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 5%

---

# UIでのIDフィールドの定義

Experience Data Model(XDM)では、識別フィールドとは、レコードや時系列のイベントに関連する個々の人を識別するために使用できるフィールドを表します。 このドキュメントでは、Adobe Experience PlatformUIでIDフィールドを定義する方法を説明します。

## 前提条件

「ID」フィールドは、プラットフォームでの顧客IDのグラフの構築方法において重要な要素です。これは、リアルタイム顧客プロファイルが異なるデータフラグメントを結合して顧客の完全な表示を得る方法に最終的に影響します。 スキーマにIDフィールドを定義する前に、次のドキュメントを参照して、IDフィールドに関連する主なサービスと概念を確認してください。

* [Adobe Experience Platform ID サービス](../../../identity-service/home.md)：デバイスやシステム間で ID をブリッジし、準拠する XDM スキーマで定義された ID フィールドに基づいてデータセットをリンクします。
   * [ID名前空間](../../../identity-service/namespaces.md):ID名前空間は、1人の個人に関連付けることができる様々な種類のID情報を定義し、各IDフィールドに必要なコンポーネントです。
* [リアルタイム顧客プロファイル](../../../profile/home.md):顧客IDグラフを活用して、複数のソースからの集計データに基づいて、ほぼリアルタイムで更新された統一された顧客プロファイルを提供します。

## IDフィールドの定義

UIで[新しいフィールド](./overview.md#define)を定義する場合、右側のナビゲーションバーの&#x200B;**[!UICONTROL 「ID」]**&#x200B;チェックボックスを選択して、IDフィールドとして設定できます。

![](../../images/ui/fields/special/identity.png)

チェックボックスを選択すると、追加のコントロールが表示されます。 このフィールドをスキーマのプライマリIDにする場合は、**[!UICONTROL プライマリID]**&#x200B;チェックボックスをオンにします。

>[!NOTE]
>
>1人のスキーマに多数のIDフィールドが定義されていても、1つのプライマリIDしか持つことができません。 すべてのIDフィールド（主要またはその他）は、個々の顧客のIDグラフに使用されますが、リアルタイム顧客プロファイルは、データフラグメントを結合する際に、真実のソースとして主IDのみを使用します。 プロファイルでの使用をスキーマに対して有効にする場合、スキーマでプライマリIDを定義する必要があります。

「**[!UICONTROL ID名前空間]**」で、ドロップダウンメニューを使用してIDフィールドに適切な名前空間を選択します。 Adobeが提供する標準名前空間と、組織が定義するカスタム名前空間が一覧表示されます。

終了したら、「**[!UICONTROL 適用]**」を選択して、変更をスキーマに適用します。

![](../../images/ui/fields/special/identity-config.png)

キャンバスが更新され、変更が反映され、選択したフィールドに指紋記号(![](../../images/ui/fields/special/identity-symbol.png))が付き、IDとして指定されます。 左側のレールで、IDフィールドが、スキーマにフィールドを提供するクラスまたはスキーマフィールドグループの名前の下にリストされます。

すべてのIDフィールドはデフォルトで必須なので、左側のナビゲーションバーの&#x200B;**[!UICONTROL 必須フィールド]**&#x200B;の下にリストされます。 IDフィールドがスキーマ構造内でネストされている場合は、必須フィールドとしてすべての親フィールドも一覧表示されます。

![](../../images/ui/fields/special/identity-applied.png)

スキーマのプライマリIDを定義した場合は、[スキーマを有効にして、リアルタイム顧客プロファイル](../resources/schemas.md#profile)で使用できるようにすることができます。

## 次の手順

このガイドでは、UIでIDフィールドを定義する方法について説明します。 このスキーマを使用してデータが取り込まれると、顧客IDのグラフが更新され、スキーマのIDフィールドが反映されます。 UIで組織のプライベートグラフを参照する方法については、[IDグラフビューア](../../../identity-service/ui/identity-graph-viewer.md)のガイドを参照してください。

[!DNL Schema Editor]で他のXDMフィールドタイプを定義する方法については、[UI](./overview.md#special)でのフィールドの定義の概要を参照してください。
