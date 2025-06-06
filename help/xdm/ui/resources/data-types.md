---
keywords: Experience Platform；ホーム；人気のトピック；ui;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；スキーマ；スキーマ；作成；データタイプ；データタイプ；
solution: Experience Platform
title: UI を使用したデータタイプの作成と編集
type: Tutorial
description: Experience Platform ユーザーインターフェイスでデータタイプを作成および編集する方法について説明します。
exl-id: 2c917154-c425-463c-b8c8-04ba37d9247b
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1387'
ht-degree: 6%

---

# UI を使用したデータタイプの作成と編集 {#ui-create-and-edit}

>[!CONTEXTUALHELP]
>id="platform_schemas_datatype_filter"
>title="標準またはカスタムのデータタイプフィルター"
>abstract="使用可能なデータタイプのリストは、作成方法に基づいて事前にフィルタリングされています。ラジオボタンを選択して、「標準」オプションと「カスタム」オプションのいずれかを選択します。「標準」オプションは、アドビが作成したエンティティを表示し、「カスタム」オプションは、組織内で作成したエンティティを表示します。データタイプの作成と編集について詳しくは、ドキュメントを参照してください。"

エクスペリエンスデータモデル（XDM）では、データタイプは、複数のサブフィールドを含む再利用可能なフィールドです。 複数フィールド構造を一貫して使用できるという点では、スキーマフィールドグループに似ていますが、データタイプは、スキーマ構造の任意の場所に含めることができるのに対して、フィールドグループはルートレベルでのみ追加できるので、より柔軟です。

Adobe Experience Platformには、一般的なエクスペリエンス管理の様々なユースケースに対応するために使用できる、多くの標準データタイプが用意されています。 ただし、独自のビジネスニーズに対応するために、独自のカスタムデータタイプを定義することもできます。

>[!NOTE]
>
>フィールドが特定のデータタイプとして定義されている場合、同じフィールドを別のスキーマの異なるデータタイプで作成することはできません。 この制約は、組織のテナントに適用されます。

このチュートリアルでは、Experience Platform ユーザーインターフェイスでカスタムデータタイプを作成および編集する手順を説明します。

## 前提条件 {#prerequisites}

このガイドでは、XDM システムに関する十分な知識が必要です。 Experience Platform エコシステムでの XDM の役割の概要については [XDM の概要 ](../../home.md) を、データタイプが XDM スキーマにどのように寄与するかについては [ スキーマ構成の基本 ](../../schema/composition.md) を参照してください。

このガイドには必要ありませんが、[UI でのスキーマの作成 ](../../tutorials/create-schema-ui.md) に関するチュートリアルに従って、[!DNL Schema Editor] の様々な機能を理解することをお勧めします。

## データタイプの [!DNL Schema Editor] を開く {#data-type}

Experience Platform UI で、左側のナビゲーションで「**[!UICONTROL スキーマ]**」を選択して [!UICONTROL &#x200B; スキーマ &#x200B;] ワークスペースを開き、「**[!UICONTROL データタイプ]**」タブを選択します。 使用可能なデータタイプのリストが表示されます。 データタイプのリストは、作成方法に基づいて自動的にフィルタリングされます。 デフォルト設定には、Adobeで定義されたデータタイプが表示されます。 また、リストをフィルタリングして、組織で作成したリストを表示することもできます。

![ 左側のナビゲーションで [!UICONTROL &#x200B; スキーマ &#x200B;] がハイライト表示され ][!UICONTROL &#x200B; データタイプ &#x200B;] を含む [[!UICONTROL &#x200B; スキーマ ワークスペース。]](../../images/ui/resources/data-types/data-types-tab.png)

ここから、次のオプションを使用できます。

- [新しいデータタイプの作成](#create)
- [データタイプのフィルタリング](#filter)
- [編集する既存のデータタイプを選択](#edit)

### 新しいデータタイプの作成 {#create}

**[!UICONTROL データタイプ]** タブから、「**[!UICONTROL データタイプを作成]**」を選択します。

![[!UICONTROL &#x200B; データタイプを作成 &#x200B;] がハイライト表示された [!UICONTROL &#x200B; スキーマ &#x200B;] ワークスペース [!UICONTROL &#x200B; データタイプ &#x200B;] タブ ](../../images/ui/resources/data-types/create.png)

[!DNL Schema Editor] が表示され、キャンバスに新しいデータタイプの現在の構造が表示されます。 エディターの右側で、データタイプの表示名と説明（オプション）を入力できます。 スキーマに追加する際にデータタイプを識別する方法と同様に、データタイプに一意で簡潔な名前を指定します。

このチュートリアルでは、レストランプロパティを説明するデータタイプを作成するので、データタイプには「Restaurant」という表示名が付きます。

![](../../images/ui/resources/data-types/data-type-properties.png)

ここから、[ 次のセクション ](#add-fields) に進んで、新しいデータタイプへのフィールドの追加を開始できます。

### データタイプのフィルタリング {#filter}

使用可能なデータタイプのリストは、作成方法に基づいて事前にフィルタリングされています。ラジオボタンを選択して、「[!UICONTROL &#x200B; 標準 &#x200B;]」オプションと「[!UICONTROL &#x200B; カスタム &#x200B;] オプションの中から選択します。 「[!UICONTROL &#x200B; 標準 &#x200B;]」オプションにはAdobeで作成されたエンティティが表示され、「[!UICONTROL &#x200B; カスタム &#x200B;]」オプションには組織内で作成されたエンティティが表示されます。

![[!UICONTROL &#x200B; 標準 &#x200B;] と [!UICONTROL &#x200B; カスタム &#x200B;] がハイライト表示された [!UICONTROL &#x200B; スキーマ &#x200B;] ワークスペースの [!UICONTROL &#x200B; データタイプ &#x200B;] タブ ](../../images/ui/resources/data-types/standard-and-custom-data-types.png)

### 既存のデータタイプの編集 {#edit}

>[!NOTE]
>
>リアルタイム顧客プロファイルで使用できるように設定されているスキーマで既存のデータタイプを使用すると、以降はそのデータタイプに非破壊的な変更のみを加えることができます。 詳しくは、[ スキーマ進化のルール ](../../schema/composition.md#evolution) を参照してください。

編集できるのは、組織で定義されたカスタムデータタイプのみです。 組織が所有するカスタムデータタイプのみを表示する場合は、「**[!UICONTROL カスタム]**」を選択します。

編集するデータタイプをリストから選択して右側のパネルを開き、データタイプの詳細を表示します。 詳細パネルから、サンプルファイルをダウンロードしたり、JSON 構造をコピーしたり、データタイプをパッケージに追加したりすることもできます。

右側のパネルでデータタイプの名前を選択して、[!DNL Schema Editor] で構造を開きます。

![ データタイプ [!UICONTROL &#x200B; カスタム &#x200B;] とデータタイプ ] 名前 [[!UICONTROL &#x200B; がハイライト表示された [!UICONTROL &#x200B; スキーマ &#x200B;] ワークスペースの [!UICONTROL &#x200B; データタイプ &#x200B;] タブ &#x200B;]](../../images/ui/resources/data-types/edit.png)

## データタイプへのフィールドの追加 {#add-fields}

データタイプへのフィールドの追加を開始するには、キャンバスのルートレベルフィールドの横にある **プラス（+）** アイコンを選択します。 下に新しいフィールドが表示され、右側のパネルが更新されて、新しいフィールドのコントロールが表示されます。

![](../../images/ui/resources/data-types/new-field.png)

右側のパネルのコントロールを使用して、新しいフィールドの詳細を設定します。 フィールドを設定してデータタイプに追加する方法に関する具体的な手順については、[UI でのフィールドの定義 ](../fields/overview.md#define) に関するガイドを参照してください。

レストラン データタイプには、レストランの名前を表す文字列フィールドが必要です。 そのため、[!UICONTROL &#x200B; フィールド名 &#x200B;] は「名前」に設定され、[!UICONTROL &#x200B; タイプ &#x200B;] は「[!UICONTROL &#x200B; 文字列 &#x200B;]」に設定されます。 「**[!UICONTROL 適用]**」を選択して、フィールドに変更を適用します。

![](../../images/ui/resources/data-types/name-field.png)

必要に応じて、データタイプにさらにフィールドを追加します。 レストラン データタイプの例には、ブランド、座席数およびフロアスペース用の追加フィールドが含まれるようになりました。

![](../../images/ui/resources/data-types/more-fields.png)

基本フィールドに加えて、カスタムデータタイプ内に追加のデータタイプをネストすることもできます。 例えば、Restaurant データタイプには、プロパティの物理アドレスを表すフィールドが必要です。 このシナリオでは、標準のデータタイプ「[!UICONTROL &#x200B; 郵送先住所 &#x200B;]」が割り当てられた新しい「住所」フィールドを追加できます。

![](../../images/ui/resources/data-types/address-field.png)

これは、データ型がデータを記述する際にどのように柔軟かを示しています。データ型は、フィールドを使用して、データ型を構成することもでき、フィールド自体にさらにデータ型を含めることもできます。 これにより、XDM スキーマ全体で一般的なデータパターンを抽象化して再利用できるので、複雑なデータ構造を簡単に表すことができます。

データタイプへのフィールドの追加が完了したら、「**[!UICONTROL 保存]** を選択して変更を保存し、データタイプを [!DNL Schema Library] に追加します。

## スキーマへのデータタイプの追加 {#add-data-type}

データタイプを作成したら、スキーマでそのデータタイプの使用を開始できます。 XDM スキーマはクラスと 0 個以上のフィールドグループで構成されるので、データタイプによって提供されるフィールドをスキーマに直接追加することはできません。 代わりに、クラスまたはフィールドグループに含める必要があります。

まず、[ クラスへのフィールドの追加 ](./classes.md#add-fields) または [ フィールドグループへのフィールドの追加 ](./field-groups.md#add-fields) の手順に従います。 または、[ スキーマに直接フィールドを追加 ](./schemas.md#add-individual-fields) を開始し、そこから親クラスまたはフィールドグループを選択することもできます。 新しいフィールドの **[!UICONTROL タイプ]** を選択する際に、ドロップダウンメニューからデータタイプの名前を選択します。

## 複数フィールドオブジェクトのデータ型への変換 {#convert}

[!DNL Schema Editor] に複数のサブフィールドを持つオブジェクトタイプフィールドを作成する場合、そのフィールドをデータタイプに変換することで、同じフィールド構造を異なるクラスやフィールドグループで使用できるようになります。

オブジェクトタイプのフィールドをデータタイプに変換するには、キャンバスでフィールドを選択します。 フィールドを変換する前に、**[!UICONTROL 表示名]** が、オブジェクトに格納されるデータを説明したものであることを確認してください。これがデータタイプの名前になります。 フィールドを変換する準備が整ったら、右側のパネルで **[!UICONTROL 新しいデータタイプに変換]** を選択します。

![](../../images/ui/resources/data-types/convert-object.png)

キャンバスは、フィールドのデータタイプを「[!UICONTROL Object]」から新しいデータタイプに更新します。 この構造は、新しいフィールドを定義する際に **[!UICONTROL タイプ]** ドロップダウンからこのデータタイプを選択することで、他のクラスやフィールドグループで再利用できるようになりました。

![](../../images/ui/resources/data-types/converted.png)

## 次の手順 {#next-steps}

このガイドでは、Experience Platform UI を使用してデータタイプを作成および編集する方法について説明しました。 [!UICONTROL &#x200B; スキーマ &#x200B;] ワークスペースの機能について詳しくは、[[!UICONTROL &#x200B; スキーマ &#x200B;] ワークスペースの概要 ](../overview.md) を参照してください。

[!DNL Schema Registry] API を使用してデータタイプを管理する方法については、[ データタイプエンドポイントガイド ](../../api/data-types.md) を参照してください。
