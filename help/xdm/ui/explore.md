---
keywords: Experience Platform；ホーム；人気のトピック；ui;UI;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；データモデル；探索；クラス；フィールドグループ；データタイプ；スキーマ；
solution: Experience Platform
title: UI でのスキーマリソースの調査
description: Experience Platformユーザーインターフェイスで既存のスキーマ、クラス、スキーマフィールドグループ、データ型を調べる方法について説明します。
topic-legacy: tutorial
type: Tutorial
exl-id: b527b2a0-e688-4cfe-a176-282182f252f2
source-git-commit: 744d87c82b7e7e06782c6c1b9db2ec46a5444d28
workflow-type: tm+mt
source-wordcount: '957'
ht-degree: 0%

---

# UI でのスキーマリソースの調査

Adobe Experience Platformでは、すべての Experience Data Model(XDM) スキーマリソースは、 [!DNL Schema Library]：組織が定義したAdobeおよびカスタムリソースによって提供される標準リソースを含みます。 Experience PlatformUI で、 [!DNL Schema Library]. UI は、これらの XDM リソースが提供する各フィールドの期待されるデータタイプと使用例に関する情報を提供するので、データ取得の計画と準備をおこなう際に特に役立ちます。

このチュートリアルでは、Experience PlatformUI の既存のスキーマ、クラス、フィールドグループ、データタイプを調べる手順を説明します。

## スキーマリソースの検索 {#lookup}

Platform UI で、「 **[!UICONTROL スキーマ]** をクリックします。 この [!UICONTROL スキーマ] workspace の **[!UICONTROL 参照]** タブをクリックし、組織内のすべてのスキーマを参照します。また、参照用の専用タブも追加されています。 **[!UICONTROL クラス]**, **[!UICONTROL フィールドグループ]**、および **[!UICONTROL データタイプ]** それぞれ

![](../images/ui/explore/tabs.png)

フィルターアイコン (![フィルターアイコン画像](../images/ui/explore/icon.png)) をクリックすると、左側のレールにコントロールが表示され、リストに表示される結果を絞り込みます。 表示されるコントロールは、表示されるリソースのタイプに応じて異なります。

例えば、リストをフィルターして、Adobeが提供する標準データ型のみを表示するには、「 」を選択します。 **[!UICONTROL データタイプ]** および **[!UICONTROL Adobe]** の下に **[!UICONTROL タイプ]** および **[!UICONTROL 所有者]** 各セクションに割り当てられます。

この **[!UICONTROL プロファイルに含まれる]** toggle を使用すると、結果をフィルタリングして、で使用可能になっているスキーマで使用されているリソースのみを表示できます [リアルタイム顧客プロファイル](../../profile/home.md).

![](../images/ui/explore/filter.png)

リソースを **[!UICONTROL クラス]**, **[!UICONTROL フィールドグループ]**&#x200B;または **[!UICONTROL データタイプ]** タブ、 **[!UICONTROL Adobe]** 標準のリソースのみを表示するか、 **[!UICONTROL 顧客]** ：組織が作成したリソースのみを表示します。

![](../images/ui/explore/filter-data-type.png)

検索バーを使用して、結果をさらに絞り込むこともできます。

![](../images/ui/explore/search.png)

検索結果に表示されるリソースは、タイトルの一致順に並べられ、次に説明の一致順に並べられます。 次に、これらのカテゴリに一致する単語が多いほど、リソースがリストに表示されます。

参照するリソースが見つかったら、リストからその名前を選択して、キャンバスに構造を表示します。

## キャンバスでの XDM リソースの参照 {#explore}

リソースを選択すると、その構造がキャンバスに表示されます。

![](../images/ui/explore/canvas.png)

サブプロパティを含むすべてのオブジェクトタイプフィールドは、最初にキャンバスに表示されたときに、デフォルトで折りたたまれます。 任意のフィールドのサブプロパティを表示するには、名前の横にあるアイコンを選択します。

![](../images/ui/explore/field-expand.png)

### システム生成フィールド {#system-fields}

一部のフィールド名の前には、アンダースコアが付いています ( 例： `_repo` および `_id`. これらは、データの取り込み時にシステムが自動的に生成し、割り当てるフィールドのプレースホルダーを表します。

したがって、これらのフィールドのほとんどは、Platform に取り込む際に、データの構造から除外する必要があります。 このルールの主な例外は、 [`_{TENANT_ID}` フィールド](../api/getting-started.md#know-your-tenant_id)：組織の下で作成するすべての XDM フィールドは、の下に名前空間化する必要があります。

### データタイプ {#data-types}

キャンバスに表示される各フィールドについて、名前の横に対応するデータ型が表示され、フィールドの取得に必要なデータのタイプが一目で示されます。

![](../images/ui/explore/data-types.png)

角括弧 (`[]`) は、特定のデータ型の配列を表します。 例えば、 **[!UICONTROL 文字列]\[]** は、フィールドに文字列値の配列が必要であることを示します。 データタイプ **[!UICONTROL 支払い項目]\[]** は、 [!UICONTROL 支払い項目] データタイプ。

配列フィールドがオブジェクトタイプに基づいている場合、キャンバスでそのアイコンを選択して、各配列項目に対して期待される属性を表示できます。

![](../images/ui/explore/array-type.png)

### [!UICONTROL フィールドプロパティ] {#field-properties}

キャンバスで任意のフィールドの名前を選択すると、右側のレールが更新され、そのフィールドの詳細が以下に表示されます **[!UICONTROL フィールドプロパティ]**. これには、フィールドの意図した使用例の説明、デフォルト値、パターン、形式、必須フィールドかどうかなどが含まれます。

![](../images/ui/explore/field-properties.png)

検査中のフィールドが列挙フィールドの場合、右側のレールには、フィールドが受け取ると想定される有効な値も表示されます。

![](../images/ui/explore/enum-field.png)

### ID フィールド {#identity}

ID フィールドを含むスキーマを検査する場合、これらのフィールドは、スキーマに提供するクラスまたはフィールドグループの下の左側のレールに表示されます。 左側のレールで ID フィールド名を選択して、フィールドがネストされている深さに関係なく、キャンバスにフィールドを表示します。

キャンバス上で ID フィールドが指紋アイコン (![指紋アイコン画像](../images/ui/explore/identity-symbol.png)) をクリックします。 ID フィールドの名前を選択すると、 [id 名前空間](../../identity-service/namespaces.md) フィールドがスキーマのプライマリ ID かどうかを示します。

![](../images/ui/explore/identity-field.png)

>[!NOTE]
>
>詳しくは、 [ID フィールドの定義](./fields/identity.md) id フィールドとダウンストリームの Platform サービスとの関係に関する詳細。

### 関係フィールド {#relationship}

関係フィールドを含むスキーマを調べる場合、フィールドは左側のレールの下に一覧表示されます。 **[!UICONTROL 関係]**. 左側のレールで関係フィールド名を選択して、フィールドがネストされている深さに関係なく、キャンバスにフィールドを表示します。

また、関係フィールドはキャンバスで一意にハイライト表示され、フィールドが参照する宛先スキーマの名前が表示されます。 関係フィールドの名前を選択した場合は、宛先スキーマのプライマリ ID の ID 名前空間を右側のパネルに表示できます。

![](../images/ui/explore/relationship-field.png)

>[!NOTE]
>
>に関するチュートリアルを参照してください。 [UI での関係の作成](../tutorials/relationship-ui.md) を参照してください。

## 次の手順

このドキュメントでは、Experience PlatformUI で既存の XDM リソースを参照する方法を説明しました。 の様々な機能の詳細 [!UICONTROL スキーマ] ワークスペースと [!DNL Schema Editor]を参照し、 [[!UICONTROL スキーマ] workspace の概要](./overview.md).
