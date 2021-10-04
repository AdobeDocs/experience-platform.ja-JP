---
keywords: Experience Platform；ホーム；人気のあるトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；フィールド；
solution: Experience Platform
title: UI での XDM フィールドの定義
description: ユーザーインターフェイスで XDM フィールドを定義するExperience Platformについて説明します。
topic-legacy: user guide
exl-id: 2adb03d4-581b-420e-81f8-e251cf3d9fb9
source-git-commit: 39d04cf482e862569277211d465bb2060a49224a
workflow-type: tm+mt
source-wordcount: '1331'
ht-degree: 5%

---

# UI での XDM フィールドの定義

Adobe Experience Platformユーザーインターフェイスの [!DNL Schema Editor] を使用すると、カスタムエクスペリエンスデータモデル (XDM) クラスとスキーマフィールドグループ内に独自のフィールドを定義できます。 このガイドでは、UI で XDM フィールドを定義する手順と、各フィールドタイプで使用可能な設定オプションについて説明します。

## 前提条件

このガイドでは、XDM システムに関する十分な知識が必要です。 Experience Platformエコシステム内での XDM の役割の概要については、[XDM の概要 ](../../home.md) を参照してください。また、XDM スキーマにクラスとフィールドグループがフィールドに与える影響については、[ 基本的なスキーマ構成 ](../../schema/composition.md) を参照してください。

このガイドは必須ではありませんが、 [!DNL Schema Editor] の様々な機能に慣れるために、 [UI でのスキーマの構成 ](../../tutorials/create-schema-ui.md) に関するチュートリアルに従うことをお勧めします。

## フィールドを追加するリソースの選択 {#select-resource}

UI で新しい XDM フィールドを定義するには、まず [!DNL Schema Editor] 内でスキーマを開く必要があります。 [!DNL Schema Library] で現在使用可能なスキーマに応じて、[ 新しいスキーマ ](../resources/schemas.md#create) を作成するか、[ 編集する既存のスキーマを選択します。](../resources/schemas.md#edit)

[!DNL Schema Editor] を開いたら、左パネルを使用して、フィールドを定義するクラスまたはフィールドグループを選択します。 リソースが組織で定義されたカスタムリソースの場合、フィールドを追加または編集するためのコントロールがキャンバスに表示されます。 これらのコントロールは、スキーマの名前の横に表示され、選択したクラスまたはフィールドグループの下で定義されたオブジェクトタイプのフィールドも表示されます。

![](../../images/ui/fields/overview/select-resource.png)

>[!NOTE]
>
>選択したクラスまたはフィールドグループが、Adobeが提供するコアリソースである場合、編集できないので、上記のコントロールは表示されません。 フィールドの追加先のスキーマがコア XDM クラスに基づいており、カスタムフィールドグループが含まれていない場合は、代わりに [ 新しいフィールドグループ ](../resources/field-groups.md#create) を作成して、スキーマに追加できます。

リソースに新しいフィールドを追加するには、キャンバスでスキーマ名の横にある「**プラス (+)**」アイコンを選択するか、フィールドを定義するオブジェクトタイプフィールドの横にある「」を選択します。

![](../../images/ui/fields/overview/plus-icon.png)

## リソースのフィールドの定義 {#define}

**プラス (+)** アイコンを選択すると、キャンバスに **[!UICONTROL 新しいフィールド]** が表示されます。このフィールドは、一意のテナント ID に名前空間が割り当てられたルートレベルのオブジェクト内にあります（下の例では `_tenantId`）。 Adobeが指定したクラスやフィールドグループの他のフィールドと競合しないように、カスタムクラスやフィールドグループを使用してスキーマに追加されたすべてのフィールドは、自動的にこの名前空間内に配置されます。

![](../../images/ui/fields/overview/new-field.png)

**[!UICONTROL フィールドのプロパティ]** の下の右側のレールで、新しいフィールドの詳細を設定できます。 各フィールドには、次の情報が必要です。

| Field プロパティ | 説明 |
| --- | --- |
| [!UICONTROL フィールド名] | フィールドを説明する一意の名前。 スキーマを保存した後は、フィールドの名前を変更できません。<br><br>名前は camelCase で書くのが理想的です。英数字、ダッシュ、アンダースコアの各文字を使用できますが、先頭にアンダースコアを付けることはできません。****<ul><li>**正しい**:  `fieldName`</li><li>**許容可能：** `field_name2`,  `Field-Name`,  `field-name_3`</li><li>**誤った**:  `_fieldName`</li></ul> |
| [!UICONTROL 表示名] | フィールドのわかりやすい名前。 |
| [!UICONTROL タイプ] | フィールドに格納するデータのタイプ。 このドロップダウンメニューから、XDM でサポートされている [ 標準のスカラー型 ](../../schema/field-constraints.md) の 1 つ、または [!DNL Schema Registry] で以前に定義した複数フィールドの [ データ型 ](../resources/data-types.md) の 1 つを選択できます。<br><br>「詳細タイプの検索」 **[!UICONTROL を選択して、既存]** のデータタイプを検索およびフィルタリングし、目的のタイプを見つけやすくすることもできます。 |

{style=&quot;table-layout:auto&quot;}

また、人間が読み取り可能なオプションの **[!UICONTROL 説明]** をフィールドに指定して、フィールドの意図した使用例に関するより詳細なコンテキストを提供することもできます。

>[!NOTE]
>
>フィールドに選択した **[!UICONTROL タイプ]** に応じて、追加の設定コントロールが右側のレールに表示される場合があります。 これらのコントロールの詳細については、[ タイプ固有のフィールドプロパティ ](#type-specific-properties) の節を参照してください。
>
>右側のレールには、特別なフィールドタイプを指定するためのチェックボックスも表示されます。 詳しくは、[ 特別なフィールドタイプ ](#special) の節を参照してください。

フィールドの設定が完了したら、「**[!UICONTROL 適用]**」を選択します。

![](../../images/ui/fields/overview/field-details.png)

キャンバスが更新され、フィールドの名前とタイプが表示され、右側のレールにフィールドのパスが他のプロパティと共に表示されます。

![](../../images/ui/fields/overview/field-added.png)

上記の手順に従って、引き続きスキーマにフィールドを追加できます。 スキーマを保存すると、基本クラスとフィールドグループに変更が加えられた場合は、そのグループも保存されます。

>[!NOTE]
>
>1 つのスキーマのフィールドグループまたはクラスに対して行った変更は、それらを使用する他のすべてのスキーマに反映されます。

## タイプ固有のフィールドプロパティ {#type-specific-properties}

新しいフィールドを定義する際に、フィールドに選択した **[!UICONTROL タイプ]** に応じて、追加の設定オプションが右側のレールに表示される場合があります。 次の表に、これらの追加のフィールドプロパティと、互換性のあるタイプの概要を示します。

| Field プロパティ | 互換性のある型 | 説明 |
| --- | --- | --- |
| [!UICONTROL デフォルト値] | [!UICONTROL String]、 [!UICONTROL Double]、 [!UICONTROL Long]、 [!UICONTROL Integer]、 [!UICONTROL Short]、 [!UICONTROL Byte]、 [!UICONTROL Boolean] | 取り込み中に他の値が指定されない場合にこのフィールドに割り当てられるデフォルト値。 この値は、フィールドの選択されたタイプに準拠している必要があります。 |
| [!UICONTROL パターン] | [!UICONTROL 文字列] | 取得時に受け入れられるように、このフィールドの値が準拠している必要がある [ 正規表現 ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)。 |
| [!UICONTROL 形式] | [!UICONTROL 文字列] | 値が準拠する必要がある文字列に対して、事前に定義された形式のリストから選択します。 使用できる形式は次のとおりです。 <ul><li>[[!UICONTROL date-time]](https://tools.ietf.org/html/rfc3339)</li><li>[[!UICONTROL 電子メール]](https://tools.ietf.org/html/rfc2822)</li><li>[[!UICONTROL hostname]](https://tools.ietf.org/html/rfc1123#page-13)</li><li>[[!UICONTROL ipv4]](https://tools.ietf.org/html/rfc791)</li><li>[[!UICONTROL ipv6]](https://tools.ietf.org/html/rfc2460)</li><li>[[!UICONTROL uri]](https://tools.ietf.org/html/rfc3986)</li><li>[[!UICONTROL uri-reference]](https://tools.ietf.org/html/rfc3986#section-4.1)</li><li>[[!UICONTROL url-template]](https://tools.ietf.org/html/rfc6570)</li><li>[[!UICONTROL json-pointer]](https://tools.ietf.org/html/rfc6901)</li></ul> |
| [!UICONTROL 最小長] | [!UICONTROL 文字列] | 取得時に受け入れられる値のために、文字列に含める必要がある最小文字数。 |
| [!UICONTROL 最大長] | [!UICONTROL 文字列] | 取得時に受け入れられる値のために、文字列に含める必要がある最大文字数。 |
| [!UICONTROL 最小値] | [!UICONTROL Double] | 取り込み時に受け入れられる倍精度浮動小数点数の最小値です。 取り込んだ値がここに入力した値と完全に一致する場合は、その値が受け入れられます。 この制約を使用する場合、「[!UICONTROL  排他的最小値 ]」制約は空白のままにする必要があります。 |
| [!UICONTROL 最大値] | [!UICONTROL ダブル] | 取り込み時に受け入れられる倍精度浮動小数点数の最大値です。 取り込んだ値がここに入力した値と完全に一致する場合は、その値が受け入れられます。 この制約を使用する場合、「[!UICONTROL  排他最大値 ]」制約は空白のままにする必要があります。 |
| [!UICONTROL 排他的最小値] | [!UICONTROL ダブル] | 取り込み時に受け入れられる倍精度浮動小数点数の最大値です。 取り込んだ値がここに入力した値と完全に一致する場合、値は拒否されます。 この制約を使用する場合、「[!UICONTROL  最小値 ]」（非排他）制約は空白のままにする必要があります。 |
| [!UICONTROL 排他的最大値] | [!UICONTROL ダブル] | 取り込み時に受け入れられる倍精度浮動小数点数の最大値です。 取り込んだ値がここに入力した値と完全に一致する場合、値は拒否されます。 この制約を使用する場合、「[!UICONTROL  最大値 ]」（非排他）制約は空白のままにする必要があります。 |

{style=&quot;table-layout:auto&quot;}

## 特別なフィールドタイプ {#special}

右側のレールには、選択したフィールドの特別な役割を指定するチェックボックスがいくつか表示されます。 これらのオプションの一部の使用例には、データモデリング戦略とダウンストリームの Platform サービスの使用方法に関する重要な考慮事項が含まれています。

これらの特殊なタイプについて詳しくは、次のドキュメントを参照してください。

* [[!UICONTROL 必須]](./required.md)
* [[!UICONTROL 配列]](./array.md)
* [[!UICONTROL Enum]](./enum.md)
* [[!UICONTROL ID]](./identity.md) （文字列フィールドにのみ使用可能）
* [[!UICONTROL 関係]](./relationship.md) （文字列フィールドに対してのみ使用可能）

技術的には特別なフィールドタイプではありませんが、[ オブジェクトタイプフィールドの定義 ](./object.md) のガイドを参照して、スキーマ構造が使用する場合のネストされたサブフィールドの定義の詳細を確認することをお勧めします。

## 次の手順

このガイドでは、UI で XDM フィールドを定義する方法の概要を説明しました。 フィールドは、クラスとフィールドグループを使用してのみスキーマに追加できます。 UI でこれらのリソースを管理する方法について詳しくは、[classes](../resources/classes.md) および [field groups](../resources/field-groups.md) の作成と編集に関するガイドを参照してください。

[!UICONTROL  スキーマ ] ワークスペースの機能について詳しくは、[[!UICONTROL  スキーマ ] ワークスペースの概要 ](../overview.md) を参照してください。
