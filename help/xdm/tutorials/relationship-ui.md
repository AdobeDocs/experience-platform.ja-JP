---
keywords: Experience Platform；ホーム；人気のトピック；ui;UI;XDM;XDM システム；エクスペリエンスデータモデル；エクスペリエンスデータモデル；データモデル；スキーマエディター；スキーマエディター；スキーマ；作成；関係；参照；
solution: Experience Platform
title: スキーマエディターを使用した 2 つのスキーマ間の関係の定義
description: このドキュメントでは、Experience Platform ユーザーインターフェイスのスキーマエディターを使用して、2 つのスキーマ間の関係を定義するチュートリアルを提供します。
type: Tutorial
exl-id: feed776b-bc8d-459b-9700-e5c9520788c0
source-git-commit: 5f9fdc9eff4d8bba049c03058d24e80e9b89e953
workflow-type: tm+mt
source-wordcount: '1376'
ht-degree: 21%

---

# [!DNL Schema Editor] を使用した 2 つのスキーマ間における 1 対 1 の関係を定義 {#relationship-ui}

>[!CONTEXTUALHELP]
>id="platform_schemas_relationships"
>title="スキーマの関係"
>abstract="異なるクラスに属しているスキーマは、関係フィールドを使用して文脈的ににリンクできるので、より複雑なセグメント化ルールを作成できます。スキーマの関係について詳しくは、ドキュメントを参照してください。"

>[!CONTEXTUALHELP]
>id="platform_xdm_1to1_reference_schema"
>title="参照スキーマ"
>abstract="関係を確立するスキーマを選択します。このスキーマは、現在のスキーマとは異なるクラスである可能性があります。スキーマの関係について詳しくは、ドキュメントを参照してください。"

>[!CONTEXTUALHELP]
>id="platform_xdm_1to1_identity_namespace"
>title="参照 ID 名前空間"
>abstract="参照スキーマのプライマリ ID フィールドの名前空間 (タイプ)。参照スキーマは、関係に参加するために、確立されたプライマリ ID フィールドが必要です。スキーマの関係について詳しくは、ドキュメントを参照してください。"

様々なチャネルでの顧客とブランドとの関係を理解する能力は、Adobe Experience Platform の重要な部分です。[!DNL Experience Data Model] （XDM）スキーマの構造でこれらの関係を定義すると、顧客データに対する複雑なインサイトを得ることができます。

スキーマの関係は、結合スキーマと [!DNL Real-Time Customer Profile] を使用して推論できますが、同じクラスを共有するスキーマにのみ適用されます。 異なるクラスに属する 2 つのスキーマ間の関係を確立するには、専用の関係フィールドをソーススキーマに追加する必要があります。このスキーマは、他の関連スキーマの ID を参照します。

>[!NOTE]
>
>ソースおよび宛先スキーマの両方が同じクラスに属する場合は、専用の関係フィールドを **使用しない** でください。 この場合は、和集合スキーマ UI を使用して関係を確認します。 これを行う方法については、結合スキーマ UI ガイドの [ 関係を表示 ](../../profile/ui/union-schema.md#view-relationships) の節を参照してください。

このドキュメントでは、[!DNL Experience Platform] ユーザーインターフェイスのスキーマエディターを使用して、2 つのスキーマ間の関係を定義するチュートリアルを提供します。 API を使用してスキーマ関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。

>[!NOTE]
>
>Adobe Real-Time Customer Data Platform B2B editionで多対 1 の関係を作成する手順については、[B2B の関係の作成 ](./relationship-b2b.md) に関するガイドを参照してください。

## はじめに

このチュートリアルでは、[!DNL Experience Platform] UI の [!DNL XDM System] とスキーマエディターに関する十分な知識が必要です。 このチュートリアルを始める前に、次のドキュメントを確認してください。

* [Experience Platformの XDM システム ](../home.md):XDM と [!DNL Experience Platform] での実装の概要です。
* [スキーマ構成の基本](../schema/composition.md)：XDM スキーマの構築ブロックの紹介。
* [ を使用したスキーマの作成  [!DNL Schema Editor]](create-schema-ui.md)：ス [!DNL Schema Editor] ーマの操作の基本を説明するチュートリアル。

## ソースおよび参照スキーマの定義

この関係で定義される 2 つのスキーマが既に作成されていると想定されます。デモ目的で、このチュートリアルは、組織のロイヤルティプログラム （「[!DNL Loyalty Members]」スキーマで定義）のメンバーとお気に入りのホテル （「[!DNL Hotels]」スキーマで定義）のメンバーの関係を作成します。

>[!IMPORTANT]
>
>関係を確立するには、両方のスキーマにプライマリ ID が定義され、[!DNL Real-Time Customer Profile] が有効になっている必要があります。 スキーマを適切に設定する方法に関するガイダンスが必要な場合は、スキーマ作成チュートリアルの [ プロファイルで使用するスキーマの有効化 ](./create-schema-ui.md#profile) に関する節を参照してください。

スキーマ関係は、**参照スキーマ** 内の別のフィールドを指す **ソーススキーマ** 内の専用フィールドで表されます。 以下の手順では、「[!DNL Loyalty Members]」がソーススキーマになり、「[!DNL Hotels]」が参照スキーマとして機能します。

以下の節では、関係が定義される前に、このチュートリアルで使用する各スキーマの構造について説明します。

### [!DNL Loyalty Members] スキーマ

ソーススキーマ「[!DNL Loyalty Members]」は、[!DNL XDM Individual Profile] クラスに基づいており、これには、ロイヤルティプログラムのメンバーを記述するフィールドが含まれています。 これらのフィールドの 1 つである `personalEmail.addess` は、[!UICONTROL  メール ] 名前空間の下でスキーマのプライマリ ID として機能します。 **[!UICONTROL スキーマプロパティ]** に示すように、このスキーマは [!DNL Real-Time Customer Profile] での使用が有効になっています。

![](../images/tutorials/relationship/loyalty-members.png)

### [!DNL Hotels] スキーマ

参照スキーマ「[!DNL Hotels]」は、カスタム「[!DNL Hotels]」クラスに基づいており、ホテルを説明するフィールドを含んでいます。 関係に参加するには、参照スキーマにプライマリ ID も定義され、[!UICONTROL  プロファイル ] に対して有効になっている必要があります。 この場合、は `_tenantId.hotelId` カスタムの「[!DNL Hotel ID]」 ID 名前空間を使用して、スキーマのプライマリ ID として機能します。

![ プロファイルに対して有効にする ](../images/tutorials/relationship/hotels.png)

>[!NOTE]
>
>カスタム ID 名前空間の作成方法については、[ID サービスドキュメント ](../../identity-service/features/namespaces.md#manage-namespaces) を参照してください。

## 関係フィールドグループの作成

>[!NOTE]
>
>この手順は、ソーススキーマに、参照スキーマのプライマリ ID へのポインターとして使用される専用の文字列タイプフィールドがない場合にのみ必要です。 このフィールドがソーススキーマで既に定義されている場合は、次の[関係フィールドを定義](#relationship-field)する手順に進んでください。

2 つのスキーマ間の関係を定義するには、ソーススキーマに、参照スキーマのプライマリ ID を示す専用フィールドが必要です。 このフィールドをソーススキーマに追加するには、新しいスキーマフィールドグループを作成するか、既存のスキーマフィールドグループを拡張します。

[!DNL Loyalty Members] スキーマの場合、新しい `preferredHotel` フィールドが追加され、企業訪問に対するロイヤルティメンバーの優先ホテルを示します。 まず、ソーススキーマ名の横にあるプラスアイコン（**+**）を選択します。

![](../images/tutorials/relationship/loyalty-add-field.png)

新規フィールドプレースホルダーがキャンバスに表示されます。 「**[!UICONTROL フィールドプロパティ]**」で、フィールド名とフィールドの表示名を指定し、タイプを「[!UICONTROL  文字列 ]」に設定します。 「**[!UICONTROL 割り当て先]**」で、拡張する既存のフィールドグループを選択するか、一意の名前を入力して新しいフィールドグループを作成します。 この場合、新しい「[!DNL Preferred Hotel]」フィールドグループが作成されます。

![](../images/tutorials/relationship/relationship-field-details.png)

完了したら、「**[!UICONTROL 適用]**」を選択します。

![](../images/tutorials/relationship/relationship-field-apply.png)

更新された `preferredHotel` フィールドは、カスタムフィールドなので、`_tenantId` オブジェクトの下のキャンバスに表示されます。 「**[!UICONTROL 保存]**」を選択して、スキーマに対する変更を最終決定します。

![](../images/tutorials/relationship/relationship-field-save.png)

## ソーススキーマで関係フィールドを定義 {#relationship-field}

ソーススキーマに専用の参照フィールドが定義されたら、それを関係フィールドとして指定できます。

>[!NOTE]
>
>関係は、文字列フィールドまたは文字列配列フィールドでのみサポートできます。

キャンバスで「`preferredHotel`」フィールドを選択し、**[!UICONTROL フィールドプロパティ]** サイドバーで **[!UICONTROL 関係を追加]** を選択します。

![ フィールドプロパティのサイドバーで「関係を追加」がハイライト表示されたスキーマエディター。](../images/tutorials/relationship/add-relationship.png)

[!UICONTROL  関係を追加 ] ダイアログが表示されます。 このダイアログから、関係フィールドを設定するために必要なパラメーターを設定できます。 Real-Time CDP B2C ユーザーの場合は、ソーススキーマと参照スキーマの間に 1 対 1 の関係を **のみ** 設定できます。

>[!NOTE]
>
>Real-Time CDP B2B editionにアクセスできる場合は、キャンバスの右側のパネルのコントロールを使用して関係フィールドを定義したり、[ 同じダイアログ ](./relationship-b2b.md#relationship-field) を使用して多対 1 の関係を構築したりできます。

![ 関係を追加ダイアログ ](../images/tutorials/relationship/add-relationship-dialog.png)

**[!UICONTROL 参照スキーマ]** のドロップダウンを使用し、関係の参照スキーマを選択します（この例では「[!DNL Hotels]」）。

>[!NOTE]
>
>プライマリ ID を含むスキーマのみが参照スキーマドロップダウンメニューに含まれます。 このセーフガードは、まだ適切に設定されていないスキーマと、誤って関係を作成するのを防ぎます。

参照スキーマの ID 名前空間（この場合は「[!DNL Hotel ID]」）は、**[!UICONTROL 参照 ID 名前空間]** の下に自動的に入力されます。 終了したら「**[!UICONTROL 適用]**」を選択します。

![ 関係パラメーターが設定され、「適用」がハイライト表示された関係を追加ダイアログ ](../images/tutorials/relationship/apply-relationship.png)

キャンバスで「`preferredHotel`」フィールドが関係としてハイライト表示され、参照スキーマの名前が表示されるようになりました。 「**[!UICONTROL 保存]**」を選択して変更を保存し、ワークフローを完了します。

![ 関係参照と「保存」がハイライト表示されたスキーマエディター ](../images/tutorials/relationship/relationship-save.png)

### 既存の関係フィールドを編集 {#edit-relationship}

参照スキーマを変更するには、既存の関係を持つフィールドを選択し、**[!UICONTROL フィールドプロパティ]** サイドバーで **[!UICONTROL 関係を編集]** を選択します。

![ 「関係を編集」がハイライト表示されたスキーマエディター。](../images/tutorials/relationship/edit-relationship.png)

[!UICONTROL  関係を編集 ] ダイアログが表示されます。 ここから、[ 関係フィールドの定義 ](#relationship-field) で説明されているプロセスに従うか、関係を削除できます。 「**[!UICONTROL 関係を削除]**」を選択して、参照スキーマへの関係を削除します。

![ 関係を編集ダイアログ ](../images/tutorials/relationship/edit-relationship-dialog.png)

## 関係のフィルタリングと検索 {#filter-and-search}

[!UICONTROL  スキーマ ] ワークスペースの「[!UICONTROL  関係 ]」タブから、スキーマ内の特定の関係をフィルタリングして検索できます。 このビューを使用すると、関係をすばやく見つけて管理できます。 フィルタリングオプションの手順について詳しくは、[ スキーマリソースの調査 ](../ui/explore.md#lookup) に関するドキュメントを参照してください。

![ スキーマ ワークスペースの「関係」タブ ](../images/tutorials/relationship-b2b/relationship-tab.png)

## 次の手順

このチュートリアルでは、[!DNL Schema Editor] を使用して 2 つのスキーマ間に 1 対 1 の関係を正常に作成しました。 API を使用して関係を定義する手順については、[スキーマレジストリ API を使用した関係の定義](relationship-api.md)についてのチュートリアルを参照してください。
