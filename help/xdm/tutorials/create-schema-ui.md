---
keywords: Experience Platform；ホーム；人気の高いトピック；ui;UI;XDM;XDM system；エクスペリエンスデータモデル；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマエディタ；スキーマエディタ；スキーマ;スキーマ;スキーマ；作成
solution: Experience Platform
title: スキーマエディタを使用したスキーマの作成
topic-legacy: tutorial
type: Tutorial
description: このチュートリアルでは、Experience Platform 内でスキーマエディターを使用してスキーマを作成する手順を説明します。
exl-id: 3edeb879-3ce4-4adb-a0bd-8d7ad2ec6102
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '3682'
ht-degree: 16%

---

# [!DNL Schema Editor]

Adobe Experience Platformのユーザーインターフェイスを使用すると、[!DNL Schema Editor]と呼ばれるインタラクティブなビジュアルキャンバスで[!DNL Experience Data Model] (XDM)スキーマを作成および管理できます。 このチュートリアルでは、[!DNL Schema Editor]を使用してスキーマを作成する方法を説明します。

>[!NOTE]
>
>デモ目的で、このチュートリアルの手順では、顧客忠誠度プログラムのメンバーを説明するサンプルスキーマを作成します。 これらの手順を使用して目的に合わせて別のスキーマを作成できますが、まずサンプルスキーマの作成に従って[!DNL Schema Editor]の機能を学ぶことをお勧めします。

代わりに[!DNL Schema Registry] APIを使用してスキーマを構成する場合は、[API](create-schema-api.md)を使用したスキーマの作成に関するチュートリアルを開始する前に、[[!DNL Schema Registry] 開発者ガイド](../api/getting-started.md)を読んで、開始をお勧めします。

## はじめに

このチュートリアルでは、スキーマの作成に関わるAdobe Experience Platformのさまざまな側面について、十分に理解する必要があります。 このチュートリアルを始める前に、次の概念に関するドキュメントを確認してください。

* [[!DNL Experience Data Model (XDM)]](../home.md)：顧客体験データを編成する際に [!DNL Platform] に使用される標準化されたフレームワーク。
   * [スキーマ合成の基本](../schema/composition.md)：XDM スキーマとその構築ブロック（クラス、mixin、データ型、フィールドなど）の概要です。
* [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

## [!UICONTROL スキーマ]ワークスペース{#browse}を開きます

[!DNL Platform] UIの[!UICONTROL スキーマ]ワークスペースは[!DNL Schema Library]のビジュアライゼーションを提供し、組織で使用可能なスキーマを表示が管理できます。 このワークスペースには、このチュートリアル全体でスキーマを構成できるキャンバスである[!DNL Schema Editor]も含まれています。

[!DNL Experience Platform]にログインした後、左側のナビゲーションで&#x200B;**[!UICONTROL スキーマ]**&#x200B;を選択し、**[!UICONTROL スキーマ]**&#x200B;ワークスペースを開きます。 「**[!UICONTROL 参照]**」タブには、スキーマのリスト（[!DNL Schema Library]の表現）が表示され、表示やカスタマイズが可能です。 リストには、スキーマの基となる名前、型、クラス、動作（レコードまたは時系列）、およびスキーマが最後に変更された日時が含まれます。

詳しくは、[UI](../ui/explore.md)の既存のXDMリソースの詳細を参照してください。

## スキーマの作成と命名 {#create}

スキーマの構成を開始するには、**[!UICONTROL スキーマ]**&#x200B;ワークスペースの右上隅にある「スキーマ&#x200B;]**を作成」を選択します。**[!UICONTROL &#x200B;ドロップダウンメニューが表示され、コアクラス[!UICONTROL XDM個別プロファイル]と[!UICONTROL XDM ExperienceEvent]のいずれかを選択できます。 これらのクラスが目的に合わない場合は、「**[!UICONTROL 参照]**」を選択して他の使用可能なクラスから選択するか、[新しいクラス](#create-new-class)を作成することもできます。

このチュートリアルの目的で、「**[!UICONTROL XDM個別プロファイル]**」を選択します。

![](../images/tutorials/create-schema/create_schema_button.png)

スキーマの基にする標準的なXDMクラスを選択したので、**[!UICONTROL mixin追加]**&#x200B;ダイアログが表示され、フィールドをスキーマに追加する際に、すぐに開始を実行できます。 ここでは、「**[!UICONTROL キャンセル]**」を選択してダイアログを終了します。

![](../images/tutorials/create-schema/cancel-mixin.png)

[!DNL Schema Editor]が表示されます。 これは、スキーマを作成するキャンバスです。エディターに移動すると、名称未設定のスキーマがキャンバスの&#x200B;**[!UICONTROL 構造]**&#x200B;セクションに自動的に作成され、そのクラスに基づくすべてのスキーマに含まれる標準フィールドも作成されます。 スキーマに割り当てられたクラスは、**[!UICONTROL 「構成]**」セクションの「クラス&#x200B;**[!UICONTROL Class]**」にも表示されます。

![](../images/tutorials/create-schema/schema_editor.png)

>[!NOTE]
>
> スキーマが保存される前の初期構成プロセス中の任意の時点で[スキーマのクラスを変更](#change-class)できますが、これは非常に注意しておこなう必要があります。ミックスインは特定のクラスとのみ互換性があるので、クラスを変更するとキャンバスと追加したフィールドがリセットされます。

エディターの右側のフィールドを使用して、スキーマの表示名とオプションの説明を入力します。 名前を入力すると、キャンバスが更新され、スキーマの新しい名前が反映されます。

![](../images/tutorials/create-schema/name_schema.png)

スキーマの名前を決定する際に考慮すべき重要な点がいくつかあります。

* スキーマ名は短く、スキーマを後で簡単に見つけられるように、説明的にします。
* スキーマ名は一意である必要があります。つまり、将来再利用されないように十分に具体的でなければなりません。例えば、組織が異なるブランドに対して別々のロイヤルティプログラムを持つ場合、後で定義する他のロイヤルティ関連スキーマと区別しやすいように、スキーマに「ブランド A ロイヤルティメンバー」という名前を付けると効果的です。
* スキーマの説明を使用して、スキーマに関する追加のコンテキスト情報を提供することもできます。

このチュートリアルでは、忠誠度プログラムのメンバに関連するデータを取り込むスキーマを構成します。したがって、スキーマの名前は「Loyalty Members」になります。

## Mixin の追加 {#mixin}

これで、mixin を追加して、スキーマにフィールドを追加できます。ミックスインは、特定の概念を説明するためによく一緒に使用される1つ以上のフィールドのグループです。 このチュートリアルでは、mixin を使用してロイヤルティプログラムのメンバーを説明し、名前、誕生日、電話番号、住所などの重要な情報を取得します。

ミックスインを追加するには、**[!UICONTROL ミックスイン]**&#x200B;サブセクションで追加&#x200B;****&#x200B;を選択します。

![](../images/tutorials/create-schema/add_mixin_button.png)

新しいダイアログが表示され、使用可能なミックスインのリストが表示されます。 各ミックスインは特定のクラスでのみ使用されるため、ダイアログには選択したクラス（この場合は[!DNL XDM Individual Profile]クラス）と互換性のあるリストミックスインのみが表示されます。 標準のXDMクラスを使用している場合、ミックスインのリストは、使用頻度に基づいてインテリジェントに並べ替えられます。

![](../images/tutorials/create-schema/mixin-popularity.png)

リストからMixinを選択すると、右側のパネルにMixinが表示されます。 必要に応じて複数のミックスインを選択し、各ミックスインを確認する前に右側のレールのリストに追加します。 また、現在選択されているミックスインの右側にアイコンが表示され、そのフィールドの構造をプレビューできます。

![](../images/tutorials/create-schema/preview-mixin-button.png)

Mixinをプレビューする際、Mixinのスキーマの詳細が右側のパネルに表示されます。 また、提供されたキャンバス内のMixinのフィールド間を移動することもできます。 別のフィールドを選択すると、右側のパネルが更新され、目的のフィールドに関する詳細が表示されます。 プレビューが終了したら、「**[!UICONTROL 戻る]**」を選択して、mixinの選択ダイアログに戻ります。

![](../images/tutorials/create-schema/preview-mixin.png)

このチュートリアルでは、**[!UICONTROL 人口統計の詳細]**&#x200B;ミックスインを選択し、**[!UICONTROL ミックスイン追加]**&#x200B;を選択します。

![](../images/tutorials/create-schema/add_mixin_person_details.png)

スキーマキャンバスが再び表示されます。**[!UICONTROL ミックスイン]**&#x200B;セクションには、「[!UICONTROL 人口統計的詳細]」と&#x200B;**[!UICONTROL 構造]**&#x200B;セクションには、ミックスインが寄与するフィールドが含まれるようになりました。 「**[!UICONTROL ミックスイン]**」セクションでミックスインの名前を選択し、キャンバス内で提供される特定のフィールドを強調表示することができます。

![](../images/tutorials/create-schema/person_details_structure.png)

このミックスインは、最上位レベルの名前`person`の下に、「[!UICONTROL 人]」というデータ型のフィールドをいくつか使用します。 このフィールドグループは、名前、生年月日、性別など、個人に関する情報を説明します。

>[!NOTE]
>
>フィールドは、スカラー型（文字列、整数、配列、日付など）と、[!DNL Schema Registry]内で定義された任意のデータ型（共通の概念を表すフィールドのグループ）を使用できます。

`name`フィールドのデータ型は「[!UICONTROL 人名]」です。つまり、一般的な概念を表し、名、姓、敬称、敬称、接尾辞など、名前に関連するサブフィールドが含まれます。

キャンバス内の別のフィールドを選択すると、スキーマ構造に影響を与える追加のフィールドが表示されます。

## 別の mixin の追加 {#mixin-2}

次に、同じ手順を繰り返して、別の mixin を追加できます。今回は&#x200B;**[!UICONTROL mixin追加]**&#x200B;ダイアログを表示する際に、「[!UICONTROL 人口統計の詳細]」ミックスインは灰色表示になっており、隣のチェックボックスは選択できないことに注意してください。 これは、既に現在のスキーマに含まれている mixin を誤って複製するのを防ぎます。

このチュートリアルでは、ダイアログから「[!DNL Personal Contact Details]」ミックスインを選択し、「**[!UICONTROL ミックスイン追加]**」を選択してスキーマに追加します。

![](../images/tutorials/create-schema/add_mixin_personal_details.png)

追加すると、キャンバスが再び表示されます。「[!UICONTROL 個人の連絡先の詳細]」が&#x200B;**[!UICONTROL コンポジション]**&#x200B;セクションの&#x200B;**[!UICONTROL ミックスイン]**&#x200B;の下に表示され、自宅住所、携帯電話などのフィールドが&#x200B;**[!UICONTROL 構造]**&#x200B;の下に追加されました。

`name`フィールドと同様、先ほど追加したフィールドは複数フィールドの概念を表しています。 例えば、`homeAddress`のデータ型は「[!UICONTROL 住所]」、`mobilePhone`のデータ型は「[!UICONTROL 電話番号]」です。 これらの各フィールドを選択して展開し、データ型に含まれる追加のフィールドを表示できます。

![](../images/tutorials/create-schema/personal_details_structure.png)

## カスタムミックスインを定義する{#define-mixin}

「[!UICONTROL 忠誠度メンバー]」スキーマは、忠誠度プログラムのメンバーに関連するデータを取り込むためのものなので、いくつかの特定の忠誠度関連フィールドが必要になります。

標準の[!UICONTROL 忠誠度の詳細]ミックスインがあり、スキーマに追加して、忠誠度のプログラムに関連する一般的なフィールドを取り込むことができます。 スキーマがキャプチャした概念を表すために標準ミックスインを使用することを強くお勧めしますが、標準忠誠度ミックスインの構造は、特定の忠誠度プログラムに関連するすべてのデータを取り込めない場合があります。 このシナリオでは、代わりに、これらのフィールドを取り込むための新しいカスタムミックスインを定義できます。

**[!UICONTROL Mixin追加]**&#x200B;ダイアログを再度開きますが、今回は上部の「**[!UICONTROL 新しいMixinを作成]**」を選択します。 その後、ミックスインの表示名と説明を入力するように求められます。

![](../images/tutorials/create-schema/mixin_create_new.png)

クラス名と同様に、mixin 名は短く単純で、mixin がスキーマに与える影響を説明するべきです。これらも一意なので、名前を再利用できません。名前が十分に具体的であるようにしてください。

このチュートリアルでは、新しい mixin に「Loyalty Details」という名前を付けます。

**[!UICONTROL ミッ追加クスイン]**&#x200B;を選択して[!DNL Schema Editor]に戻ります。 キャンバスの左側の&#x200B;**[!UICONTROL ミックスイン]**&#x200B;の下に&quot;[!UICONTROL 忠誠度の詳細]&quot;が表示されるはずですが、関連するフィールドはまだないので、**[!UICONTROL 構造]**&#x200B;の下に新しいフィールドは表示されません。

## Mixin へのフィールドの追加 {#mixin-fields}

「Loyalty Details」mixin を作成したら、mixin がスキーマに貢献するフィールドを定義します。

最初に、**[!UICONTROL ミックスイン]**&#x200B;セクションでミックスイン名を選択します。 これを行うと、mixinのプロパティがエディターの右側に表示され、**[!UICONTROL 構造]**&#x200B;の下のスキーマ名の横に&#x200B;**プラス(+)**&#x200B;アイコンが表示されます。

![](../images/tutorials/create-schema/loyalty_details_structure.png)

「[!DNL Loyalty Members]」の横のプラス(+)**アイコンを選択して、構造内に新しいノードを作成します。**&#x200B;このノード（この例では`_tenantId`と呼ばれます）は、IMS組織のテナントIDを表し、前にアンダースコアが付きます。 テナント ID の存在は、追加するフィールドが組織の名前空間に限られていることを示しています。

つまり、追加するフィールドは組織に固有のものであり、組織にのみアクセス可能な特定の領域の[!DNL Schema Registry]に保存されます。 定義するフィールドは、他の標準クラス、ミックスイン、データ型、およびフィールドの名前との競合を防ぐために、常にテナント名前空間に追加する必要があります。

その名前の付いたノードの中には、「[!UICONTROL 新しいフィールド]」が入っています。 これは、「[!UICONTROL 忠誠度の詳細]」ミックスインの開始です。

![](../images/tutorials/create-schema/new_field_loyalty.png)

エディターの右側のコントロールを使用して、忠誠度関連のフィールドの保持に使用する`loyalty`オブジェクト]の型を持つ[!UICONTROL フィールドを作成し、開始します。 終了したら、「**[!UICONTROL 適用]**」を選択します。

![](../images/tutorials/create-schema/loyalty_object.png)

変更が適用され、新しく作成された`loyalty`オブジェクトが表示されます。 オブジェクトの横にあるプラス(+)**アイコンを選択し、忠誠度関連のフィールドを追加します。**「[!UICONTROL 新しいフィールド]」が表示され、キャンバスの右側に&#x200B;**[!UICONTROL フィールドプロパティ]**&#x200B;セクションが表示されます。

![](../images/tutorials/create-schema/new_field_in_loyalty_object.png)

各フィールドには、次の情報が必要です。

* **[!UICONTROL Field Name]：フィールド** の名前。キャメルケースで書かれています。例：loyaltyLevel
* **[!UICONTROL 表示名]：フィールド** の名前。タイトルの場合は大文字で記述されます。例：Loyalty Level
* **[!UICONTROL Type]:** フィールドのデータ型。これには、基本的なスカラー型と[!DNL Schema Registry]で定義されているすべてのデータ型が含まれます。 例：[!UICONTROL 文字列]、[!UICONTROL 整数]、[!UICONTROL ブール値]、[!UICONTROL 人]、[!UICONTROL 住所]、[!UICONTROL 電話番号]など。
* **[!UICONTROL 説明]:（オプション）フィールド** の説明は、文頭の場合と同様に、最大200文字で記述します。

`Loyalty`オブジェクトの最初のフィールドは、`loyaltyId`という文字列になります。 新しいフィールドのタイプを「[!UICONTROL 文字列]」に設定すると、**[!UICONTROL フィールドプロパティ]**&#x200B;セクションに、デフォルト値、形式、最大長など、制約を適用するためのオプションが入力されます。

![](../images/tutorials/create-schema/string_constraints.png)

選択したデータ型に応じて、様々な制約オプションを使用できます。`loyaltyId`は電子メールアドレスになるので、**[!UICONTROL 形式]**&#x200B;ドロップダウンメニューから「[!UICONTROL 電子メール]」を選択します。 「**[!UICONTROL 適用]**」を選択して変更を適用します。

![](../images/tutorials/create-schema/loyaltyId_field.png)

## mixinの追加追加フィールド{#mixin-fields-2}

`loyaltyId`フィールドを追加したら、次のような忠誠度関連の情報を取り込むためのフィールドを追加できます。

* ポイント（整数）
* 会員登録日

各フィールドをスキーマに追加するには、`loyalty`オブジェクトの横にある&#x200B;**プラス(+)**&#x200B;アイコンを選択し、必要な情報を入力します。

完了すると、忠誠度オブジェクトには、忠誠度ID、ポイント、およびメンバー登録用のフィールドが含まれます。

![](../images/tutorials/create-schema/loyalty_object_fields.png)

## mixin追加の列挙フィールド{#enum}

[!DNL Schema Editor]にフィールドを定義する場合、基本的なフィールドの種類に適用できる追加のオプションがいくつかあり、フィールドに含めることのできるデータに対してさらなる制約を設けます。 これらの制約の使用例を次の表に示します。

| 制約 | 説明 |
| --- | --- |
| [!UICONTROL 必須] | フィールドがデータ取り込みに必要であることを示します。 このフィールドを含まないデータセットに基づいてスキーマセットにアップロードされたデータの取得は失敗します。 |
| [!UICONTROL 配列] | フィールドに値の配列が含まれ、各値は指定されたデータ型になっていることを示します。 例えば、データ型が「[!UICONTROL String]」のフィールドに対してこの制約を使用すると、フィールドに文字列の配列が含まれるように指定できます。 |
| [!UICONTROL Enum] | このフィールドに、可能な値の列挙リストの値の1つを含める必要があることを示します。 |
| [!UICONTROL ID] | このフィールドがIDフィールドであることを示します。 ID フィールドの詳細については、[このチュートリアルの後半](#identity-field)で説明します。 |
| [!UICONTROL Relationship] | スキーマの関係は、和集合スキーマと[!DNL Real-time Customer Profile]を使用して推定できますが、これは同じクラスを共有するスキーマにのみ当てはまります。 [!UICONTROL Relationship]制約は、このフィールドが、異なるクラスに基づくスキーマの主なIDを参照することを示し、2つのスキーマ間の関係を示します。 詳細は、[関係の定義](./relationship-ui.md)のチュートリアルを参照してください。 |

>[!NOTE]
>
>左側のナビゲーションバーには、必須フィールド、IDフィールドまたは関係フィールドが表示され、スキーマの複雑さに関係なく、これらのフィールドを簡単に見つけることができます。
>
>![](../images/tutorials/create-schema/left-rail-special.png)

このチュートリアルでは、スキーマの[!DNL "loyalty"]オブジェクトに、顧客の「忠誠度レベル」を示す新しい列挙フィールドが必要です。値は、4つのオプションのうちの1つに限定されます。 このフィールドをスキーマに追加するには、`loyalty`オブジェクトの横にある&#x200B;**プラス(+)**&#x200B;アイコンを選択し、**[!UICONTROL フィールド名]**&#x200B;と&#x200B;**[!UICONTROL 表示名]**&#x200B;の必須フィールドに入力します。 「**[!UICONTROL タイプ]**」で、「[!UICONTROL 文字列]」を選択します。

![](../images/tutorials/create-schema/loyalty-level-type.png)

**[!UICONTROL 配列]**、**[!UICONTROL 列挙]**、**[!UICONTROL ID]**&#x200B;のチェックボックスなど、タイプを選択した後に、フィールドに追加のチェックボックスが表示されます。

「**[!UICONTROL 列挙]**」チェックボックスを選択して、下の&#x200B;**[!UICONTROL 列挙値]**&#x200B;を開きます。 ここで、許容可能な各ロイヤルティレベルの&#x200B;**[!UICONTROL 値]**（キャメルケース）と&#x200B;**[!UICONTROL ラベル]**（タイトルケースでの読みやすい名前でオプション）を入力できます。

すべてのフィールドプロパティを完了したら、「**[!UICONTROL 適用]**」を選択して、「[!DNL loyaltyLevel]」フィールドを`loyalty`オブジェクトに追加します。

![](../images/tutorials/create-schema/loyalty_level_enum.png)

## 複数フィールドオブジェクトのデータ型への変換 {#datatype}

`loyalty`オブジェクトには複数の忠誠度固有のフィールドが含まれ、他のスキーマで役立つ共通のデータ構造を表すようになりました。 [!DNL Schema Editor]を使用すると、再利用可能な複数フィールドオブジェクトを簡単に適用できます。これらのオブジェクトの構造をデータ型に変換できます。

データ型を使用すると、複数フィールド構造を一貫して使用でき、mixin よりも柔軟性が高まります。これは、データ型がスキーマ内のどこでも使用できるからです。これは、フィールドの&#x200B;**[!UICONTROL Type]**&#x200B;値を、[!DNL Schema Registry]で定義されている任意のデータ型の値に設定することで行います。

`loyalty`オブジェクトをデータ型に変換するには、**[!UICONTROL 構造]**&#x200B;の下の`loyalty`フィールドを選択し、**[!UICONTROL フィールドのプロパティ]**&#x200B;の下のエディターの右側の&#x200B;**[!UICONTROL 新しいデータ型]**&#x200B;に変換を選択します。 オブジェクトが正常に変換されたことを確認する緑のポーバーが表示されます。

![](../images/tutorials/create-schema/convert-data-type.png)

**[!UICONTROL 構造]**&#x200B;の下を見ると、`loyalty`フィールドのデータ型が「[!DNL Loyalty]」で、フィールドの横に小さな錠前のアイコンが表示され、個々のフィールドではなく、複数フィールドのデータ型であることがわかります。

![](../images/tutorials/create-schema/loyalty_data_type.png)

今後のスキーマでは、フィールドを「[!DNL Loyalty]」型として割り当て、ID、忠誠度レベル、メンバーシンク、ポイントのフィールドを自動的に含めることができます。

>[!NOTE]
>
>カスタムデータタイプは、編集スキーマとは独立して作成および編集することもできます。 詳しくは、[データ型の作成と編集](../ui/resources/data-types.md)のガイドを参照してください。

## スキーマフィールドの検索とフィルター

スキーマには、基本クラスが提供するフィールドに加えて、複数のミックスインが含まれるようになりました。 大きいスキーマを使用する場合は、左側のレールでmixin名の横にあるチェックボックスをオンにして、表示されるフィールドを、関心のあるミックスインで提供されるフィールドのみにフィルタリングできます。

![](../images/tutorials/create-schema/filter-by-mixin.png)

スキーマ内で特定のフィールドを探している場合は、検索バーを使用して、表示されるフィールドを、どのミックスインがどの下に提供されているかに関係なく、名前でフィルターすることもできます。

![](../images/tutorials/create-schema/search.png)

>[!IMPORTANT]
>
>検索機能は、一致するフィールドを表示する際に、選択したミックスインフィルターを考慮に入れます。 検索クエリに期待した結果が表示されない場合は、関連するミックスインがフィルターで除外されていないことを重複チェックが必要になる場合があります。

## ID フィールドとしてのスキーマフィールドの設定 {#identity-field}

スキーマが提供する標準的なデータ構造を活用して、複数のソースにわたって同じ個人に属するデータを識別でき、分類、レポート、データサイエンスの分析など、様々な下流の使用例に対応できます。 個々のIDに基づいてデータを結合するには、キーフィールドを該当するスキーマ内で[!UICONTROL ID]フィールドとしてマークする必要があります。

[!DNL Experience Platform] を使用すると、の「 **** Identity」チェックボックスを使用して、IDフィールドを簡単に示すことがで [!DNL Schema Editor]きます。ただし、データの性質に基づいて、IDとして使用するのに最適なフィールドを決定する必要があります。

例えば、同じ「忠誠度レベル」に属する何千もの忠誠度プログラムメンバーが存在する場合がありますが、忠誠度プログラムの各メンバーは一意の`loyaltyId`を持ちます（この場合は、個々のメンバーの電子メールアドレスです）。 `loyaltyId`が各メンバの一意の識別子であることは、`loyaltyLevel`がIDフィールドに対する適切な候補となるのに対し、は異なります。

>[!IMPORTANT]
>
>次の手順は、既存のスキーマフィールドにID記述子を追加する方法を説明します。 スキーマ自体の構造内にIDフィールドを定義する代わりに、`identityMap`フィールドを使用してID情報を格納することもできます。
>
>`identityMap`を使用する予定がある場合は、スキーマに直接追加するすべてのプライマリIDが上書きされることに注意してください。 詳しくは、『スキーマ構成ガイド](../schema/composition.md#identityMap)の基本』の`identityMap`の節を参照してください。[

エディターの&#x200B;**[!UICONTROL 構造]**&#x200B;セクションで、`loyaltyId`フィールドを選択し、**[!UICONTROL フィールドのプロパティ]**&#x200B;の下に「**[!UICONTROL ID]**」チェックボックスが表示されます。 チェックボックスをオンにし、**[!UICONTROL プライマリID]**&#x200B;として設定するオプションが表示されます。 このボックスも選択します。

>[!NOTE]
>
>各スキーマには、1 つのプライマリ ID フィールドのみを含めることができます。スキーマフィールドをプライマリIDとして設定すると、後でスキーマ内の別のIDフィールドをプライマリIDとして設定しようとすると、エラーメッセージが表示されます。

次に、ドロップダウン内の事前定義済みの名前空間のリストから&#x200B;**[!UICONTROL ID名前空間]**&#x200B;を指定する必要があります。 `loyaltyId`はお客様の電子メールアドレスなので、ドロップダウンから「[!UICONTROL 電子メール]」を選択します。 **[!UICONTROL 「]**&#x200B;を適用」を選択して、`loyaltyId`フィールドの更新を確認します。

![](../images/tutorials/create-schema/loyaltyId_primary_identity.png)

>[!NOTE]
>
>標準名前空間とその定義のリストについては、[[!DNL Identity Service] ドキュメント](../../identity-service/troubleshooting-guide.md#standard-namespaces)を参照してください。

変更を適用した後、`loyaltyId`のアイコンに指紋の記号が表示され、現在はIDフィールドであることが示されます。

![](../images/tutorials/create-schema/identity-applied.png)

`loyaltyId`フィールドに取り込まれたすべてのデータは、その個人を特定し、その顧客の1つの表示をつなぎ合わせるのに役立ちます。 [!DNL Experience Platform]でIDを使用する方法の詳細については、[[!DNL Identity Service]](../../identity-service/home.md)のドキュメントを参照してください。

## スキーマを[!DNL Real-time Customer Profile] {#profile}で使用できるようにする

[[!DNL Real-time Customer Profile]](../../profile/home.md) アイデンティティデータ [!DNL Experience Platform] を活用して、個々の顧客の全体的な表示を提供します。このサービスは、[!DNL Experience Platform]と統合されたあらゆるシステムにわたって顧客属性の360°の堅牢なプロファイルと、顧客が持つすべてのインタラクション顧客のタイムスタンプのあるアカウントを構築します。

スキーマを[!DNL Real-time Customer Profile]で使用できるようにするには、プライマリIDが定義されている必要があります。 最初にプライマリIDを定義せずにスキーマを有効にしようとすると、エラーメッセージが表示されます。

<img src="../images/tutorials/create-schema/missing_primary_identity.png" width="600" /><br>

[!DNL Profile]で「Loyalty Members」スキーマを使用できるようにするには、まずエディターの&#x200B;**[!UICONTROL Structure]**&#x200B;セクションで「[!DNL Loyalty Members]」を選択します。

エディターの右側には、スキーマに関する情報（表示名、説明、タイプなど）が表示されます。 この情報に加えて、**[!UICONTROL プロファイル]**&#x200B;の切り替えボタンがあります。

![](../images/tutorials/create-schema/profile-toggle.png)

「**[!UICONTROL プロファイル]**」を選択すると、[!DNL Profile]のスキーマを有効にするかどうかを確認するプローバーが表示されます。

<img src="../images/tutorials/create-schema/enable-profile.png" width="700" /><br>

>[!WARNING]
>
>スキーマが[!DNL Real-time Customer Profile]に対して有効になって保存されると、無効にすることはできません。

「**[!UICONTROL 有効]**」を選択して、選択を確定します。 **[!UICONTROL プロファイル]**&#x200B;の切り替えを再度選択して、必要に応じてスキーマを無効にできますが、[!DNL Profile]が有効な間にスキーマを保存すると、無効にできなくなります。

## 次の手順とその他のリソース

スキーマの構成が完了すると、カンバスに完全なスキーマが表示されます。 「**[!UICONTROL 保存]**」を選択すると、スキーマが[!DNL Schema Library]に保存され、[!DNL Schema Registry]からアクセスできるようになります。

新しいスキーマを使って[!DNL Platform]にデータを取り込めるようになりました。 データの取得にスキーマを使用した後は、追加的な変更のみがおこなわれる場合があります。スキーマバージョン管理について詳しくは、「[スキーマ合成の基本](../schema/composition.md)」を参照してください。

これで、[UI](./relationship-ui.md)でスキーマの関係を定義するチュートリアルに従って、「Loyality Members」スキーマに新しい関係フィールドを追加できます。

「Loyality Members」スキーマは、[!DNL Schema Registry] APIを使用して表示および管理することもできます。 APIの使用を開始するには、[[!DNL Schema Registry API] 開始ガイド](../api/getting-started.md)を読んでください。

### ビデオリソース

>[!WARNING]
>
>次のビデオに示す[!DNL Platform] UIは古いです。 最新のUIのスクリーンショットと機能については、上記のドキュメントを参照してください。

次のビデオでは、[!DNL Platform] UIで単純なスキーマを作成する方法を示します。

>[!VIDEO](https://video.tv.adobe.com/v/27012?quality=12&learn=on)

次のビデオは、ミックスインとクラスを使用する際の理解を深めるためのものです。

>[!VIDEO](https://video.tv.adobe.com/v/27013?quality=12&learn=on)

## 付録

以下の節では、[!DNL Schema Editor]の使用に関する追加情報を示します。

### 新しいクラスの作成 {#create-new-class}

[!DNL Experience Platform] は、組織に固有のクラスに基づいてスキーマを定義する柔軟性を提供します。新しいクラスの作成方法については、UI](../ui/resources/classes.md#create)の[クラスの作成と編集に関するガイドを参照してください。

### スキーマクラスの変更 {#change-class}

スキーマのクラスは、スキーマが保存される前の最初の構成プロセスの任意の時点で変更できます。

>[!WARNING]
>
>スキーマに対するクラスの再割り当ては、細心の注意を払って行う必要があります。 ミックスインは特定のクラスとのみ互換性があるので、クラスを変更するとキャンバスと追加したフィールドがリセットされます。

スキーマのクラスを変更する方法については、[UI](../ui/resources/schemas.md)でのスキーマの管理に関するガイドを参照してください。
