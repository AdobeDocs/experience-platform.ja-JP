---
keywords: Experience Platform；ホーム；人気のあるトピック；API;API;XDM;XDMシステム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；フィールド；
solution: Experience Platform
title: UIでのXDMフィールドの定義
description: Experience PlatformユーザーインターフェイスでXDMフィールドを定義する方法を説明します。
topic-legacy: user guide
exl-id: 2adb03d4-581b-420e-81f8-e251cf3d9fb9
translation-type: tm+mt
source-git-commit: d425dcd9caf8fccd0cb35e1bac73950a6042a0f8
workflow-type: tm+mt
source-wordcount: '1325'
ht-degree: 4%

---

# UIでのXDMフィールドの定義

Adobe Experience Platformユーザーインターフェイスの[!DNL Schema Editor]を使用すると、カスタムのExperience Data Model(XDM)クラスとスキーマフィールドグループ内に独自のフィールドを定義できます。 このガイドでは、UIでXDMフィールドを定義する手順（各フィールドタイプで使用可能な設定オプションなど）について説明します。

## 前提条件

このガイドでは、XDMシステムに関する十分な理解が必要です。 Experience Platformエコシステム内でのXDMの役割の説明は[XDM overview](../../home.md)を参照し、クラスとフィールドグループがXDMスキーマにフィールドを寄与させる方法を学ぶために、スキーマ構成の基本[を参照してください。](../../schema/composition.md)

このガイドは必須ではありませんが、[UI](../../tutorials/create-schema-ui.md)でスキーマを構成する方法のチュートリアルに従って、[!DNL Schema Editor]の様々な機能に慣れることをお勧めします。

## フィールドを{#select-resource}に追加するリソースを選択

UIで新しいXDMフィールドを定義するには、まず[!DNL Schema Editor]内でスキーマを開く必要があります。 [!DNL Schema Library]で現在利用可能なスキーマに応じて、[新しいスキーマ](../resources/schemas.md#create)を作成するか、[既存のスキーマを選択して](../resources/schemas.md#edit)を編集します。

[!DNL Schema Editor]を開いたら、左のナビゲーションバーを使用して、フィールドを定義するクラスまたはフィールドグループを選択します。 リソースが組織によって定義されたカスタムリソースの場合、フィールドを追加または編集するためのコントロールがキャンバスに表示されます。 これらのコントロールは、スキーマ名の横に表示され、選択したクラスまたはフィールドグループで定義されているオブジェクトタイプのフィールドも表示されます。

![](../../images/ui/fields/overview/select-resource.png)

>[!NOTE]
>
>選択したクラスまたはフィールドグループがAdobeから提供されるコアリソースである場合、編集できないので、上記のコントロールは表示されません。 フィールドの追加先のスキーマがコアXDMクラスに基づいており、カスタムフィールドグループが含まれていない場合は、[新しいフィールドグループ](../resources/field-groups.md#create)を作成してスキーマに追加できます。

リソースに新しいフィールドを追加するには、キャンバスでスキーマ名の横にあるプラス(+)**アイコンを選択するか、フィールドを定義するオブジェクトタイプフィールドの横にあるアイコンを選択します。**

![](../../images/ui/fields/overview/plus-icon.png)

## リソース{#define}のフィールドを定義する

**プラス(+)**&#x200B;アイコンを選択すると、キャンバスに&#x200B;**[!UICONTROL 新しいフィールド]**&#x200B;が表示されます。このフィールドは、一意のテナントIDに名前が付けられたルートレベルオブジェクト内にあります（下の例では`_tenantId`）。 カスタムクラスとフィールドグループを通じてスキーマに追加されるすべてのフィールドは、Adobeが提供するクラスとフィールドグループの他のフィールドと競合しないように、この名前空間内に自動的に配置されます。

![](../../images/ui/fields/overview/new-field.png)

**[!UICONTROL フィールドプロパティ]**&#x200B;の右側のレールで、新しいフィールドの詳細を設定できます。 各フィールドには次の情報が必要です。

| Fieldプロパティ | 説明 |
| --- | --- |
| [!UICONTROL フィールド名] | フィールドを説明する一意の名前。 スキーマを保存すると、フィールドの名前を変更できなくなります。<br><br>名前は、camelCaseで記述するのが理想的です。英数字、ダッシュ、アンダースコア文字を含めることはできますが、**開始にアンダースコアを含めることはできません。**<ul><li>**正しい構文**:  `fieldName`</li><li>**使用可能：** `field_name2`,  `Field-Name`,  `field-name_3`</li><li>**誤った**:  `_fieldName`</li></ul> |
| [!UICONTROL 表示名] | フィールドのわかりやすい名前。 |
| [!UICONTROL タイプ] | フィールドに格納するデータの種類です。 このドロップダウンメニューから、XDMでサポートされている[標準スカラー型](../../schema/field-constraints.md)の1つ、または[!DNL Schema Registry]で定義済みの複数フィールド[データ型](../resources/data-types.md)の1つを選択できます。<br><br>また、「 **[!UICONTROL アドバンスタイプ]** 検索」を選択して、既存のデータタイプを検索およびフィルタリングし、より簡単に目的のタイプを見つけることもできます。 |

オプションで人間が読み取り可能な&#x200B;**[!UICONTROL 説明]**&#x200B;をフィールドに提供し、フィールドの使用目的に関するより多くのコンテキストを提供することもできます。

>[!NOTE]
>
>フィールドに選択した&#x200B;**[!UICONTROL タイプ]**&#x200B;によっては、右側のレールに追加の設定コントロールが表示される場合があります。 これらのコントロールの詳細については、[型固有のフィールドプロパティ](#type-specific-properties)の節を参照してください。
>
>右側のレールには、特別なフィールドタイプを指定するためのチェックボックスもあります。 詳しくは、[特別なフィールドタイプ](#special)の節を参照してください。

フィールドの設定が完了したら、「**[!UICONTROL 適用]**」を選択します。

![](../../images/ui/fields/overview/field-details.png)

カンバスが更新され、フィールドの名前とタイプが表示されます。右側のパネルには、他のプロパティに加えて、フィールドのパスがリストされます。

![](../../images/ui/fields/overview/field-added.png)

上記の手順に従って、スキーマにフィールドをさらに追加できます。 スキーマを保存すると、その基本クラスとフィールドグループも、変更が加えられた場合は保存されます。

>[!NOTE]
>
>1つのスキーマのフィールドグループまたはクラスに対して行った変更は、それらを使用する他のすべてのスキーマに反映されます。

## タイプ固有のフィールドプロパティ{#type-specific-properties}

新しいフィールドを定義する場合、右側のレールに、フィールドに対して選択した&#x200B;**[!UICONTROL タイプ]**&#x200B;に応じて、追加の設定オプションが表示される場合があります。 次の表に、これらの追加のフィールドプロパティと互換性のある型の概要を示します。

| Fieldプロパティ | 互換性のある型 | 説明 |
| --- | --- | --- |
| [!UICONTROL デフォルト値] | [!UICONTROL 文字列],重複 [!UICONTROL ,]長い [!UICONTROL ，長い]，長い [!UICONTROL ，短い，短い，短い，短い],   [!UICONTROL バイト，ブール値] | 取り込み中に他の値が指定されない場合に、このフィールドに割り当てられるデフォルト値。 この値は、フィールドで選択された種類に準拠する必要があります。 |
| [!UICONTROL パターン] | [!UICONTROL String] | 取り込み時に受け入れられるように、このフィールドの値が適合する必要がある[正規式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)。 |
| [!UICONTROL 形式] | [!UICONTROL 文字列] | 値が準拠する必要がある文字列に対して、事前定義済みの形式のリストから選択します。 次の形式を使用できます。 <ul><li>[[!UICONTROL date-time]](https://tools.ietf.org/html/rfc3339)</li><li>[[!UICONTROL 電子メール]](https://tools.ietf.org/html/rfc2822)</li><li>[[!UICONTROL hostname]](https://tools.ietf.org/html/rfc1123#page-13)</li><li>[[!UICONTROL ipv4]](https://tools.ietf.org/html/rfc791)</li><li>[[!UICONTROL ipv6]](https://tools.ietf.org/html/rfc2460)</li><li>[[!UICONTROL uri]](https://tools.ietf.org/html/rfc3986)</li><li>[[!UICONTROL uri-reference]](https://tools.ietf.org/html/rfc3986#section-4.1)</li><li>[[!UICONTROL url-template]](https://tools.ietf.org/html/rfc6570)</li><li>[[!UICONTROL json-pointer]](https://tools.ietf.org/html/rfc6901)</li></ul> |
| [!UICONTROL 最小長] | [!UICONTROL 文字列] | 取り込み時に値を受け入れるために文字列に含める必要がある最小文字数。 |
| [!UICONTROL 最大長] | [!UICONTROL 文字列] | 取り込み時に値を受け入れるために文字列に含める必要がある最大文字数。 |
| [!UICONTROL 最小値] | [!UICONTROL Double] | 取り込み時に重複が受け入れられる最小値。 取り込まれた値がここに入力された値と完全に一致する場合は、その値が受け入れられます。 この制約を使用する場合、「[!UICONTROL 排他的最小値]」制約は空白にする必要があります。 |
| [!UICONTROL 最大値] | [!UICONTROL 重複] | 取り込み時に重複が受け入れられる最大値。 取り込まれた値がここに入力された値と完全に一致する場合は、その値が受け入れられます。 この制約を使用する場合、「[!UICONTROL 排他的最大値]」制約は空白にする必要があります。 |
| [!UICONTROL 排他的最小値] | [!UICONTROL 重複] | 取り込み時に重複が受け入れられる最大値。 取り込まれた値がここに入力された値と完全に一致する場合、その値は拒否されます。 この制約を使用する場合、「[!UICONTROL 最小値]」（非排他的）制約は空白にする必要があります。 |
| [!UICONTROL 排他的最大値] | [!UICONTROL 重複] | 取り込み時に重複が受け入れられる最大値。 取り込まれた値がここに入力された値と完全に一致する場合、その値は拒否されます。 この制約を使用する場合、「[!UICONTROL 最大値]」（非排他的）制約は空白にする必要があります。 |

## 特殊フィールドの種類{#special}

右側のレールには、選択したフィールドに特殊な役割を指定するためのチェックボックスがいくつかあります。 これらのオプションの使用例には、データモデリング戦略およびダウンストリームプラットフォームサービスの使用方法に関する重要な考慮事項が含まれます。

これらの特殊なタイプについて詳しくは、次のドキュメントを参照してください。

* [[!UICONTROL 必須]](./required.md)
* [[!UICONTROL 配列]](./array.md)
* [[!UICONTROL Enum]](./enum.md)
* [[!UICONTROL ID]](./identity.md) （文字列フィールドにのみ使用可能）
* [[!UICONTROL Relationship]](./relationship.md) （文字列フィールドにのみ使用可能）

技術的には特別なフィールドタイプではありませんが、スキーマ構造体の場合のネストされたサブフィールドの定義について詳しくは、[defining object-type fields](./object.md)のガイドを参照してください。

## 次の手順

このガイドでは、UIでXDMフィールドを定義する方法の概要を説明しました。 フィールドは、クラスとフィールドグループを使用してのみスキーマに追加できます。 UIでこれらのリソースを管理する方法について詳しくは、[クラス](../resources/classes.md)と[フィールドグループ](../resources/field-groups.md)の作成および編集に関するガイドを参照してください。

[!UICONTROL スキーマ]ワークスペースの機能について詳しくは、[[!UICONTROL スキーマ]ワークスペースの概要](../overview.md)を参照してください。
