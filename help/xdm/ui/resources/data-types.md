---
keywords: Experience Platform；ホーム；人気のトピック；ui;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマレジストリ；スキーマ；スキーマ；作成；データタイプ；データタイプ；
solution: Experience Platform
title: UI を使用したデータタイプの作成と編集
type: Tutorial
description: Experience Platform ユーザーインターフェイスでデータタイプを作成および編集する方法について説明します。
exl-id: 2c917154-c425-463c-b8c8-04ba37d9247b
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1333'
ht-degree: 7%

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
>フィールドが特定のデータ型として定義されている場合、別のスキーマで同じデータ型の同じフィールドを作成することはできません。 この制約は、組織のテナント全体に適用されます。

このチュートリアルでは、Experience Platform ユーザー インターフェイスでカスタムデータ型を作成および編集する手順について説明します。

## 前提条件 {#prerequisites}

このガイドには、XDM システムについて十分に理解している必要があります。 Experience Platform エコシステムでの XDM の役割の概要については [XDM の概要 &#x200B;](../../home.md) を、データタイプが XDM スキーマにどのように寄与するかについては [&#x200B; スキーマ構成の基本 &#x200B;](../../schema/composition.md) を参照してください。

このガイドには必要ありませんが、[UI でのスキーマの作成 &#x200B;](../../tutorials/create-schema-ui.md) に関するチュートリアルに従って、[!DNL Schema Editor] の様々な機能を理解することをお勧めします。

## データタイプの [!DNL Schema Editor] を開く {#data-type}

Experience Platform UI で、左側のナビゲーションで **[!UICONTROL Schemas]** を選択して [!UICONTROL Schemas] Workspace を開き、「**[!UICONTROL Data types]**」タブを選択します。 使用可能なデータタイプのリストが表示されます。 データタイプのリストは、作成方法に基づいて自動的にフィルタリングされます。 デフォルト設定では、Adobe Systems によって定義されたデータ型が表示されます。 また、リストをフィルタリングして、組織によって作成されたものを表示することもできます。

![左側のナビゲーションに[!UICONTROL Schemas]が付いた[!UICONTROL Schemas]ワークスペース、[!UICONTROL Data types]強調表示されます。](../../images/ui/resources/data-types/data-types-tab.png)

ここから、次のオプションを使用できます。

- [新しいデータタイプの作成](#create)
- [データタイプのフィルタリング](#filter)
- [編集する既存のデータタイプを選択](#edit)

### 新しいデータタイプの作成 {#create}

「**[!UICONTROL Data types]**」タブから「**[!UICONTROL Create data type]**」を選択します。

![[!UICONTROL Schemas] がハイライト表示された「[!UICONTROL Data types] Workspace [!UICONTROL Create data type]」タブ &#x200B;](../../images/ui/resources/data-types/create.png)

[!DNL Schema Editor] が表示され、キャンバスに新しいデータタイプの現在の構造が表示されます。 エディターの右側で、データタイプの表示名と説明（オプション）を入力できます。 スキーマに追加する際にデータタイプを識別する方法と同様に、データタイプに一意で簡潔な名前を指定します。

このチュートリアルでは、レストランプロパティを説明するデータタイプを作成するので、データタイプには「Restaurant」という表示名が付きます。

![](../../images/ui/resources/data-types/data-type-properties.png)

ここから、[&#x200B; 次のセクション &#x200B;](#add-fields) に進んで、新しいデータタイプへのフィールドの追加を開始できます。

### フィルターデータタイプ {#filter}

使用可能なデータタイプのリストは、作成方法に基づいて事前にフィルタリングされています。ラジオボタンを選択して、 [!UICONTROL Standard] オプションと [!UICONTROL Custom] オプションから選択します。 [ [!UICONTROL Standard] ] オプションを選択すると、Adobe Systemsによって作成されたエンティティが表示され、[ [!UICONTROL Custom] ] オプションを選択すると、組織内で作成されたエンティティが表示されます。

![[!UICONTROL Data types]の[!UICONTROL Schemas]タブは、[!UICONTROL Standard]と[!UICONTROL Custom]が強調表示された状態でワークスペースされます。](../../images/ui/resources/data-types/standard-and-custom-data-types.png)

### 既存のデータタイプの編集 {#edit}

>[!NOTE]
>
>リアルタイム顧客プロファイルで使用できるように設定されているスキーマで既存のデータタイプを使用すると、以降はそのデータタイプに非破壊的な変更のみを加えることができます。 詳しくは、[&#x200B; スキーマ進化のルール &#x200B;](../../schema/composition.md#evolution) を参照してください。

組織で定義されたカスタム データ タイプのみを編集できます。 [ **[!UICONTROL Custom]** ] を選択すると、組織が所有するカスタム データ タイプのみが表示されます。

リストから編集するデータ型を選択して右側のパネルを開き、データ型の詳細を表示します。 詳細パネルから、サンプル ファイルをダウンロードするしたり、JSON 構造をコピーしたり、パッケージにデータ型を追加したりすることもできます。

右側のパネルでデータ型の名前を選択して、 [!DNL Schema Editor]でその構造を開きます。

![&#x200B; データタイプ [!UICONTROL Data types]、データタイプ [!UICONTROL Schemas] がハイライト表示された [!UICONTROL Custom] ワークスペースの「[!UICONTROL Name]」タブ &#x200B;](../../images/ui/resources/data-types/edit.png)

## データタイプへのフィールドの追加 {#add-fields}

データタイプへのフィールドの追加を開始するには、キャンバスのルートレベルフィールドの横にある **プラス（+）** アイコンを選択します。 下に新しいフィールドが表示され、右側のパネルが更新されて、新しいフィールドのコントロールが表示されます。

![](../../images/ui/resources/data-types/new-field.png)

右側のパネルのコントロールを使用して、新しいフィールドの詳細を設定します。 フィールドを設定してデータタイプに追加する方法に関する具体的な手順については、[UI でのフィールドの定義 &#x200B;](../fields/overview.md#define) に関するガイドを参照してください。

レストラン データタイプには、レストランの名前を表す文字列フィールドが必要です。 そのため、[!UICONTROL Field name] は「name」、[!UICONTROL Type] は「[!UICONTROL String]」に設定されます。 **[!UICONTROL Apply]**&#x200B;を選択して、フィールドに変更を適用します。

![](../../images/ui/resources/data-types/name-field.png)

続行必要に応じて、データ型にフィールドを追加します。 サンプルのレストラン データ タイプに、ブランド、座席数、およびフロア面積の追加フィールドが追加されました。

![](../../images/ui/resources/data-types/more-fields.png)

基本フィールドに加えて、カスタムデータ型内に追加のデータ型を入れ子にすることもできます。 たとえば、レストラン データ型には、プロパティの住所を表すフィールドが必要です。 このシナリオでは、標準データ型 &quot;[!UICONTROL Postal address]&quot; が割り当てられた新しい &quot;アドレス&quot; フィールドを追加できます。

![](../../images/ui/resources/data-types/address-field.png)

これは、データ型がデータを記述するという点でどれほど柔軟であるかを示しています:データ型はデータ型でもあるフィールドを使用でき、それ自体がさらにデータ型を含むことができます。 これにより、XDM スキーマ全体で一般的なデータパターンを抽象化して再利用できるので、複雑なデータ構造を簡単に表すことができます。

データタイプへのフィールドの追加が完了したら、「**[!UICONTROL Save]**」を選択して変更を保存し、データタイプを [!DNL Schema Library] に追加します。

## スキーマへのデータタイプの追加 {#add-data-type}

データタイプを作成したら、スキーマでそのデータタイプの使用を開始できます。 XDM スキーマはクラスと 0 個以上のフィールドグループで構成されるので、データタイプによって提供されるフィールドをスキーマに直接追加することはできません。 代わりに、クラスまたはフィールドグループに含める必要があります。

開始 [クラスへのフィールドの追加](./classes.md#add-fields) または [フィールドへのフィールドの追加 グループ](./field-groups.md#add-fields) に関連する手順に従います。 または、[&#x200B; スキーマに直接フィールドを追加 &#x200B;](./schemas.md#add-individual-fields) を開始し、そこから親クラスまたはフィールドグループを選択することもできます。 新しいフィールドの **[!UICONTROL Type]** を選択する場合は、ドロップダウンメニューからデータタイプの名前を選択します。

## 複数フィールドオブジェクトのデータ型への変換 {#convert}

[!DNL Schema Editor] に複数のサブフィールドを持つオブジェクトタイプフィールドを作成する場合、そのフィールドをデータタイプに変換することで、同じフィールド構造を異なるクラスやフィールドグループで使用できるようになります。

オブジェクトタイプのフィールドをデータタイプに変換するには、キャンバスでフィールドを選択します。 フィールドを変換する前に、オブジェクトに含まれるデータを説明する **[!UICONTROL Display name]** を指定してください。これがデータタイプの名前になるからです。 フィールドを変換する準備が整ったら、右側のパネルで「**[!UICONTROL Convert to new data type]**」を選択します。

![](../../images/ui/resources/data-types/convert-object.png)

キャンバスは、フィールドのデータタイプを「[!UICONTROL Object]」から新しいデータタイプに更新します。 この構造は、新しいフィールドを定義する際に **[!UICONTROL Type]** ドロップダウンからこのデータタイプを選択することで、他のクラスおよびフィールドグループで再利用できるようになりました。

![](../../images/ui/resources/data-types/converted.png)

## 次の手順 {#next-steps}

このガイドでは、Experience Platform UI を使用してデータタイプを作成および編集する方法について説明しました。 [!UICONTROL Schemas] workspace の機能について詳しくは、[[!UICONTROL Schemas] workspace の概要を参照してください &#x200B;](../overview.md)

[!DNL Schema Registry] API を使用してデータタイプを管理する方法については、[&#x200B; データタイプエンドポイントガイド &#x200B;](../../api/data-types.md) を参照してください。
