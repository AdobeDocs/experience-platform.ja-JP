---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: スキーマエディターを使用してスキーマを作成する。
topic: tutorials
translation-type: tm+mt
source-git-commit: 661789fa15ea11b0e42060b1b90d74785c04fa1f
workflow-type: tm+mt
source-wordcount: '3376'
ht-degree: 0%

---


# スキーマの作成には、 [!DNL Schema Editor]

に [!DNL Schema Registry] は、Adobe Experience Platform内のすべてのリソースの表示と管理を行えるユーザーインターフェイスおよびRESTful APIが用意されて [!DNL Schema Library]います。 には、アドビ、Experience Platformパートナー、および使用するベンダーが提供するリソースと、独自に定義してに保存したリソースが [!DNL Schema Library] 含まれ [!DNL Schema Registry]ます。

このチュートリアルでは、のスキーマエディタを使用してスキーマを作成する手順を説明 [!DNL Experience Platform]します。 スキーマレジストリAPIを使用してスキーマを作成する場合は、APIを使用したスキーマの [作成を試みる前に、『](../api/getting-started.md) スキーマレジストリ開発者ガイド [』を読んでください](create-schema-api.md)。

このチュートリアルには、スキーマを構成する際に使用できる新しいクラス [を](#create-new-class) 定義する手順も含まれています。

## はじめに

このチュートリアルでは、スキーマエディタの使用に関わるAdobe Experience Platformの様々な側面について、十分な理解が必要です。 このチュートリアルを開始する前に、次の概念に関するドキュメントを確認してください。

* [!DNL Experience Data Model (XDM)](../home.md): Platformが顧客体験データを編成する際に使用する標準化されたフレームワーク。
* [スキーマ構成の基本](../schema/composition.md): XDMスキーマとその構築ブロックの概要です。クラス、ミックスイン、データ型、フィールドなどが含まれます。
* [!DNL Real-time Customer Profile](../../profile/home.md): 複数のソースからの集計データに基づいて、統合されたリアルタイムの消費者プロファイルを提供します。

このチュートリアルでは、にアクセスする必要があり [!DNL Experience Platform]ます。 でIMS組織にアクセスできない場合は、先に進む前にシステム管理者にお問い合わせ [!DNL Experience Platform]ください。

## スキーマワークスペースでの既存のスキーマの参照 {#browse}

内のスキーマワークスペース [!DNL Experience Platform] は、のビジュアライゼーションを提供し [!DNL Schema Library]ます。これにより、利用可能なすべてのスキーマの表示と管理を行え、新しいを作成できます。 ワークスペースにはスキーマエディタも含まれています。このキャンバスには、このチュートリアル全体を通してスキーマを構成するキャンバスがあります。

ログイン後 [!DNL Experience Platform]、左側のナビゲーションで「 **[!UICONTROL スキーマ]** 」をクリックすると、スキーマワークスペースに移動します。 スキーマのリスト(の説明 [!DNL Schema Library])が表示されます。ここで、使用可能なすべてのスキーマの表示、管理およびカスタマイズを行うことができます。 リストには、スキーマの基となる名前、タイプ、クラス、動作（レコードまたは時系列）、およびスキーマが最後に変更された日時が含まれます。

検索バーの横にあるフィルターアイコンをクリックして、クラス、ミックスイン、データ型など、レジストリ内のすべてのリソースにフィルター機能を使用します。

![スキーマライブラリの表示](../images/tutorials/create-schema/schemas_filter.png)

## スキーマの作成と命名 {#create}

スキーマの構成を開始するには、スキーマワークスペースの右上隅にある「 **[!UICONTROL スキーマを作成]** 」をクリックします。

![スキーマを作成ボタン](../images/tutorials/create-schema/create_schema_button.png)

*スキーマエディタ* (Editor)が表示されます。 これは、スキーマを作成するキャンバスです。 エディターに到達すると、カンバスの「 *構造* 」セクションに「無題のスキーマ」が自動的に作成され、カスタマイズを開始できます。

![スキーマエディタ](../images/tutorials/create-schema/schema_editor.png)

エディタの右側には、 *スキーマプロパティ* (「 **[!UICONTROL 表示名]** 」フィールドを使用してスキーマの名前を指定できます)があります。 名前を入力すると、キャンバスが更新され、スキーマの新しい名前が反映されます。

![スキーマキャンバス](../images/tutorials/create-schema/name_schema.png)

スキーマの名前を決定する際に、いくつかの重要な考慮事項を考慮する必要があります。

* スキーマ名は短く、説明的にし、後でスキーマを簡単に見つけられるようにします。
* スキーマ名は一意にする必要があります。つまり、将来再利用できなくなるように十分に特定する必要があります。 例えば、組織で異なるブランドに対して別々の忠誠度プログラムを持つ場合は、後で定義する他の忠誠度関連のスキーマと区別しやすいように、スキーマに「忠誠度メンバー」という名前を付けると効果的です。
* 必要に応じて、「 **[!UICONTROL 説明]** 」フィールドを使用して、スキーマに関する追加情報を入力できます。

このチュートリアルでは、忠誠度プログラムのメンバに関連するデータを取り込むスキーマを構成しているので、スキーマの名前は「Loyality Members」になります。

## クラスの割り当て {#class}

エディターの左側には、「 *コンポジション* 」セクションがあります。 現在、2つのサブセクションが含まれています。 *[!UICONTROL スキーマ]* と *[!UICONTROL クラス]*。

スキーマに名前が付けられたら、スキーマが実装するクラスを割り当てる時間です。 「 **[!UICONTROL クラス」の横の「]** 割り当て *[!UICONTROL 」をクリックし]*&#x200B;ます。

![](../images/tutorials/create-schema/assign_class_button.png)

[ *[!UICONTROL クラスを割り当て]* ]ダイアログが表示されます。 このウィンドウには、組織で定義された任意のクラス（「お客様」として定義された所有者）と、アドビが定義した標準クラスを含む、使用可能なすべてのクラスのリストが表示されます。

クラス名をクリックして、クラスの説明を表示します。 「 **[!UICONTROL プレビュークラス構造]** 」を選択して、クラスに関連付けられたフィールドとメタデータを表示することもできます。

このチュートリアルでは、この [!DNL XDM Individual Profile] クラスを使用します。 クラスの横にあるラジオボタンをクリックして選択し、「 **[!UICONTROL Assign Class]**」をクリックします。

![[クラスを割り当て]ダイアログ](../images/tutorials/create-schema/assign_class.png)

キャンバスが再び表示されます。 「 *[!UICONTROL Class]* 」セクションには、選択したクラス([!DNL XDM Individual Profile])が含まれ、クラスによって提供されるフィールドが「 [!DNL XDM Individual Profile] Structure ** 」セクション内に表示されます。

![XDM個別プロファイルクラスが割り当てられました](../images/tutorials/create-schema/class_assigned_structure.png)

フィールドは、「fieldName」の形式で表示されます |データタイプ&quot;. UIでスキーマフィールドを定義する手順は、このチュートリアルで後述します。

>[!NOTE]
>
>スキーマのクラスは、スキーマが保存される前の最初の構成プロセスのどの時点でも [変更できますが](#change-class) 、これは非常に注意して行う必要があります。 ミックスインは特定のクラスとのみ互換性があるので、クラスを変更するとキャンバスと追加したフィールドがリセットされます。

## ミッ追加クスイン {#mixin}

クラスが割り当てられたので、 *Composition* セクションに3つ目のサブセクションが含まれます。 *[!UICONTROL ミックスイン]*。

これで、ミックスインを追加して、スキーマへのフィールドの追加を開始できます。 ミックスインは、特定の概念を説明する1つ以上のフィールドのグループです。 このチュートリアルでは、ミックスインを使用して忠誠度プログラムのメンバーの説明を行い、名前、誕生日、電話番号、住所などの主要情報を取り込みます。

Mixinを追加するには、「Mixins ****** 」サブセクションでをクリックします。

![](../images/tutorials/create-schema/add_mixin_button.png)

The *[!UICONTROL Add Mixin]* dialog appears. Mixins are only intended for use with specific classes, therefore the list of mixins shows only those compatible with the class you selected (in this case, the [!DNL XDM Individual Profile] class).

Mixinの横にあるラジオボタンを選択すると、Mixinの構造を **[!UICONTROL プレビューするオプションが表示されます]**。 Select the &quot;Profile Person Details&quot; mixin, then click **[!UICONTROL Add Mixin]**.

![](../images/tutorials/create-schema/add_mixin_person_details.png)

スキーマキャンバスが再び表示されます。 「 *[!UICONTROL Mixins]* 」セクションには「[!UICONTROL プロファイルの人物の詳細]」ミックスインがリストされ、「 *[!UICONTROL 構造]* 」セクションにはMixinが提供するフィールドが含まれるようになりました。

![](../images/tutorials/create-schema/person_details_structure.png)

このMixinは、最上位レベルの名前「person」の下に、データタイプ「Person」のフィールドを表示します。 このフィールドグループは、名前、生年月日、性別など、個人に関する情報を説明します。

>[!NOTE]
>
>Remember that fields may use scalar types (such as string, integer, array, or date) as their data type, as well as any &quot;data type&quot; (a group of fields representing a common concept) in the [!DNL Schema Registry].

Notice that the &quot;[!UICONTROL name]&quot; field has a data type of &quot;[!UICONTROL Person Name]&quot;, meaning it too describes a common concept and contains name-related sub-fields such as first name, last name, and full name.

キャンバス内の別のフィールドをクリックして、そのフィールドがスキーマ構造に貢献する追加のフィールドを表示します。

## Add another mixin {#mixin-2}

You can now repeat the same steps to add another mixin. When you view the *[!UICONTROL Add Mixin]* dialog this time, notice that the &quot;[!UICONTROL Profile Person Details]&quot; mixin has been greyed out and the radio button next to it cannot be selected. This prevents you from accidentally duplicating mixins that you have already included in the current schema.

You can now add the &quot;[!DNL Profile Personal Details" mixin] from the *[!UICONTROL Add Mixin]* dialog.

![](../images/tutorials/create-schema/add_mixin_personal_details.png)

Once added, the canvas reappears. 「[!UICONTROL プロファイルの個人詳細]」は、「 *[!UICONTROL 構成]* 」セクションの「ミックスイン」の下に表示され、自宅住所、携帯電話、さらにStructure *[!UICONTROL Personal Detailsの下に追加されてい]***&#x200B;ます。

Similar to the &quot;[!UICONTROL name]&quot; field, the fields you just added represent multi-field concepts. For example, &quot;[!UICONTROL homeAddress]&quot; has a data type of &quot;[!UICONTROL Address]&quot; and &quot;[!UICONTROL mobilePhone]&quot; has a data type of &quot;[!UICONTROL Phone Number]&quot;. You can click on each of these fields to expand them and see the additional fields included in the data type.

![](../images/tutorials/create-schema/personal_details_structure.png)

## 新しいミックスインの定義 {#define-mixin}

The &quot;[!UICONTROL Loyalty Members]&quot; schema is meant to capture data related to the members of a loyalty program, so it will require some specific loyalty-related fields. 必要なフィールドを含む標準ミックスインが存在しないので、新しいミックスインを定義する必要があります。

This time, when you open the *[!UICONTROL Add Mixin]* dialog, select **[!UICONTROL Create New Mixin]**. You will then be asked to provide a **[!UICONTROL Display Name]** and **[!UICONTROL Description]** for your mixin.

![](../images/tutorials/create-schema/mixin_create_new.png)

As with class names, the mixin name should be short and simple, describing what the mixin will contribute to the schema. These too are unique, so you will not be able to reuse the name and must therefore ensure it is specific enough.

For this tutorial, name the new mixin &quot;[!UICONTROL Loyalty Details]&quot;.

Click **[!UICONTROL Add Mixin]** to return to the schema editor. 「[!UICONTROL 忠誠度の詳細]」は、キャンバスの左側の *[!UICONTROL ミックスインの下に表示されるはずですが、関連するフィールドはまだないので、]* 構造の下に新しいフィールドは表示されません **。

## Add fields to the mixin {#mixin-fields}

「[!UICONTROL 忠誠度の詳細]」ミックスインを作成したので、ここでミックスインがスキーマに貢献するフィールドを定義します。

最初に、 *[!UICONTROL Mixins]* セクションのMixin名をクリックします。 この操作を行うと、エディタの右側に *[!UICONTROL Mixinプロパティ]* が表示され、 **[!UICONTROL 構造]** の下のスキーマ名の横にフィールド **&#x200B;ボタンが表示されます。

![](../images/tutorials/create-schema/loyalty_details_structure.png)

「 **[!UICONTROL Loyality Members]**[!UICONTROL 」の横の「]Field」をクリックして、構造内に新しいノードを作成します。 このノード（この例では「_tenantId」と呼ばれます）は、IMS組織のテナントIDを表し、前にアンダースコアが付きます。 The presence of the tenant ID indicates that the fields you are adding are contained in your organization&#39;s namespace.

つまり、追加するフィールドは組織に固有で、IMS組織にのみアクセス可能な特定の領域 [!DNL Schema Registry] に保存されます。 定義するフィールドは、他の標準クラス、ミックスイン、データ型、フィールドの名前との競合を防ぐために、常に名前空間に追加する必要があります。

その名前空間ノード内には「[!UICONTROL 新しいフィールド]」があります。 これは、「[!UICONTROL 忠誠度の詳細]」ミックスインの開始です。

![](../images/tutorials/create-schema/new_field_loyalty.png)

エディターの右側の *[!UICONTROL フィールドプロパティ]* (Field Properties[!UICONTROL )を使用して、忠誠度関連のフィールドの保持に使用するタイプが「]Object[!UICONTROL 」の「忠誠度]」フィールドを作成し、開始します。 終了したら、「 **[!UICONTROL 適用]**」をクリックします。

![](../images/tutorials/create-schema/loyalty_object.png)

変更が適用され、新しく作成された「[!UICONTROL 忠誠度]」オブジェクトが表示されます。 オブジェクトの **[!UICONTROL 横にある「]** 追加フィールド」をクリックして、忠誠度関連のフィールドを追加します。 「新しいフィールド」が表示され、キャンバスの右側に *[!UICONTROL 「フィールドプロパティ]* 」セクションが表示されます。

![](../images/tutorials/create-schema/new_field_in_loyalty_object.png)

各フィールドには、次の情報が必要です。

* **[!UICONTROL フィールド名]:**フィールドの名前。キャメルケースで書かれます。 例： loyaltyLevel
* **[!UICONTROL 表示名]:**フィールドの名前。タイトルの場合は大文字で記述されます。 例： 忠誠度レベル
* **[!UICONTROL タイプ]:**フィールドのデータ型です。 これには、基本的なスカラー型と、で定義されている任意のデータ型が含まれ[!DNL Schema Registry]ます。 例： 文字列、整数、ブール値、人、住所、電話番号など
* **[!UICONTROL 説明]:**フィールドのオプションの説明を文頭の場合と同様に記述します。 （最大200文字）

Loyaltyオブジェクトの最初のフィールドは、「[!UICONTROL loyaltyId]」という文字列になります。 新しいフィールドのタイプを「[!UICONTROL String」に設定すると、「]Field Properties *[!UICONTROL 」ウィンドウに、制約を適用するためのオプションが表示されます。]* デフォルト値のFormat、Format、MaximumのFormat、Default値のFormat、Maximumの各 ************ Formatなどがあります。

![](../images/tutorials/create-schema/string_constraints.png)

選択したデータタイプに応じて、様々な制約オプションを使用できます。 「[!UICONTROL loyaltyId]」は電子メールアドレスになるので、「[!UICONTROL 形式]」ドロップダウンメニューから「 **[!UICONTROL email]** 」を選択します。 「 **[!UICONTROL Apply]** 」を選択して変更を適用します。

![](../images/tutorials/create-schema/loyaltyId_field.png)

## Mixinの追加その他のフィールド {#mixin-fields-2}

「[!UICONTROL loyaltyId]」フィールドを追加したら、次のような忠誠度関連の情報を取り込むためのフィールドを追加できます。

* ポイント（整数）
* 次のメンバー（日付）

各フィールドを追加するには、忠誠度オブジェクトの「 **[!UICONTROL 追加Field]** 」をクリックし、必要な情報を入力します。

完了すると、Loyaltyオブジェクトに次のフィールドが含まれます。 忠誠度ID、ポイントおよびメンバー登録。

![](../images/tutorials/create-schema/loyalty_object_fields.png)

## ミックスインする追加&#39;enum&#39;フィールド {#enum}

スキーマエディタでフィールドを定義する場合、基本的なフィールドの種類に適用できる追加のオプションがいくつかあり、フィールドに含めることのできるデータに対してさらなる制約を設けることができます。

例えば、「[!UICONTROL 忠誠度レベル]」フィールドの値は、4つのオプションのいずれかにするしかありません。 このフィールドをスキーマに追加するには、「 **[!UICONTROL Loyalty]**[!UICONTROL 」オブジェクトの横にある「]追加Field *[!UICONTROL 」をクリックし、「]* Field Properties」の下の必須フィールドに入力します。

「 **[!UICONTROL Type]**」で「String」を選択すると、「 **[!UICONTROL Array]**」、「 **[!UICONTROL Enum]**」、「 **** Identity」の各チェックボックスが追加表示されます。

[ **[!UICONTROL 列挙]** ]チェックボックスをオンにして、下の[ *[!UICONTROL 列挙値]* ]セクションを開きます。 ここでは、許容可能な各忠誠度レベルの **[!UICONTROL 値]** （camelCase単位）と **[!UICONTROL ラベル]** （タイトルケース単位のオプションで読みやすい名前）を入力できます。

すべてのフィールドプロパティを完了したら、「 **[!UICONTROL 適用]** 」をクリックします。「[!UICONTROL 忠誠度]」フィールドが「忠誠度」オブジェクトに追加されます。

![](../images/tutorials/create-schema/loyalty_level_enum.png)

使用可能な追加制約の詳細：

* **[!UICONTROL 必須]:**フィールドがデータ取り込みに必要であることを示します。 このスキーマに基づいてデータセットにアップロードされた、このフィールドを含まないデータは、取り込み時に失敗します。
* **[!UICONTROL 配列]:**フィールドに値の配列が含まれ、各値は指定されたデータ型になっていることを示します。 例えば、「String」というデータ型を選択し、「Array」チェックボックスをオンにすると、フィールドに文字列の配列が含まれます。
* **[!UICONTROL 列挙]:**このフィールドに、可能な値の列挙リストの値の1つを含める必要があることを示します。
* **[!UICONTROL ID]:**このフィールドがIDフィールドであることを示します。 IDフィールドの詳細については、このチュートリアル[で後述します](#identity-field)。

## 複数フィールドオブジェクトのデータ型への変換 {#datatype}

複数の忠誠度固有のフィールドを追加した後、「[!UICONTROL 忠誠度]」オブジェクトに、他のスキーマで役立つ共通のデータ構造が含まれるようになりました。

複数フィールド構造が再利用可能な場合があり、同じデータ構造を他の場所で柔軟に使用できるようにしたい場合、スキーマエディターを使用すると、その構造をデータ型に変換できます。

データ型を使用すると、複数フィールド構造を一貫して使用でき、ミックスインよりも柔軟に使用できます。これは、データ型がスキーマ内のどこでも使用できるからです。 これは、mixin内のフィールドの **[!UICONTROL 種類]** (Type)を、レジストリで定義されている任意のデータ型に設定することで行います。

「[!UICONTROL 忠誠度]」オブジェクトをデータ型に変換するには、「 *[!UICONTROL 構造」の下の「忠誠度」フィールドをクリックし]* 、エディタの右側の「新しいデータ型に変換 **[!UICONTROL 」を選択します。「フィールドプロパティ」は、「]****」です。 「データタイプに変換された[!UICONTROL オブジェクト]」を確認する緑色の小さなポップアップが表示されます。

「 *[!UICONTROL 構造]*」の下を見ると、「[!UICONTROL 忠誠度]」フィールドのデータタイプが「[!UICONTROL 忠誠度]」で、フィールドの横に小さな錠前のアイコンが表示され、個々のフィールドではなく、複数フィールド構造の一部であることがわかります。

今後のスキーマでは、フィールドに「 **[!UICONTROL 忠誠度]** 」のタイプを割り当て、「忠誠度レベル」、「ポイント」、「メンバー次期」、「忠誠度ID」の各フィールドを自動的に含めることができます。

![](../images/tutorials/create-schema/loyalty_data_type.png)

## スキーマフィールドをIDフィールドとして設定する {#identity-field}

スキーマはデータをに取り込むために使用され [!DNL Experience Platform]、最終的にそのデータは個人を識別し、複数のソースからの情報を結合するために使用されます。 この処理に役立つように、主要フィールドを「[!UICONTROL ID]」フィールドとしてマークすることができます。

[!DNL Experience Platform] で「 **[!UICONTROL ID]** 」チェックボックスを使用して、IDフィールドを簡単に示すことがで [!DNL Schema Editor]きます。

例えば、同じ「レベル」に属する忠誠度プログラムの何千ものメンバーが存在する場合がありますが、忠誠度プログラムの各メンバーには一意の「loyaltyId」（この場合は、個々のメンバーの電子メールアドレス）があります。 「loyaltyId」が各メンバーの一意の識別子であることは、IDフィールドに適した候補となりますが、「level」は異なります。

エディタの「 *[!UICONTROL 構造]* 」セクションで、作成した「[!UICONTROL 忠誠度ID]」フィールドをクリックすると、「フィールドのプロパティ」の下に「 **[!UICONTROL ID]** 」チェックボックスが表示され **&#x200B;ます。 チェックボックスをオンにすると、これを **[!UICONTROL プライマリIDに設定するオプションが表示されます]**。 そのチェックボックスもオンにします。

次に、 **[!UICONTROL ID名前空間を指定する必要があります]**。 定義済みの名前空間はいくつかありますが、「[!UICONTROL loyaltyId]」はメンバーの電子メールアドレスなので、ドロップダウンリストから「Email」を選択します。 「 **[!UICONTROL Apply]** 」をクリックして、「[!UICONTROL loyaltyId]」フィールドの更新を確認できるようになりました。

これで、「[!UICONTROL loyaltyId]」フィールドに取り込まれるすべてのデータは、その個人を識別し、その顧客の1つの表示をつなぎ合わせるのに使用されます。

![](../images/tutorials/create-schema/loyaltyId_primary_identity.png)

>[!NOTE]
>
>スキーマフィールドをプライマリIDとして設定すると、後でスキーマ内の別のフィールドをプライマリIDとして設定しようとすると、エラーメッセージが表示されます。 各スキーマには、1つのプライマリIDフィールドのみを含めることができます。

IDの使用方法の詳細については、ドキュメントを参照してくだ [!DNL Identity Service](../../identity-service/home.md) さい。

<!-- ## Relationship

Schemas define a static view of a concept, but do not provide specific details on how data based on these schemas (datasets, etc) may relate to one another. Adobe Experience Platform allows you to describe these relationships through the **Relationship** checkbox in the schema editor. 

In order to define a relationship, click on the field and check the **Relationship** checkbox on the right-side of the canvas. 

![](../images/tutorials/create-schema/relationship.png)

More information about relationships and other schema metadata can be found in the [Schema Registry API Developer Guide](../schema_registry_developer_guide.md). -->

## スキーマを [!DNL Real-time Customer Profile] {#profile}

スキーマエディターでは、で使用するスキーマを有効にする機能が提供され [!DNL Real-time Customer Profile](../../profile/home.md)ます。 [!DNL Profile] 顧客属性の360°の堅牢なプロファイルを構築し、顧客が統合されたあらゆるシステムにわたって行ったすべてのインタラクションのタイムスタンプのあるアカウントを作成することで、各顧客の全体的な表示を提供 [!DNL Experience Platform]します。

スキーマをで使用できるようにするには、プライマリIDが定義されている必要があ [!DNL Real-time Customer Profile]ります。 最初にプライマリIDを定義せずにプライマリを有効にしようとすると、「スキーマIDが見つかりません」というエラーメッセージが表示されます。

<img src="../images/tutorials/create-schema/missing_primary_identity.png" width="600" /><br>

での「忠誠度メンバー」スキーマの使用を有効にするに [!DNL Profile]は、まずエディターの「 *構造* 」セクションで「忠誠度メンバー」をクリックします。

エディタの右側の[ *スキーマプロパティ*]には、スキーマに関する情報（表示名、説明、タイプなど）が表示されます。 この情報に加えて、 **[!UICONTROL プロファイルというトグルボタンがあります]**。

![](../images/tutorials/create-schema/unified_profile_toggle.png)

「 **[!UICONTROL プロファイル]** 」をクリックすると、スキーマを有効にするかどうかを確認するポップアップが表示され [!DNL Profile]ます。

![](../images/tutorials/create-schema/enable_unified_profile.png)

>[!NOTE]
>
>スキーマを有効にして保存すると、無効にする [!DNL Real-time Customer Profile] ことはできません。

## 次の手順とその他のリソース

「[!UICONTROL 忠誠度メンバー]」スキーマの構成が完了すると、エディターの「 *構造* 」セクションに完全なスキーマが表示されます。 「 **[!UICONTROL 保存]** 」をクリックすると、スキーマがに保存され [!DNL Schema Library]、がアクセスできるようになり [!DNL Schema Registry]ます。

これで、新しいスキーマを使用してにデータを取り込むことができ [!DNL Platform]ます。 スキーマを使用してデータを取り込むと、追加的な変更のみが行われる場合があることに注意してください。 スキーマのバージョン管理について詳しくは、スキーマ構成の [基本](../schema/composition.md) （英語）を参照してください。

「[!UICONTROL 忠誠度メンバー]」スキーマは、 [!DNL Schema Registry] APIを使用して表示および管理することもできます。 APIの使用を開始するには、 [スキーマレジストリAPI開発ガイドを読んで開始します](../api/getting-started.md)。

>[!WARNING]
>
>次のビデオに示す [!DNL Platform] UIは古いものです。 最新のUIのスクリーンショットと機能については、上記のドキュメントを参照してください。

次のビデオでは、 [!DNL Platform] UIで単純なスキーマを作成する方法を示します。

>[!VIDEO](https://video.tv.adobe.com/v/27012?quality=12&learn=on)

次のビデオは、ミックスインとクラスを使用する際の理解を深めるためのものです。

>[!VIDEO](https://video.tv.adobe.com/v/27013?quality=12&learn=on)

## 付録

次の情報は、「スキーマエディタのチュートリアル」の補足情報です。

### Create a new class {#create-new-class}

[!DNL Experience Platform] 組織に固有のクラスに基づいてスキーマを定義できる柔軟性を提供します。

スキーマエディタの「 *[!UICONTROL Class]***** 」セクションで「Assign *[!UICONTROL 」をクリックして、「Assign Class]* 」ダイアログを開きます。 ダイアログで、「 **[!UICONTROL 新しいクラスを作成&#x200B;]**」を選択します。

次に、新しいクラスに、 **[!UICONTROL スキーマを定義するデータの]** DisplayName **[!UICONTROL （短い、一意で、ユーザにわかりやすい名前）、Description(]**&#x200B;説明 **[!UICONTROL )]** 、[!UICONTROL Behavior](&quot;RecordTime Series[!UICONTROL ”)、Destaing(&quot;]RecordTime Series”)を付けます。

![新しいクラスの詳細](../images/tutorials/create-schema/create_new_class.png)

>[!NOTE]
>
>組織で定義されたクラスを実装するスキーマを構築する場合、ミックスインは互換性のあるクラスでのみ使用できることに注意してください。 定義したクラスは新しいので、 *Mixin* ダイアログには互換性のあるミックスインが一覧表示されません。 代わりに、「 **[!UICONTROL 新しいMixinを作成」を選択し]** 、そのクラスで使用するMixinを定義する必要があります。 次に新しいクラスを実装するスキーマを作成すると、定義したミックスインが一覧表示され、使用できます。

### スキーマのクラスの変更 {#change-class}

スキーマの初期構成プロセス中、いつでも、スキーマを保存する前に、スキーマの基となるクラスを変更できます。

>[!WARNING]
>
>授業を変える前に注意してください。 ミックスインは特定のクラスとのみ互換性があるので、クラスを変更するとキャンバスがリセットされ、その時点に追加したフィールドはすべて削除されます。

クラスを変更するには、エディタの「 **[!UICONTROL 組版]** 」セクションで「 *[!UICONTROL クラス]*** 」の横にある「割り当て」をクリックします。

[ *[!UICONTROL クラスを割り当て]* ]ダイアログが開いたら、使用可能なリストから新しいクラスを選択できます。 「 **[!UICONTROL クラスを割り当て]** 」をクリックすると、新しいクラスを割り当てるかどうかを確認する新しいダイアログが開きます。

![クラスの変更](../images/tutorials/create-schema/assign_new_class_warning.png)

クラスの変更を確認すると、キャンバスがリセットされ、構成の進行状況はすべて失われます。