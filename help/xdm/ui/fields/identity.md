---
keywords: Experience Platform；ホーム；人気のトピック；API;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；ID；フィールド；
solution: Experience Platform
title: UI での ID フィールドの定義
description: Experience Platformユーザーインターフェイスで ID フィールドを定義する方法を説明します。
exl-id: 11a53345-4c3f-4537-b3eb-ee7a5952df2a
source-git-commit: 857c1d4f74b6352e90f9c97ef22d686a883e3563
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 10%

---

# UI での ID フィールドの定義

エクスペリエンスデータモデル (XDM) の ID フィールドは、レコードまたは時系列イベントに関連する個々の人物の識別に使用できるフィールドを表します。 このドキュメントでは、Adobe Experience Platform UI で ID フィールドを定義する方法について説明します。

## 前提条件

ID フィールドは、Platform で顧客 ID グラフを構築する方法に関する重要なコンポーネントで、最終的には、異なるデータフラグメントをリアルタイム顧客プロファイルが結合して顧客の全体像を把握する方法に影響します。 スキーマで ID フィールドを定義する前に、次のドキュメントを参照して、ID フィールドに関する主要なサービスと概念について確認してください。

* [Adobe Experience Platform ID サービス](../../../identity-service/home.md)：デバイスやシステム間で ID をブリッジし、準拠する XDM スキーマで定義された ID フィールドに基づいてデータセットをリンクします。
   * [ID 名前空間](../../../identity-service/namespaces.md)：ID 名前空間は、1 人の人物に関連している可能性のある様々なタイプの ID 情報を定義する、各 ID フィールドに必須のコンポーネントです。
* [リアルタイム顧客プロファイル](../../../profile/home.md):顧客の ID グラフを活用して、ほぼリアルタイムで更新された、複数のソースからの集計データに基づいて統合された消費者プロファイルを提供します。

## ID フィールドの定義 {#define-a-identity-field}

>[!CONTEXTUALHELP]
>id="platform_schemas_identityField_primaryIdentityRestriction"
>title="プライマリ ID に対する制限"
>abstract="このスキーマは、特定のソース接続での使用を目的としたフィールドグループを使用します。 接続では、identityMap をプライマリ ID として使用する必要があり、自動的に設定されています。"

条件 [新しいフィールドの定義](./overview.md#define) UI で、 **[!UICONTROL ID]** 」チェックボックスをオンにします。

![](../../images/ui/fields/special/identity.png)

チェックボックスを選択すると、追加のコントロールが表示されます。 このフィールドをスキーマのプライマリ ID にする場合は、 **[!UICONTROL プライマリID]** チェックボックス。

>[!NOTE]
>
>1 つのスキーマに、多数の ID フィールドが定義されている場合がありますが、1 つのプライマリ ID のみを持つことができます。 すべての ID フィールド（プライマリまたはその他）が個々の顧客の ID グラフに影響しますが、リアルタイム顧客プロファイルは、データフラグメントを結合する際に、プライマリ ID のみを真のソースとして使用します。 プロファイルでスキーマの使用を有効にする場合、スキーマにはプライマリ ID が定義されている必要があります。

の下 **[!UICONTROL ID 名前空間]**」の上にマウスポインターを置き、ドロップダウンメニューを使用して「 id 」フィールドに適した名前空間を選択します。 Adobeが提供する標準名前空間と、組織が定義したカスタム名前空間が一覧表示されます。

終了したら、「 」を選択します。 **[!UICONTROL 適用]** をクリックして、スキーマに変更を適用します。

![](../../images/ui/fields/special/identity-config.png)

キャンバスが更新され、変更が反映されます。選択したフィールドに指紋記号 (![](../../images/ui/fields/special/identity-symbol.png)) をクリックして id として指定します。 左側のレールで、ID フィールドが、スキーマにフィールドを提供するクラスまたはスキーマフィールドグループの名前の下にリストされます。

フィールドもプライマリ ID として設定されていた場合は、その ID もの下に表示されます。 **[!UICONTROL 必須フィールド]** をクリックします。 ID フィールドがスキーマ構造内にネストされている場合、すべての親フィールドも必要に応じて表示されます。

![](../../images/ui/fields/special/identity-applied.png)

スキーマのプライマリ ID を定義した場合は、次に進むことができます。 [リアルタイム顧客プロファイルでのスキーマ使用の有効化](../resources/schemas.md#profile).

## 次の手順

このガイドでは、UI で ID フィールドを定義する方法について説明しました。 このスキーマを使用してデータが取り込まれると、顧客の ID グラフが更新され、スキーマの ID フィールドが反映されます。 詳しくは、 [ID グラフビューア](../../../identity-service/ui/identity-graph-viewer.md) を参照して、UI で組織のプライベートグラフを参照する方法を確認してください。

概要については、 [UI でのフィールドの定義](./overview.md#special) で他の XDM フィールドタイプを定義する方法を学ぶには、以下を実行します。 [!DNL Schema Editor].
