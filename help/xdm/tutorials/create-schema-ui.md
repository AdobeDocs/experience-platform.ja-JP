---
keywords: Experience Platform;home;popular topics;ui;UI;XDM;XDM system;;experience data model;Experience data model;Experience Data Model;data model;Data Model;schema editor;Schema Editor;schema;Schema;schemas;Schemas;create
solution: Experience Platform
title: スキーマエディターを使用したスキーマの作成
topic: tutorial
type: Tutorial
description: このチュートリアルでは、Experience Platform 内でスキーマエディターを使用してスキーマを作成する手順を説明します。
translation-type: tm+mt
source-git-commit: 26c3aa3b21c2d9850f29816d57ddf2da953d6b10
workflow-type: tm+mt
source-wordcount: '3779'
ht-degree: 18%

---


# スキーマの作成には、 [!DNL Schema Editor]

Adobe Experience Platformユーザーインターフェイスを使用すると、と呼ばれるインタラクティブなビジュアルキャンバスで [!DNL Experience Data Model] (XDM)スキーマを作成および管理でき [!DNL Schema Editor]ます。 このチュートリアルでは、を使用してスキーマを作成する方法を説明し [!DNL Schema Editor]ます。

>[!NOTE]
>
>デモ目的で、このチュートリアルの手順では、顧客忠誠度プログラムのメンバーを説明するサンプルスキーマを作成します。 これらの手順を使用して目的に合わせて別のスキーマを作成できますが、まずサンプルスキーマの作成に従って、の機能を学ぶことをお勧めし [!DNL Schema Editor]ます。

If you prefer to compose a schema using the [!DNL Schema Registry] API instead, start by reading the [[!DNL Schema Registry] developer guide](../api/getting-started.md) before attempting the tutorial on [creating a schema using the API](create-schema-api.md).

## はじめに

このチュートリアルでは、スキーマの作成に関わるAdobe Experience Platformのさまざまな側面について、十分に理解する必要があります。 このチュートリアルを始める前に、次の概念に関するドキュメントを確認してください。

* [[!DNL Experience Data Model (XDM)]](../home.md):顧客体験データを [!DNL Platform] 整理する際に使用される標準化されたフレームワーク。
   * [スキーマ合成の基本](../schema/composition.md)：XDM スキーマとその構築ブロック（クラス、mixin、データ型、フィールドなど）の概要です。
* [[!DNL Real-time Customer Profile]](../../profile/home.md):複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

## [ [!UICONTROL スキーマ] ]ワークスペースを開く {#browse}

[!UICONTROL UIの] スキーマ [!DNL Platform] ワークスペースは、のビジュアライゼーションを提供し [!DNL Schema Library]、組織で使用可能なスキーマを表示が管理できます。 The workspace also includes the [!DNL Schema Editor], the canvas on which you can compose a schema throughout this tutorial.

ログイン後、左側のナビゲーションで「 [!DNL Experience Platform]スキーマ **[!UICONTROL 」を選択し、]** スキーマ **** ワークスペースを開きます。 「 **[!UICONTROL 参照]** 」タブには、スキーマのリスト(の表現 [!DNL Schema Library])が表示され、表示やカスタマイズが可能です。 リストには、スキーマの基となる名前、型、クラス、動作（レコードまたは時系列）、およびスキーマが最後に変更された日時が含まれます。

詳しくは、UI内の既存のXDMリソースの [詳細に関するガイドを参照してください](./explore.md) 。

## スキーマの作成と命名 {#create}

To begin composing a schema, select **[!UICONTROL Create schema]** in the top-right corner of the **[!UICONTROL Schemas]** workspace. ドロップダウンメニューが表示され、コアクラス [!UICONTROL XDM Indivialプロファイル] と [!UICONTROL XDM ExperienceEventのいずれかを選択できます]。 これらのクラスが目的に合わない場合は、 **[!UICONTROL 「参照]** 」を選択して、使用可能な他のクラスから選択するか、新しいクラス [を作成することもできます](#create-new-class)。

このチュートリアルの目的で、「 **[!UICONTROL XDM Individual Individualプロファイル]**」を選択します。

![](../images/tutorials/create-schema/create_schema_button.png)

が表示 [!DNL Schema Editor] されます。 これは、スキーマを作成するキャンバスです。スキーマの基にする標準XDMクラスを選択したので、エディターに到達すると、名称未設定のスキーマがキャンバスの **[!UICONTROL 構造]** セクションに自動的に作成され、そのクラスに基づくすべてのスキーマに含まれる標準フィールドも作成されます。 スキーマに割り当てられたクラスは、「 **[!UICONTROL 組版]****[!UICONTROL 」セクションの「クラス]** 」にも表示されます。

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

To add a mixin, select **[!UICONTROL Add]** in the **[!UICONTROL Mixins]** sub-section.

![](../images/tutorials/create-schema/add_mixin_button.png)

新しいダイアログが表示され、使用可能なミックスインのリストが表示されます。 各ミックスインは特定のクラスでのみ使用されるため、ダイアログには選択したクラス（この場合はクラス）と互換性のあるリストミックスインのみが表示され [!DNL XDM Individual Profile] ます。 標準のXDMクラスを使用している場合、ミックスインのリストは、使用頻度に基づいてインテリジェントに並べ替えられます。

![](../images/tutorials/create-schema/mixin-popularity.png)

リストからMixinを選択すると、右側のパネルにMixinが表示されます。 必要に応じて複数のミックスインを選択し、各ミックスインを確認する前に右側のレールのリストに追加します。 また、現在選択されているミックスインの右側にアイコンが表示され、そのフィールドの構造をプレビューできます。

![](../images/tutorials/create-schema/preview-mixin-button.png)

Mixinをプレビューする際、Mixinのスキーマの詳細が右側のパネルに表示されます。 また、提供されたキャンバス内のMixinのフィールド間を移動することもできます。 別のフィールドを選択すると、右側のパネルが更新され、目的のフィールドに関する詳細が表示されます。 プレビューが終了したら **[!UICONTROL 「戻る]** 」を選択して、mixinの選択ダイアログに戻ります。

![](../images/tutorials/create-schema/preview-mixin.png)

このチュートリアルでは、「 **[!UICONTROL 人口統計詳細]** 」ミックスインを選択し、「ミックスイン」を選択し ****&#x200B;ます。

![](../images/tutorials/create-schema/add_mixin_person_details.png)

スキーマキャンバスが再び表示されます。The **[!UICONTROL Mixins]** section now lists &quot;[!UICONTROL Demographic Details]&quot; and the **[!UICONTROL Structure]** section includes the fields contributed by the mixin. 「ミックスイン **** 」セクションの下でミックスインの名前を選択して、キャンバス内で提供される特定のフィールドを強調表示することができます。

![](../images/tutorials/create-schema/person_details_structure.png)

This mixin contributes several fields under the top-level name `person` with the data type &quot;[!UICONTROL Person]&quot;. このフィールドグループは、名前、生年月日、性別など、個人に関する情報を説明します。

>[!NOTE]
>
>Remember that fields may use scalar types (such as string, integer, array, or date), as well as any data type (a group of fields representing a common concept) defined within the [!DNL Schema Registry].

Notice that the `name` field has a data type of &quot;[!UICONTROL Person name]&quot;, meaning it too describes a common concept and contains name-related sub-fields such as first name, last name, courtesy title, and suffix.

キャンバス内の別のフィールドを選択すると、スキーマ構造に影響を与える追加のフィールドが表示されます。

## 別の mixin の追加 {#mixin-2}

次に、同じ手順を繰り返して、別の mixin を追加できます。When you view the **[!UICONTROL Add mixin]** dialog this time, notice that the &quot;[!UICONTROL Demographic Details]&quot; mixin has been greyed out and the checkbox next to it cannot be selected. これは、既に現在のスキーマに含まれている mixin を誤って複製するのを防ぎます。

このチュートリアルでは、ダイアログから「[!DNL Personal Contact Details]」ミックスインを選択し、「 **[!UICONTROL mixin]** 」を選択してスキーマに追加します。

![](../images/tutorials/create-schema/add_mixin_personal_details.png)

追加すると、キャンバスが再び表示されます。&quot;[!UICONTROL Personal Contact Details]&quot; is now listed under **[!UICONTROL Mixins]** in the **[!UICONTROL Composition]** section, and fields for home address, mobile phone, and more have been added under **[!UICONTROL Structure]**.

Similar to the `name` field, the fields you just added represent multi-field concepts. For example, `homeAddress` has a data type of &quot;[!UICONTROL Postal address]&quot; and `mobilePhone` has a data type of &quot;[!UICONTROL Phone number]&quot;. これらの各フィールドを選択して展開し、データ型に含まれる追加のフィールドを表示できます。

![](../images/tutorials/create-schema/personal_details_structure.png)

## 新しい mixin の定義 {#define-mixin}

The &quot;[!UICONTROL Loyalty Members]&quot; schema is meant to capture data related to the members of a loyalty program, so it will require some specific loyalty-related fields. 必要なフィールドを含む標準 mixin がないので、新しい mixin を定義する必要があります。

今回は、**[!UICONTROL Mixin の追加]**&#x200B;ダイアログを開いたときに、「**[!UICONTROL 新規 mixin の作成]**」を選択します。その後、ミックスインの表示名と説明を入力するように求められます。

![](../images/tutorials/create-schema/mixin_create_new.png)

クラス名と同様に、mixin 名は短く単純で、mixin がスキーマに与える影響を説明するべきです。これらも一意なので、名前を再利用できません。名前が十分に具体的であるようにしてください。

このチュートリアルでは、新しい mixin に「Loyalty Details」という名前を付けます。

に戻る **[!UICONTROL には]** 、「 [!DNL Schema Editor]mixin」を選択します。 &quot;[!UICONTROL Loyalty Details]&quot; should now appear under **[!UICONTROL Mixins]** on the left-side of the canvas, but there are no fields associated with it yet and therefore no new fields appear under **[!UICONTROL Structure]**.

## Mixin へのフィールドの追加 {#mixin-fields}

「Loyalty Details」mixin を作成したら、mixin がスキーマに貢献するフィールドを定義します。

To begin, select the mixin name in the **[!UICONTROL Mixins]** section. これを行うと、Mixinのプロパティがエディタの右側に表示され、「 **構造** 」の下のスキーマ名の横に **[!UICONTROL プラス(+)]**&#x200B;アイコンが表示されます。

![](../images/tutorials/create-schema/loyalty_details_structure.png)

「 **」の横の** プラス(+)[!DNL Loyalty Members]アイコンを選択して、構造内に新しいノードを作成します。 This node (called `_tenantId` in this example) represents your IMS Organization&#39;s tenant ID, preceded by an underscore. テナント ID の存在は、追加するフィールドが組織の名前空間に限られていることを示しています。

つまり、追加するフィールドは組織に固有のものであり、組織にのみアクセス可能な特定の領域 [!DNL Schema Registry] に保存されます。 定義するフィールドは、他の標準クラス、ミックスイン、データ型、およびフィールドの名前との競合を防ぐために、常にテナント名前空間に追加する必要があります。

Inside that namespaced node is a &quot;[!UICONTROL New Field]&quot;. This is the beginning of the &quot;[!UICONTROL Loyalty Details]&quot; mixin.

![](../images/tutorials/create-schema/new_field_loyalty.png)

Using the controls on the right-hand side of the editor, start by creating a `loyalty` field with type &quot;[!UICONTROL Object]&quot; that will be used to hold your loyalty-related fields. When finished, select **[!UICONTROL Apply]**.

![](../images/tutorials/create-schema/loyalty_object.png)

The changes are applied and the newly created `loyalty` object appears. オブジェクトの横にある **プラス(+)** アイコンを選択して、忠誠度関連のフィールドを追加します。 A &quot;[!UICONTROL New Field]&quot; appears and the **[!UICONTROL Field properties]** section is visible on the right-hand side of the canvas.

![](../images/tutorials/create-schema/new_field_in_loyalty_object.png)

各フィールドには、次の情報が必要です。

* **[!UICONTROL フィールド名]:** フィールドの名前。キャメルケースで書かれます。 例：loyaltyLevel
* **[!UICONTROL 表示名]:** フィールドの名前。タイトルの場合は大文字で記述されます。 例：Loyalty Level
* **[!UICONTROL タイプ]:** フィールドのデータ型です。 This includes basic scalar types and any data types defined in the [!DNL Schema Registry]. Examples: [!UICONTROL String], [!UICONTROL Integer], [!UICONTROL Boolean], [!UICONTROL Person], [!UICONTROL Address], [!UICONTROL Phone number], etc.
* **[!UICONTROL 説明]:** フィールドのオプションの説明は、文頭の場合で記述し、最大200文字で入力します。

The first field for the `Loyalty` object will be a string called `loyaltyId`. When setting the new field&#39;s type to &quot;[!UICONTROL String]&quot;, the **[!UICONTROL Field properties]** section becomes populated with several options for applying constraints, including default value, format, and maximum length.

![](../images/tutorials/create-schema/string_constraints.png)

選択したデータ型に応じて、様々な制約オプションを使用できます。Since `loyaltyId` will be an email address, select &quot;[!UICONTROL email]&quot; from the **[!UICONTROL Format]** dropdown menu. 「**[!UICONTROL 適用]**」を選択して変更を適用します。

![](../images/tutorials/create-schema/loyaltyId_field.png)

## Add more fields to the mixin {#mixin-fields-2}

Now that you have added the `loyaltyId` field, you can add additional fields to capture loyalty-related information such as:

* ポイント（整数）
* 会員登録日

各フィールドをスキーマに追加するには、オ **ブジェクトの横のプラス(+)**`loyalty` アイコンを選択し、必要な情報を入力します。

完了すると、忠誠度オブジェクトには、忠誠度ID、ポイント、およびメンバー登録用のフィールドが含まれます。

![](../images/tutorials/create-schema/loyalty_object_fields.png)

## mixin追加の列挙フィールド {#enum}

When defining fields in the [!DNL Schema Editor], there are some additional options that you can apply to basic field types in order to provide further constraints on the data the field can contain. これらの制約の使用例を次の表に示します。

| 制約 | 説明 |
| --- | --- |
| [!UICONTROL 必須] | フィールドがデータ取り込みに必要であることを示します。 このフィールドを含まないデータセットに基づいてスキーマセットにアップロードされたデータの取得は失敗します。 |
| [!UICONTROL 配列] | フィールドに値の配列が含まれ、各値は指定されたデータ型になっていることを示します。 例えば、データ型が「[!UICONTROL String]」のフィールドに対してこの制約を使用すると、フィールドに文字列の配列が含まれるように指定できます。 |
| [!UICONTROL Enum] | このフィールドに、可能な値の列挙リストの値の1つを含める必要があることを示します。 |
| [!UICONTROL ID] | このフィールドがIDフィールドであることを示します。 ID フィールドの詳細については、[このチュートリアルの後半](#identity-field)で説明します。 |
| [!UICONTROL Relationship] | スキーマの関係は、和集合スキーマを使用して推論できますが、 [!DNL Real-time Customer Profile]これは同じクラスを共有するスキーマにのみ適用されます。 [!UICONTROL Relationship] 制約は、このフィールドが、異なるクラスに基づくスキーマの主なIDを参照することを示し、2つのスキーマ間の関係を示します。 See the tutorial on [defining a relationship](./relationship-ui.md) for more information. |

このチュートリアルでは、スキーマの [!DNL "loyalty"] オブジェクトに、顧客の「忠誠度レベル」を説明する新しい列挙フィールドが必要です。値は、4つのオプションのうちの1つに限定できます。 このフィールドをスキーマに追加するには、 **オブジェクトの横にある** プラス(+) `loyalty` アイコンを選択し、「フィールド名 **** 」と「 **[!UICONTROL 表示名]**」の必須フィールドに入力します。 「 **[!UICONTROL タイプ]**」で「[!UICONTROL 文字列]」を選択します。

![](../images/tutorials/create-schema/loyalty-level-type.png)

タイプを選択した後、 **[!UICONTROL Array]**、 **[!UICONTROL Enum]**、 **** Identityのチェックボックスなど、フィールドに追加のチェックボックスが表示されます。

Select the **[!UICONTROL Enum]** checkbox to open the **[!UICONTROL Enum values]** section below. ここで、許容可能な各ロイヤルティレベルの&#x200B;**[!UICONTROL 値]**（キャメルケース）と&#x200B;**[!UICONTROL ラベル]**（タイトルケースでの読みやすい名前でオプション）を入力できます。

すべてのフィールドプロパティの入力が完了したら、「 **[!UICONTROL Apply]** 」を選択して「[!DNL loyaltyLevel]」フィールドを `loyalty` オブジェクトに追加します。

![](../images/tutorials/create-schema/loyalty_level_enum.png)

## 複数フィールドオブジェクトのデータ型への変換 {#datatype}

この `loyalty` オブジェクトには複数の忠誠度固有のフィールドが含まれ、他のスキーマで役立つ共通のデータ構造を表すようになりました。 では、これら [!DNL Schema Editor] のオブジェクトの構造をデータ型に変換することで、再利用可能な複数フィールドオブジェクトを簡単に適用できます。

データ型を使用すると、複数フィールド構造を一貫して使用でき、mixin よりも柔軟性が高まります。これは、データ型がスキーマ内のどこでも使用できるからです。これは、フィールドの **[!UICONTROL Type]** 値を、で定義されている任意のデータ型の値に設定することで行われ [!DNL Schema Registry]ます。

To convert the `loyalty` object to a data type, select the `loyalty` field under **[!UICONTROL Structure]**, then select **[!UICONTROL Convert to new data type]** on the right-hand side of the editor under **[!UICONTROL Field properties]**. オブジェクトが正常に変換されたことを確認する緑のポーバーが表示されます。

![](../images/tutorials/create-schema/convert-data-type.png)

Now, when you look under **[!UICONTROL Structure]**, you can see that the `loyalty` field has a data type of &quot;[!DNL Loyalty]&quot; and the fields have small lock icons beside them, indicating they are no longer individual fields but rather part of a multi-field data type.

![](../images/tutorials/create-schema/loyalty_data_type.png)

今後のスキーマでは、フィールドを「[!DNL Loyalty]」タイプとして割り当て、ID、忠誠度レベル、メンバーシンク、ポイントのフィールドが自動的に含まれるようになります。

>[!NOTE]
>
>カスタムデータタイプは、編集スキーマとは独立して作成および編集することもできます。 詳しくは、データタイプの [作成と編集に関するチュートリアル](./create-data-type.md) を参照してください。

## スキーマフィールドの検索とフィルター

スキーマには、基本クラスが提供するフィールドに加えて、複数のミックスインが含まれるようになりました。 大きいスキーマを使用する場合は、左側のレールでmixin名の横にあるチェックボックスをオンにして、表示されるフィールドを、関心のあるミックスインで提供されるフィールドのみにフィルタリングできます。

![](../images/tutorials/create-schema/filter-by-mixin.png)

スキーマ内で特定のフィールドを探している場合は、検索バーを使用して、表示されるフィールドを、どのミックスインがどの下に提供されているかに関係なく、名前でフィルターすることもできます。

![](../images/tutorials/create-schema/search.png)

>[!IMPORTANT]
>
>検索機能は、一致するフィールドを表示する際に、選択したミックスインフィルターを考慮に入れます。 検索クエリに期待した結果が表示されない場合は、関連するミックスインがフィルターで除外されていないことを重複チェックが必要になる場合があります。

## ID フィールドとしてのスキーマフィールドの設定 {#identity-field}

スキーマが提供する標準的なデータ構造を活用して、複数のソースにわたって同じ個人に属するデータを識別でき、分類、レポート、データサイエンスの分析など、様々な下流の使用例に対応できます。 個々のIDに基づいてデータを結合するには、該当するスキーマ内でキーフィールドを [!UICONTROL ID] フィールドとしてマークする必要があります。

[!DNL Experience Platform] で「 **[!UICONTROL ID]** 」チェックボックスを使用して、IDフィールドを簡単に示すことがで [!DNL Schema Editor]きます。 ただし、データの性質に基づいて、IDとして使用するのに最適なフィールドを決定する必要があります。

For example, there may be thousands of loyalty program members belonging to the same &quot;loyalty level&quot;, but each member of the loyalty program has a unique `loyaltyId` (which in this instance is the individual member&#39;s email address). The fact that `loyaltyId` is a unique identifier for each member makes it a good candidate for an identity field, whereas `loyaltyLevel` is not.

>[!IMPORTANT]
>
>次の手順は、既存のスキーマフィールドにID記述子を追加する方法を説明します。 スキーマ自体の構造内にIDフィールドを定義する代わりに、 `identityMap` フィールドを使用してID情報を含めることもできます。
>
>を使用する場合は、スキーマに直接追加するすべてのプライマリIDが上書きされることに注意して `identityMap`ください。 詳しくは、『スキーマコンポジションの `identityMap` 基本』ガイドののの節を参照してください [](../schema/composition.md#identityMap) 。

エディタの「 **[!UICONTROL 構造]** 」セクションで、 `loyaltyId` フィールドを選択し、「フィールドプロパティ **[!UICONTROL 」の下に「]** ID ****」チェックボックスが表示されます。 Check the box and the option to set this as the **[!UICONTROL Primary identity]** appears. このボックスも選択します。

>[!NOTE]
>
>各スキーマには、1 つのプライマリ ID フィールドのみを含めることができます。スキーマフィールドをプライマリIDとして設定すると、後でスキーマ内の別のIDフィールドをプライマリIDとして設定しようとすると、エラーメッセージが表示されます。

次に、ドロップダウン内の事前定義済みの **[!UICONTROL 名前空間のリストから]** ID名前空間を指定する必要があります。 は顧客の電子メ `loyaltyId` ールアドレスなので、ドロップダウンから「[!UICONTROL Email]」を選択します。 「 **[!UICONTROL 適用]** 」を選択して、 `loyaltyId` フィールドの更新を確認します。

![](../images/tutorials/create-schema/loyaltyId_primary_identity.png)

>[!NOTE]
>
>標準名前空間とその定義のリストについては、 [[!DNL Identity Service] ドキュメントを参照してください](../../identity-service/troubleshooting-guide.md#standard-namespaces)。

変更を適用した後、のアイコンには指紋の記号が表示され、それが現在のIDフィールドであることが示されます。 `loyaltyId`

![](../images/tutorials/create-schema/identity-applied.png)

Now all data ingested into the `loyaltyId` field will be used to help identify that individual and stitch together a single view of that customer. To learn more about working with identities in [!DNL Experience Platform], please review the [[!DNL Identity Service]](../../identity-service/home.md) documentation.

## スキーマを [!DNL Real-time Customer Profile] {#profile}

[[!DNL Real-time Customer Profile]](../../profile/home.md) でアイデンティティデータ [!DNL Experience Platform] を活用して、各顧客の全体的な表示を提供します。 このサービスは、堅牢で360°の顧客属性プロファイルを構築し、お客様がと統合されたあらゆるシステムにわたって持つすべてのインタラクション顧客のタイムスタンプのあるアカウントを作成 [!DNL Experience Platform]します。

In order for a schema to be enabled for use with [!DNL Real-time Customer Profile], it must have a primary identity defined. 最初にプライマリIDを定義せずにスキーマを有効にしようとすると、エラーメッセージが表示されます。

<img src="../images/tutorials/create-schema/missing_primary_identity.png" width="600" /><br>

To enable the &quot;Loyalty Members&quot; schema for use in [!DNL Profile], begin by selecting &quot;[!DNL Loyalty Members]&quot; in the **[!UICONTROL Structure]** section of the editor.

エディターの右側には、スキーマに関する情報（表示名、説明、タイプなど）が表示されます。 この情報に加えて、 **[!UICONTROL プロファイル]** 切り替えボタンもあります。

![](../images/tutorials/create-schema/profile-toggle.png)

Select **[!UICONTROL Profile]** and a popover appears, asking you to confirm that you wish to enable the schema for [!DNL Profile].

<img src="../images/tutorials/create-schema/enable-profile.png" width="700" /><br>

>[!WARNING]
>
>Once a schema has been enabled for [!DNL Real-time Customer Profile] and saved, it cannot be disabled.

「 **[!UICONTROL 有効にする]** 」を選択して選択を確定します。 必要に応じて、 **[!UICONTROL プロファイルの切り替えを再度選択してスキーマを無効にすることができますが、有効にした状態でスキーマを保存すると]**[!DNL Profile] 、無効にすることはできません。

## 次の手順とその他のリソース

スキーマの構成が完了すると、カンバスに完全なスキーマが表示されます。 「 **[!UICONTROL 保存]** 」を選択すると、スキーマがに保存され [!DNL Schema Library]、がアクセスできるようになり [!DNL Schema Registry]ます。

これで、新しいスキーマを使用してにデータを取り込むことができ [!DNL Platform]ます。 データの取得にスキーマを使用した後は、追加的な変更のみがおこなわれる場合があります。スキーマバージョン管理について詳しくは、「[スキーマ合成の基本](../schema/composition.md)」を参照してください。

これで、UIでスキーマの関係を [定義するチュートリアルに従って、「Loyalty Members](./relationship-ui.md) 」スキーマに新しい関係フィールドを追加できます。

The &quot;Loyalty Members&quot; schema is also available to be viewed and managed using the [!DNL Schema Registry] API. To begin working with the API, start by reading the [[!DNL Schema Registry API] developer guide](../api/getting-started.md).

### ビデオリソース

>[!WARNING]
>
>次のビデオに示す [!DNL Platform] UIは古いです。 最新のUIのスクリーンショットと機能については、上記のドキュメントを参照してください。

次のビデオでは、 [!DNL Platform] UIで単純なスキーマを作成する方法を示します。

>[!VIDEO](https://video.tv.adobe.com/v/27012?quality=12&learn=on)

次のビデオは、ミックスインとクラスを使用する際の理解を深めるためのものです。

>[!VIDEO](https://video.tv.adobe.com/v/27013?quality=12&learn=on)

## 付録

以下の節では、の使用に関する追加情報について説明し [!DNL Schema Editor]ます。

### 新しいクラスの作成 {#create-new-class}

[!DNL Experience Platform] は、組織に固有のクラスに基づいてスキーマを定義する柔軟性を提供します。

[ **[!UICONTROL スキーマ]****ワークスペースで[スキーマの]**&#x200B;作成 **を選択し、ドロップダウンから[]** 参照]を選択します。

![](../images/tutorials/create-schema/browse-classes.png)

使用可能なクラスのリストから選択できるダイアログが表示されます。 ダイアログの上部で、「 **[!UICONTROL 新しいクラスを作成]**」を選択します。 You can then give your new class a display name (a short, descriptive, unique, and user-friendly name for the class), a description, and a behavior (&quot;[!UICONTROL Record]&quot; or &quot;[!UICONTROL Time Series]&quot;) for the data the schema will define.

![](../images/tutorials/create-schema/create_new_class.png)

>[!IMPORTANT]
>
> 組織で定義されたクラスを実装するスキーマを構築する場合、mixin は互換性のあるクラスでのみ使用できることに注意してください。Since the class you defined is new, there are no compatible mixins listed in the **[!UICONTROL Add mixin]** dialog. Instead, you will need to select **[!UICONTROL Create new mixin]** and define a mixin for use with that class. 次に新しいクラスを実装するスキーマを作成すると、定義した mixin が一覧表示され、使用できます。

### スキーマクラスの変更 {#change-class}

スキーマのクラスは、スキーマが保存される前の最初の構成プロセスの任意の時点で変更できます。

>[!WARNING]
>
>スキーマに対するクラスの再割り当ては、細心の注意を払って行う必要があります。 ミックスインは特定のクラスとのみ互換性があるので、クラスを変更するとキャンバスと追加したフィールドがリセットされます。

クラスを再割り当てするには、キャンバスの左側で **[!UICONTROL 「割り当て]** 」を選択します。

![](../images/tutorials/create-schema/assign_class_button.png)

A dialog appears that displays a list of all available classes, including any defined by your organization (the owner being &quot;[!UICONTROL Customer]&quot;) as well as standard classes defined by Adobe.

リストからクラスを選択し、ダイアログの右側に説明を表示します。 You can also select **[!UICONTROL Preview class structure]** to see the fields and metadata associated with the class. 続行するには、[ **[!UICONTROL クラスを割り当て]** ]を選択します。

![](../images/tutorials/create-schema/assign_class.png)

新しいクラスを割り当てるかどうかを確認する新しいダイアログが開きます。 「 **[!UICONTROL 割り当て]** 」を選択して確認します。

![](../images/tutorials/create-schema/assign-confirm.png)

クラスの変更を確認した後、キャンバスはリセットされ、構成の進行状況はすべて失われます。