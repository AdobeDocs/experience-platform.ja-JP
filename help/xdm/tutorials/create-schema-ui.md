---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマエディターを使用したスキーマの作成
topic: tutorials
translation-type: tm+mt
source-git-commit: 661789fa15ea11b0e42060b1b90d74785c04fa1f
workflow-type: tm+mt
source-wordcount: '3376'
ht-degree: 56%

---


# スキーマの作成には、 [!DNL Schema Editor]

The [!DNL Schema Registry] provides a user interface and RESTful API from which you can view and manage all resources in the Adobe Experience Platform [!DNL Schema Library]. The [!DNL Schema Library] contains resources made available to you by Adobe, Experience Platform partners, and vendors whose applications you use, as well as resources that you define and save to the [!DNL Schema Registry].

This tutorial covers the steps for creating a schema using the Schema Editor within [!DNL Experience Platform]. スキーマレジストリ API を使用してスキーマを作成する場合は、『[API を使用したスキーマの作成](create-schema-api.md)』チュートリアルを読む前に、『[スキーマレジストリ開発者ガイド](../api/getting-started.md)』を読んでください。

このチュートリアルでは、[新しいクラスを定義](#create-new-class)し、そのクラスを使用してスキーマを作成する手順も説明します。

## はじめに

このチュートリアルでは、スキーマエディターの使用に関わる Adobe Experience Platform の様々な側面について、十分に理解しておく必要があります。このチュートリアルを始める前に、次の概念に関するドキュメントを確認してください。

* [!DNL Experience Data Model (XDM)](../home.md): Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
* [スキーマ合成の基本](../schema/composition.md)：XDM スキーマとその構築ブロック（クラス、mixin、データ型、フィールドなど）の概要です。
* [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

This tutorial requires you to have access to [!DNL Experience Platform]. If you do not have access to an IMS Organization in [!DNL Experience Platform], please speak to your system administrator before proceeding.

## スキーマワークスペースでの既存のスキーマの参照 {#browse}

The Schemas workspace within [!DNL Experience Platform] provides a visualization of the [!DNL Schema Library], allowing you to view and manage all of the schemas available to you, as well as compose new ones. ワークスペースには、このチュートリアル全体でスキーマを作成するキャンバスであるスキーマエディターも含まれています。

After logging into [!DNL Experience Platform], click **[!UICONTROL Schemas]** in the left-hand navigation and you will be taken to the Schemas workspace. You will see a list of schemas (a representation of the [!DNL Schema Library]) where you can view, manage, and customize all schemas available to you. リストには、スキーマの基となる名前、型、クラス、動作（レコードまたは時系列）、およびスキーマが最後に変更された日時が含まれます。

クラス、mixin、データ型を含む、レジストリ内のすべてのリソースに対してフィルター機能を使用するには、検索バーの横にあるフィルターアイコンをクリックします。

![スキーマライブラリの表示](../images/tutorials/create-schema/schemas_filter.png)

## スキーマの作成と命名 {#create}

スキーマの合成を開始するには、スキーマワークスペースの右上隅にある「**[!UICONTROL スキーマを作成]**」をクリックします。

![スキーマ作成ボタン](../images/tutorials/create-schema/create_schema_button.png)

*スキーマエディタ*&#x200B;が表示されます。これは、スキーマを作成するキャンバスです。エディターに到達すると、キャンバスの「*構造*」セクションに「名称未設定スキーマ」が自動的に作成されており、カスタマイズを開始できます。

![スキーマエディター](../images/tutorials/create-schema/schema_editor.png)

エディターの右側には&#x200B;*スキーマプロパティ*&#x200B;があり、スキーマの名前を指定できます（**[!UICONTROL 名前を表示]**&#x200B;フィールドを使用）。名前を入力すると、キャンバスが更新され、スキーマの新しい名前が反映されます。

![スキーマキャンバス](../images/tutorials/create-schema/name_schema.png)

スキーマの名前を決定する際に考慮すべき重要な点がいくつかあります。

* 後でスキーマをライブラリ内で簡単に見つけられるように、スキーマ名は短く説明的な名前にする必要があります。
* スキーマ名は一意である必要があります。つまり、将来再利用されないように十分に具体的でなければなりません。例えば、組織が異なるブランドに対して別々のロイヤルティプログラムを持つ場合、後で定義する他のロイヤルティ関連スキーマと区別しやすいように、スキーマに「ブランド A ロイヤルティメンバー」という名前を付けると効果的です。
* オプションで、「**[!UICONTROL 説明]**」フィールドを使用してスキーマに関する追加情報を入力できます。

このチュートリアルでは、ロイヤルティプログラムのメンバーに関連するデータを取得するスキーマを合成するので、このスキーマの名前は「ロイヤルティメンバー」になります。

## クラスの割り当て {#class}

エディターの左側には、「*合成*」セクションがあります。現在、「*[!UICONTROL スキーマ]*」と「*[!UICONTROL クラス]*」の 2 つのサブセクションが含まれています。

これで、スキーマに名前が付いたので、スキーマが実装するクラスを割り当てます。「*[!UICONTROL クラス]*」の横の「**[!UICONTROL 割り当て]**」をクリックします。

![](../images/tutorials/create-schema/assign_class_button.png)

*[!UICONTROL クラスの割り当て]*&#x200B;ダイアログが表示されます。このウィンドウには、組織で定義されている任意のクラス（所有者は「顧客」）と、アドビで定義されている標準クラスを含む、使用可能なすべてのクラスのリストが表示されます。

クラス名をクリックすると、クラスの説明が表示されます。「**[!UICONTROL クラス構造をプレビュー]**」を選択して、クラスに関連付けられたフィールドとメタデータを表示することもできます。

This tutorial uses the [!DNL XDM Individual Profile] class. クラスの横のラジオボタンをクリックして選択し、「**[!UICONTROL クラスを割り当て]**」をクリックします。

![クラスの割り当てダイアログ](../images/tutorials/create-schema/assign_class.png)

キャンバスが再び表示されます。The *[!UICONTROL Class]* section now contains the class you selected ([!DNL XDM Individual Profile]) and the fields contributed by the [!DNL XDM Individual Profile] class are now visible within the *[!UICONTROL Structure]* section.

![XDM Individual Profile クラスの割り当て](../images/tutorials/create-schema/class_assigned_structure.png)

フィールドは「fieldName | Data Type」の形式で表示されます。UI でスキーマフィールドを定義する手順は、このチュートリアルで後述します。

>[!NOTE]
>
> スキーマが保存される前の初期構成プロセス中の任意の時点で[スキーマのクラスを変更](#change-class)できますが、これは非常に注意しておこなう必要があります。Mixin は特定のクラスとのみ互換性があるので、クラスを変更するとキャンバスと追加したフィールドがリセットされます。

## Mixin の追加  {#mixin}

クラスが割り当てられたので、 *合成*&#x200B;セクションに 3 つ目のサブセクション（*[!UICONTROL mixin]*）が含まれます。

これで、mixin を追加して、スキーマにフィールドを追加できます。Mixin は、特定の概念を説明する 1 つ以上のフィールドのグループです。このチュートリアルでは、mixin を使用してロイヤルティプログラムのメンバーを説明し、名前、誕生日、電話番号、住所などの重要な情報を取得します。

Mixin を追加するには、「*Mixin*」サブセクションで「**追加**」をクリックします。

![](../images/tutorials/create-schema/add_mixin_button.png)

*[!UICONTROL Mixin を追加]*&#x200B;ダイアログが表示されます。Mixins are only intended for use with specific classes, therefore the list of mixins shows only those compatible with the class you selected (in this case, the [!DNL XDM Individual Profile] class).

Mixin の横にあるラジオボタンを選択すると、**[!UICONTROL Mixin の構造をプレビュー]**&#x200B;できます。「Profile Person Details」mixin を選択し、「**[!UICONTROL Mixin を追加]**」をクリックします。

![](../images/tutorials/create-schema/add_mixin_person_details.png)

スキーマキャンバスが再び表示されます。The *[!UICONTROL Mixins]* section now lists the &quot;[!UICONTROL Profile Person Details]&quot; mixin and the *[!UICONTROL Structure]* section includes the fields contributed by the mixin.

![](../images/tutorials/create-schema/person_details_structure.png)

この mixin は、最上位の名前「person」の下に、データ型「Person」を持つ複数のフィールドを表示します。このフィールドグループは、名前、生年月日、性別など、個人に関する情報を説明します。

>[!NOTE]
>
>Remember that fields may use scalar types (such as string, integer, array, or date) as their data type, as well as any &quot;data type&quot; (a group of fields representing a common concept) in the [!DNL Schema Registry].

Notice that the &quot;[!UICONTROL name]&quot; field has a data type of &quot;[!UICONTROL Person Name]&quot;, meaning it too describes a common concept and contains name-related sub-fields such as first name, last name, and full name.

キャンバス内のさまざまなフィールドをクリックして、スキーマ構造に寄与する追加のフィールドを表示します。

## 別の mixin の追加 {#mixin-2}

次に、同じ手順を繰り返して、別の mixin を追加できます。When you view the *[!UICONTROL Add Mixin]* dialog this time, notice that the &quot;[!UICONTROL Profile Person Details]&quot; mixin has been greyed out and the radio button next to it cannot be selected. これは、既に現在のスキーマに含まれている mixin を誤って複製するのを防ぎます。

You can now add the &quot;[!DNL Profile Personal Details" mixin] from the *[!UICONTROL Add Mixin]* dialog.

![](../images/tutorials/create-schema/add_mixin_personal_details.png)

追加すると、キャンバスが再び表示されます。The &quot;[!UICONTROL Profile Personal Details]&quot; is now listed under *[!UICONTROL Mixins]* in the *[!UICONTROL Composition]* section, and fields for home address, mobile phone, and more have been added under *[!UICONTROL Structure]*.

Similar to the &quot;[!UICONTROL name]&quot; field, the fields you just added represent multi-field concepts. For example, &quot;[!UICONTROL homeAddress]&quot; has a data type of &quot;[!UICONTROL Address]&quot; and &quot;[!UICONTROL mobilePhone]&quot; has a data type of &quot;[!UICONTROL Phone Number]&quot;. これらの各フィールドをクリックして展開し、データ型に含まれる追加のフィールドを確認できます。

![](../images/tutorials/create-schema/personal_details_structure.png)

## 新しい mixin の定義 {#define-mixin}

The &quot;[!UICONTROL Loyalty Members]&quot; schema is meant to capture data related to the members of a loyalty program, so it will require some specific loyalty-related fields. 必要なフィールドを含む標準 mixin がないので、新しい mixin を定義する必要があります。

今回は、*[!UICONTROL Mixin の追加]*&#x200B;ダイアログを開いたときに、「**[!UICONTROL 新規 mixin の作成]**」を選択します。その後、mixin の&#x200B;**[!UICONTROL 表示名]**&#x200B;と&#x200B;**[!UICONTROL 説明]**&#x200B;を入力するよう求められます。

![](../images/tutorials/create-schema/mixin_create_new.png)

クラス名と同様に、mixin 名は短く単純で、mixin がスキーマに与える影響を説明するべきです。これらも一意なので、名前を再利用できません。名前が十分に具体的であるようにしてください。

For this tutorial, name the new mixin &quot;[!UICONTROL Loyalty Details]&quot;.

「**[!UICONTROL Mixin を追加]**」をクリックして、スキーマエディターに戻ります。&quot;[!UICONTROL Loyalty Details]&quot; should now appear under *[!UICONTROL Mixins]* on the left-side of the canvas, but there are no fields associated with it yet and therefore no new fields appear under *[!UICONTROL Structure]*.

## Mixin へのフィールドの追加 {#mixin-fields}

Now that you have created the &quot;[!UICONTROL Loyalty Details]&quot; mixin, it is time to define the fields that the mixin will contribute to the schema.

まず、「*[!UICONTROL Mixin]*」セクションの mixin 名をクリックします。この操作をおこなうと、「*[!UICONTROL Mixin プロパティ]*」がエディターの右側に表示されて、「**[!UICONTROL フィールドの追加]**」ボタンが「*[!UICONTROL 構造]*」の下でスキーマ名の横に表示されます。。

![](../images/tutorials/create-schema/loyalty_details_structure.png)

Click **[!UICONTROL Add Field]** next to &quot;[!UICONTROL Loyalty Members]&quot; to create a new node in the structure. このノード（この例では「_tenantId」と呼ばれます）は、IMS 組織のテナント ID を表し、前にアンダースコアが付いています。テナント ID の存在は、追加するフィールドが組織の名前空間に限られていることを示しています。

In other words, the fields you are adding are unique to your organization and are going to be saved in the [!DNL Schema Registry] in a specific area accessible only to your IMS Org. Fields you define must always be added to your namespace to prevent collisions with names from other standard classes, mixins, data types, and fields.

Inside that namespaced node is a &quot;[!UICONTROL New Field]&quot;. This is the beginning of the &quot;[!UICONTROL Loyalty Details]&quot; mixin.

![](../images/tutorials/create-schema/new_field_loyalty.png)

Using *[!UICONTROL Field Properties]* on the right-hand side of the editor, start by creating a &quot;[!UICONTROL loyalty]&quot; field with type &quot;[!UICONTROL Object]&quot; that will be used to hold your loyalty-related fields. 完了したら、「**[!UICONTROL 適用]**」をクリックします。

![](../images/tutorials/create-schema/loyalty_object.png)

The changes are applied and the newly created &quot;[!UICONTROL loyalty]&quot; object appears. オブジェクトの横にある「**[!UICONTROL フィールドの追加]**」をクリックして、その他のロイヤルティ関連のフィールドを追加します。「新規フィールド」が表示され、「*[!UICONTROL フィールドプロパティ]*」セクションがキャンバスの右側に表示されます。

![](../images/tutorials/create-schema/new_field_in_loyalty_object.png)

各フィールドには、次の情報が必要です。

* **[!UICONTROL フィールド名]:**フィールドの名前。キャメルケースで書かれます。 例：loyaltyLevel
* **[!UICONTROL 表示名]:**フィールドの名前。タイトルの場合は大文字で記述されます。 例：Loyalty Level
* **[!UICONTROL タイプ]:**フィールドのデータ型です。 This includes basic scalar types and any data types defined in the[!DNL Schema Registry]. 例：文字列、整数、ブール値、人、住所、電話番号など
* **[!UICONTROL 説明]:**フィールドのオプションの説明を文頭の場合と同様に記述します。 （最大 200 文字）

The first field for the Loyalty object will be a string called &quot;[!UICONTROL loyaltyId]&quot;. When setting the new field&#39;s type to &quot;[!UICONTROL String]&quot;, the *[!UICONTROL Field Properties]* window becomes populated with several options for applying constraints, including **[!UICONTROL Default Value]**, **[!UICONTROL Format]**, and **[!UICONTROL Maximum Length]**.

![](../images/tutorials/create-schema/string_constraints.png)

選択したデータ型に応じて、様々な制約オプションを使用できます。Since &quot;[!UICONTROL loyaltyId]&quot; will be an email address, select &quot;[!UICONTROL email]&quot; from the **[!UICONTROL Format]** dropdown menu. 「**[!UICONTROL 適用]**」を選択して変更を適用します。

![](../images/tutorials/create-schema/loyaltyId_field.png)

## Mixin への他のフィールドの追加 {#mixin-fields-2}

Now that you have added the &quot;[!UICONTROL loyaltyId]&quot; field, you can add additional fields to capture loyalty-related information such as:

* ポイント（整数）
* メンバー登録日（日付）

フィールドを追加するには、loyalty オブジェクトで「**[!UICONTROL フィールドの追加]**」をクリックして、必要な情報を入力します。

完了すると、Loyalty オブジェクトに、Loyalty ID、ポイント、メンバー登録日のフィールドが含まれます。

![](../images/tutorials/create-schema/loyalty_object_fields.png)

## Mixin への「enum」フィールドの追加 {#enum}

スキーマエディターでフィールドを定義する場合、フィールドに含めることができるデータにさらに制約を加えるために、基本的なフィールドタイプに適用できるいくつかの追加オプションがあります。

An example of this would be a &quot;[!UICONTROL Loyalty Level]&quot; field, where the value can only be one of four possible options. To add this field to the schema, click **[!UICONTROL Add Field]** beside the &quot;[!UICONTROL loyalty]&quot; object and fill in the required fields under *[!UICONTROL Field Properties]*.

「**[!UICONTROL 型]**」で「String」を選択すると、「**[!UICONTROL 配列]**」、「**[!UICONTROL 列挙体]**」、「**[!UICONTROL ID]**」の追加のチェックボックスが表示されます。

「**[!UICONTROL 列挙]**」チェックボックスを選択し 、下の「*[!UICONTROL 列挙値]*」セクションを開きます。ここで、許容可能な各ロイヤルティレベルの&#x200B;**[!UICONTROL 値]**（キャメルケース）と&#x200B;**[!UICONTROL ラベル]**（タイトルケースでの読みやすい名前でオプション）を入力できます。

When you have completed all field properties, click **[!UICONTROL Apply]** and the &quot;[!UICONTROL loyaltyLevel]&quot; field will be added to the &quot;loyalty&quot; object.

![](../images/tutorials/create-schema/loyalty_level_enum.png)

利用可能な追加の制約に関する詳細情報：

* **[!UICONTROL 必須]:**フィールドがデータ取り込みに必要であることを示します。 このフィールドを含まないデータセットに基づいてスキーマセットにアップロードされたデータの取得は失敗します。
* **[!UICONTROL 配列]:**フィールドに値の配列が含まれ、各値は指定されたデータ型になっていることを示します。 例えば、「String」データ型を選択し、「Array」チェックボックスをオンにすると、フィールドに文字列の配列が含まれます。
* **[!UICONTROL 列挙]:**このフィールドに、可能な値の列挙リストの値の1つを含める必要があることを示します。
* **[!UICONTROL ID]:**このフィールドがIDフィールドであることを示します。 ID フィールドの詳細については、[このチュートリアルの後半](#identity-field)で説明します。

## 複数フィールドオブジェクトのデータ型への変換 {#datatype}

After adding several loyalty-specific fields, the &quot;[!UICONTROL loyalty]&quot; object now contains a common data structure that could be useful in other schemas.

複数フィールドの構造が再利用可能で、同じデータ構造を柔軟に使用したい場合は、スキーマエディターを使用して、その構造をデータ型に変換できます。

データ型を使用すると、複数フィールド構造を一貫して使用でき、mixin よりも柔軟性が高まります。これは、データ型がスキーマ内のどこでも使用できるからです。これは、mixin のフィールドの&#x200B;**[!UICONTROL 型]**&#x200B;を、レジストリで定義されている任意のデータ型に設定することによっておこなわれます。

To convert the &quot;[!UICONTROL loyalty]&quot; object to a data type, click on the &quot;loyalty&quot; field under *[!UICONTROL Structure]* and select **[!UICONTROL Convert to New Data Type]** on the right-hand-side of the editor under *[!UICONTROL Field Properties]*. A small green pop-up appears confirming &quot;[!UICONTROL Object Converted to Data Type]&quot;.

Now, when you look under *[!UICONTROL Structure]*, you can see that the &quot;[!UICONTROL loyalty]&quot; field has a data type of &quot;[!UICONTROL Loyalty]&quot; and the fields have small lock icons beside them indicating they are no longer individual fields, but rather part of a multi-field structure.

In a future schema, you could now assign a field the **[!UICONTROL Type]** of &quot;[!UICONTROL Loyalty]&quot; and it would automatically include Loyalty Level, Points, Member Since, and Loyalty ID fields.

![](../images/tutorials/create-schema/loyalty_data_type.png)

## ID フィールドとしてのスキーマフィールドの設定 {#identity-field}

Schemas are used for ingesting data into [!DNL Experience Platform], and that data is ultimately used to identify individuals and stitch together information coming from multiple sources. To help with this process, key fields can be marked as &quot;[!UICONTROL Identity]&quot; fields.

[!DNL Experience Platform] で「 **[!UICONTROL ID]** 」チェックボックスを使用して、IDフィールドを簡単に示すことがで [!DNL Schema Editor]きます。

例えば、同じ「レベル」に属するロイヤルティプログラムのメンバーが何千人もいる場合がありますが、ロイヤルティプログラムのメンバーはそれぞれ一意の「ロイヤルティ ID」（この場合は、個々のメンバーの電子メールアドレス）を持ちます。各メンバーの一意の識別子である「loyaltyId」は ID フィールドに適した候補となりますが、「level」はなりません。

In the *[!UICONTROL Structure]* section of the editor, click on the &quot;[!UICONTROL loyaltyId]&quot; field that you created and you will see the **[!UICONTROL Identity]** checkbox appear under *[!UICONTROL Field Properties]*. チェックボックスをオンにすると、これを&#x200B;**[!UICONTROL プライマリ ID]** に設定するオプションが表示されます。そのチェックボックスもオンにします。

次に、**[!UICONTROL ID 名前空間]**&#x200B;を指定する必要があります。There are several pre-defined namespaces, but since the &quot;[!UICONTROL loyaltyId]&quot; is the member&#39;s email address, select &quot;Email&quot; from the dropdown list. You can now click **[!UICONTROL Apply]** to confirm the updates to the &quot;[!UICONTROL loyaltyId]&quot; field.

Now all data ingested into the &quot;[!UICONTROL loyaltyId]&quot; field will be used to help identify that individual and stitch together a single view of that customer.

![](../images/tutorials/create-schema/loyaltyId_primary_identity.png)

>[!NOTE]
>
> スキーマフィールドをプライマリ ID として設定した場合、後でスキーマ内の別のフィールドをプライマリとして設定しようとすると、エラーメッセージが表示されます。各スキーマには、1 つのプライマリ ID フィールドのみを含めることができます。

To learn more about working with identities, please review the [!DNL Identity Service](../../identity-service/home.md) documentation.

<!-- ## Relationship

Schemas define a static view of a concept, but do not provide specific details on how data based on these schemas (datasets, etc) may relate to one another. Adobe Experience Platform allows you to describe these relationships through the **Relationship** checkbox in the schema editor. 

In order to define a relationship, click on the field and check the **Relationship** checkbox on the right-side of the canvas. 

![](../images/tutorials/create-schema/relationship.png)

More information about relationships and other schema metadata can be found in the [Schema Registry API Developer Guide](../schema_registry_developer_guide.md). -->

## スキーマを [!DNL Real-time Customer Profile] {#profile}

The Schema Editor provides the ability to enable a schema for use with [!DNL Real-time Customer Profile](../../profile/home.md). [!DNL Profile] 顧客属性の360°の堅牢なプロファイルを構築し、顧客が統合されたあらゆるシステムにわたって行ったすべてのインタラクションのタイムスタンプのあるアカウントを作成することで、各顧客の全体的な表示を提供 [!DNL Experience Platform]します。

In order for a schema to be enabled for use with [!DNL Real-time Customer Profile], it must have a primary identity defined. 最初にプライマリ ID を定義せずにスキーマを有効にしようとすると、「プライマリ ID が見つかりません」というエラーメッセージが表示されます。

<img src="../images/tutorials/create-schema/missing_primary_identity.png" width="600" /><br>

To enable the &quot;Loyalty Members&quot; schema for use in [!DNL Profile], begin by clicking on &quot;Loyalty Members&quot; in the *Structure* section of the editor.

エディターの右側の「*スキーマプロパティ*」の下で、スキーマの表示名、説明、型などの情報が表示されます。この情報に加えて、「**[!UICONTROL プロファイル]**」というトグルボタンがあります。

![](../images/tutorials/create-schema/unified_profile_toggle.png)

Click **[!UICONTROL Profile]** and a pop-up appears, asking you to confirm that you wish to enable the schema for [!DNL Profile].

![](../images/tutorials/create-schema/enable_unified_profile.png)

>[!NOTE]
>
>Once a schema has been enabled for [!DNL Real-time Customer Profile] and saved, it cannot be disabled.

## 次の手順とその他のリソース

Now that you have finished composing a &quot;[!UICONTROL Loyalty Members]&quot; schema, you can see the complete schema in the *Structure* section of the editor. Click **[!UICONTROL Save]** and the schema will be saved to the [!DNL Schema Library], making it accessible by the [!DNL Schema Registry].

Your new schema is now able to be used to ingest data into [!DNL Platform]. データの取得にスキーマを使用した後は、追加的な変更のみがおこなわれる場合があります。スキーマバージョン管理について詳しくは、「[スキーマ合成の基本](../schema/composition.md)」を参照してください。

The &quot;[!UICONTROL Loyalty Members]&quot; schema is also available to be viewed and managed using the [!DNL Schema Registry] API. API の使用を開始するには、まず『[スキーマレジストリ API 開発者ガイド](../api/getting-started.md)』を読んでください。

>[!WARNING]
>
>次のビデオに示す [!DNL Platform] UIは古いものです。 最新のUIのスクリーンショットと機能については、上記のドキュメントを参照してください。

次のビデオでは、 [!DNL Platform] UIで単純なスキーマを作成する方法を示します。

>[!VIDEO](https://video.tv.adobe.com/v/27012?quality=12&learn=on)

次のビデオは、ミックスインとクラスを使用する際の理解を深めるためのものです。

>[!VIDEO](https://video.tv.adobe.com/v/27013?quality=12&learn=on)

## 付録

次の情報は、「スキーマエディタのチュートリアル」の補足です。

### 新しいクラスの作成 {#create-new-class}

[!DNL Experience Platform] は、組織に固有のクラスに基づいてスキーマを定義する柔軟性を提供します。

*[!UICONTROL クラスの割り当て]*&#x200B;ダイアログを開くには、スキーマエディターの「*[!UICONTROL クラス]*」セクションで、「**[!UICONTROL 割り当て]**」をクリックします。Within the dialog, select **C[!UICONTROL reate New Class ]**.

You can then give your new class a **[!UICONTROL Display Name]** (a short, descriptive, unique, and user-friendly name for the class), a **[!UICONTROL Description]**, and a **[!UICONTROL Behavior]** (&quot;[!UICONTROL Record]&quot; or &quot;[!UICONTROL Time Series]&quot;) for the data the schema will define.

![新しいクラスの詳細](../images/tutorials/create-schema/create_new_class.png)

>[!NOTE]
>
> 組織で定義されたクラスを実装するスキーマを構築する場合、mixin は互換性のあるクラスでのみ使用できることに注意してください。定義したクラスは新しいので、「*Mixin の追加*」には互換性のある mixin が表示されません。代わりに、「**[!UICONTROL 新規 mixin の作成]**」を選択して、そのクラスで使用する mixin を定義する必要があります。次に新しいクラスを実装するスキーマを作成すると、定義した mixin が一覧表示され、使用できます。

### スキーマクラスの変更 {#change-class}

最初のスキーマ合成プロセス中は、スキーマを保存する前に、いつでもスキーマの基となるクラスを変更できます。

>[!WARNING]
>
> クラスを変更する前に注意してください。Mixin は特定のクラスとのみ互換性があるので、クラスを変更するとキャンバスがリセットされ、その時点までに追加されたフィールドが削除されます。

クラスを変更するには、エディターの「*[!UICONTROL 合成]*」セクションで、「*[!UICONTROL クラス]*」の横の「**[!UICONTROL 割り当て]**」をクリックします。

「*[!UICONTROL クラスの割り当て]*」ダイアログが開いたら、使用可能なリストから新しいクラスを選択します。「**[!UICONTROL クラスの割り当て]**」をクリックすると、新しいクラスの割り当てを確認する新しいダイアログが開きます。

![クラスの変更](../images/tutorials/create-schema/assign_new_class_warning.png)

クラスの変更を確認すると、キャンバスがリセットされ、合成の進行状況がすべて失われます。