---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；id；フィールド；
solution: Experience Platform
title: UI での ID フィールドの定義
description: Experience Platform ユーザーインターフェイスで ID フィールドを定義する方法を説明します。
exl-id: 11a53345-4c3f-4537-b3eb-ee7a5952df2a
source-git-commit: 3570197ca6cff95368b4facb034386e793033fe2
workflow-type: tm+mt
source-wordcount: '657'
ht-degree: 15%

---

# UI での ID フィールドの定義

エクスペリエンスデータモデル（XDM）では、ID フィールドは、レコードまたは時系列イベントに関連する個々のユーザーを識別するために使用できるフィールドを表します。 このドキュメントでは、Adobe Experience Platform UI で ID フィールドを定義する方法について説明します。

## 前提条件

ID フィールドは、Experience Platformでの顧客 ID グラフの作成方法において重要なコンポーネントです。これは、最終的に、リアルタイム顧客プロファイルが様々なデータフラグメントを結合して顧客の全体像を把握する方法に影響します。 スキーマで ID フィールドを定義する前に、次のドキュメントを参照して、ID フィールドに関連する主要なサービスと概念について確認してください。

* [Adobe Experience Platform ID サービス](../../../identity-service/home.md)：デバイスやシステム間で ID をブリッジし、準拠する XDM スキーマで定義された ID フィールドに基づいてデータセットをリンクします。
   * [ID 名前空間](../../../identity-service/features/namespaces.md)：ID 名前空間は、1 人の人物に関連している可能性のある様々なタイプの ID 情報を定義する、各 ID フィールドに必須のコンポーネントです。
* [ リアルタイム顧客プロファイル ](../../../profile/home.md)：顧客 ID グラフを活用して、ほぼリアルタイムで更新される、複数のソースからの集計データに基づいて統合された消費者プロファイルを提供します。

## ID フィールドを定義 {#define-a-identity-field}

>[!CONTEXTUALHELP]
>id="platform_schemas_identityField_primaryIdentityRestriction"
>title="プライマリ ID に対する制限"
>abstract="このスキーマは、特定のソース接続で使用するためのフィールドグループを使用します。この接続では、identityMap をプライマリ ID として使用する必要があり、自動的に設定されています。"

UI で [ 新しいフィールドを定義 ](./overview.md#define) する際に、右側のパネルの「**[!UICONTROL Identity]**」チェックボックスを選択して、ID フィールドとして設定できます。

![](../../images/ui/fields/special/identity.png)

チェックボックスを選択すると、追加のコントロールが表示されます。 このフィールドをスキーマのプライマリ ID にする場合は、「**[!UICONTROL Primary identity]**」チェックボックスを選択します。

>[!NOTE]
>
>1 つのスキーマに多数の ID フィールドを定義できますが、持つことができるのは 1 つのプライマリ ID のみです。 すべての ID フィールド（プライマリまたはその他）は、個々の顧客の ID グラフに寄与しますが、リアルタイム顧客プロファイルは、データフラグメントを結合する際にプライマリ ID のみを情報源として使用します。 プロファイルで使用するためにスキーマを有効にする場合、スキーマにはプライマリ ID が定義されている必要があります。

**[!UICONTROL Identity namespace]** の下で、ドロップダウンメニューを使用して、ID フィールドに適した名前空間を選択します。 Adobeが提供する標準名前空間と、組織で定義されたカスタム名前空間が一覧表示されます。

終了したら、「**[!UICONTROL Apply]**」を選択して、スキーマに変更を適用します。

>[!IMPORTANT]
>
>スキーマがリアルタイム顧客プロファイルでの使用に対して有効にされ、データが取り込まれたら、**プライマリ ID フィールドは変更できません**。 これを試みると、検証エラーが発生します。 別のプライマリ ID を使用する必要がある場合は、新しいスキーマと、更新された ID 設定を持つ新しいデータセットを作成する必要があります。

![](../../images/ui/fields/special/identity-config.png)

キャンバスが更新されて変更が反映され、選択したフィールドに ID を示す指紋記号（![](/help/images/icons/identity-service.png)）が表示されます。 左側のパネルで、スキーマにフィールドを提供するクラスまたはスキーマフィールドグループの名前の下に ID フィールドが表示されるようになりました。

フィールドがプライマリ ID としても設定されている場合は、左側のパネルの **[!UICONTROL Required fields]** にも表示されます。 ID フィールドがスキーマ構造内にネストされている場合、すべての親フィールドも必要に応じてリストされます。

![](../../images/ui/fields/special/identity-applied.png)

スキーマのプライマリ ID を定義した場合は、[ リアルタイム顧客プロファイルで使用するスキーマを有効にする ](../resources/schemas.md#profile) に進むことができます。

## 次の手順

このガイドでは、UI で ID フィールドを定義する方法について説明しました。 このスキーマを使用してデータが取り込まれると、顧客 ID グラフが更新され、スキーマの ID フィールドが反映されます。 UI で組織のプライベートグラフを調べる方法については、[ID グラフビューア ](../../../identity-service/features/identity-graph-viewer.md) に関するガイドを参照してください。

[ で他の XDM フィールドタイプを定義する方法については、](./overview.md#special)UI でのフィールドの定義 [!DNL Schema Editor] の概要を参照してください。
