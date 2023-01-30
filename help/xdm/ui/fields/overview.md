---
keywords: Experience Platform；ホーム；人気のトピック；API;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；フィールド；
solution: Experience Platform
title: UI での XDM フィールドの定義
description: XDM ユーザーインターフェイスで XDM フィールドを定義するExperience Platformについて説明します。
exl-id: 2adb03d4-581b-420e-81f8-e251cf3d9fb9
source-git-commit: f1a8bcc7c1ba33d74e1f687b4cfd83bddce2fadc
workflow-type: tm+mt
source-wordcount: '1407'
ht-degree: 4%

---

# UI での XDM フィールドの定義

この [!DNL Schema Editor] Adobe Experience Platformユーザーインターフェイスを使用すると、カスタム Experience Data Model(XDM) クラスとスキーマフィールドグループ内に独自のフィールドを定義できます。 このガイドでは、UI で XDM フィールドを定義する手順と、各フィールドタイプで使用可能な設定オプションについて説明します。

## 前提条件

このガイドでは、XDM システムに関する十分な知識が必要です。 詳しくは、 [XDM の概要](../../home.md) Experience Platformエコシステム内での XDM の役割、および [スキーマ構成の基本](../../schema/composition.md) クラスとフィールドグループが XDM スキーマにフィールドを貢献する方法を学ぶには、以下を参照してください。

このガイドは必須ではありませんが、 [UI でのスキーマの構成](../../tutorials/create-schema-ui.md) の様々な機能を身に付ける [!DNL Schema Editor].

## フィールドを追加するリソースを選択 {#select-resource}

UI で新しい XDM フィールドを定義するには、まず [!DNL Schema Editor]. 現在、 [!DNL Schema Library]を使用する場合、 [新しいスキーマの作成](../resources/schemas.md#create) または [編集する既存のスキーマを選択](../resources/schemas.md#edit).

次に、 [!DNL Schema Editor] を開くと、フィールドを追加または編集するためのコントロールがキャンバスに表示されます。 これらのコントロールは、スキーマ名の横に表示され、選択したクラスまたはフィールドグループの下で定義されたオブジェクトタイプのフィールドも表示されます。

![](../../images/ui/fields/overview/select-resource.png)

>[!WARNING]
>
>標準フィールドグループで提供されるオブジェクトにフィールドを追加しようとすると、そのフィールドグループはカスタムフィールドグループに変換され、元のフィールドグループは使用できなくなります。 詳しくは、 [標準フィールドグループへのフィールドの追加](../resources/schemas.md#custom-fields-for-standard-groups) （スキーマ UI ガイド）を参照してください。

リソースに新しいフィールドを追加するには、 **プラス (+)** キャンバスでスキーマの名前の横、またはフィールドを定義するオブジェクトタイプフィールドの横にあるアイコン。

![](../../images/ui/fields/overview/plus-icon.png)

フィールドを直接スキーマに追加するか、構成するクラスとフィールドグループに追加するかに応じて、フィールドの追加に必要な手順は異なります。 このドキュメントの残りの部分では、スキーマ内でフィールドが表示される場所に関係なく、フィールドのプロパティを設定する方法に焦点を当てます。 フィールドをスキーマに追加する様々な方法について詳しくは、『スキーマ UI ガイド』の次の節を参照してください。

* [フィールドグループにフィールドを追加](../resources/schemas.md#add-fields)
* [スキーマに直接フィールドを追加](../resources/schemas.md#add-individual-fields)

## フィールドのプロパティを定義する {#define}

選択後、 **プラス (+)** アイコン、 **[!UICONTROL 名称未設定フィールド]** プレースホルダーがキャンバスに表示されます。

![](../../images/ui/fields/overview/new-field.png)

の下の右側のレールで **[!UICONTROL フィールドプロパティ]**&#x200B;を使用すると、新しいフィールドの詳細を設定できます。 各フィールドには、次の情報が必要です。

| Field プロパティ | 説明 |
| --- | --- |
| [!UICONTROL フィールド名] | フィールドを説明する一意の名前。 スキーマを保存した後は、フィールドの名前を変更できません。 この値は、コード内のフィールドおよび他のダウンストリームアプリケーションでのフィールドの識別および参照に使用されます<br><br>名前は camelCase で書くのが理想的です。 英数字、ダッシュ、アンダースコアの各文字を使用できますが、 **次の場合は不可** まず、アンダースコアを使用します。<ul><li>**正しい**: `fieldName`</li><li>**許容可能：** `field_name2`, `Field-Name`, `field-name_3`</li><li>**誤った**: `_fieldName`</li></ul> |
| [!UICONTROL 表示名] | フィールドの表示名。 これは、スキーマエディターキャンバス内のフィールドを表すために使用される名前です。 |
| [!UICONTROL タイプ] | フィールドに格納するデータのタイプ。 このドロップダウンメニューから、 [標準スカラー型](../../schema/field-constraints.md) XDM またはマルチフィールドの 1 つでサポートされます。 [データタイプ](../resources/data-types.md) 以前に [!DNL Schema Registry].<br><br>また、 **[!UICONTROL 詳細タイプ検索]** 既存のデータ型を検索およびフィルタリングし、目的の型を見つけやすくする。 |

{style=&quot;table-layout:auto&quot;}

オプションで人間が読み取れる **[!UICONTROL 説明]** をフィールドに追加して、フィールドの意図した使用例に関するより多くのコンテキストを提供します。

>[!NOTE]
>
>に応じて **[!UICONTROL タイプ]** 「 」フィールドで「 」を選択した場合、追加の設定コントロールが右側のパネルに表示される場合があります。 詳しくは、 [タイプ固有のフィールドプロパティ](#type-specific-properties) を参照してください。
>
>右側のレールには、特別なフィールドタイプを指定するためのチェックボックスも表示されます。 詳しくは、 [特別なフィールドタイプ](#special) を参照してください。

フィールドの設定が完了したら、「 **[!UICONTROL 適用]**.

![](../../images/ui/fields/overview/field-details.png)

キャンバスが更新され、新しく追加されたフィールドが、一意のテナント ID( `_tenantId` （以下の例）。 Adobeが提供するクラスやフィールドグループの他のフィールドとの競合を防ぐために、スキーマに追加されたすべてのカスタムフィールドは、自動的にこの名前空間内に配置されます。 右側のレールに、他のプロパティに加えて、フィールドのパスが表示されるようになりました。

![](../../images/ui/fields/overview/field-added.png)

引き続き上記の手順に従って、スキーマにフィールドを追加できます。 スキーマを保存すると、基本クラスとフィールドグループも、変更が加えられた場合は保存されます。

>[!NOTE]
>
>1 つのスキーマのフィールドグループまたはクラスに対して行った変更は、それらを使用する他のすべてのスキーマに反映されます。

## タイプ固有のフィールドプロパティ {#type-specific-properties}

新しいフィールドを定義する際に、右側のパネルに追加の設定オプションが表示される場合があります。 **[!UICONTROL タイプ]** 「 」フィールドで「 」を選択します。 次の表に、その他のフィールドプロパティと互換性のあるタイプの概要を示します。

| Field プロパティ | 互換性のあるタイプ | 説明 |
| --- | --- | --- |
| [!UICONTROL デフォルト値] | [!UICONTROL 文字列], [!UICONTROL ダブル], [!UICONTROL Long], [!UICONTROL 整数], [!UICONTROL Short], [!UICONTROL Byte], [!UICONTROL ブール値] | 取り込み中に他の値が指定されない場合にこのフィールドに割り当てられるデフォルト値。 この値は、フィールドの選択されたタイプに準拠している必要があります。 |
| [!UICONTROL パターン] | [!UICONTROL 文字列] | A [正規表現](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions) 取り込み時に受け入れられるように、このフィールドの値がに準拠している必要があります。 |
| [!UICONTROL 形式] | [!UICONTROL 文字列] | 値が準拠する必要がある文字列に対して、事前定義済みの形式のリストから選択します。 使用可能な形式は次のとおりです。 <ul><li>[[!UICONTROL date-time]](https://tools.ietf.org/html/rfc3339)</li><li>[[!UICONTROL 電子メール]](https://tools.ietf.org/html/rfc2822)</li><li>[[!UICONTROL hostname]](https://tools.ietf.org/html/rfc1123#page-13)</li><li>[[!UICONTROL ipv4]](https://tools.ietf.org/html/rfc791)</li><li>[[!UICONTROL ipv6]](https://tools.ietf.org/html/rfc2460)</li><li>[[!UICONTROL uri]](https://tools.ietf.org/html/rfc3986)</li><li>[[!UICONTROL uri-reference]](https://tools.ietf.org/html/rfc3986#section-4.1)</li><li>[[!UICONTROL url-template]](https://tools.ietf.org/html/rfc6570)</li><li>[[!UICONTROL json-pointer]](https://tools.ietf.org/html/rfc6901)</li></ul> |
| [!UICONTROL 最小長] | [!UICONTROL 文字列] | 取得時に受け入れられる値の最小文字数。 |
| [!UICONTROL 最大長] | [!UICONTROL 文字列] | 取得時に受け入れられる値のために、文字列に含める必要がある最大文字数です。 |
| [!UICONTROL 最小値] | [!UICONTROL Double] | 取り込み時に受け入れられる倍精度浮動小数点数の最小値です。 取り込んだ値がここに入力した値と完全に一致する場合は、値が受け入れられます。 この制約を使用する場合、[!UICONTROL 排他的な最小値]「 」制約は空白のままにする必要があります。 |
| [!UICONTROL 最大値] | [!UICONTROL Double] | 取り込み時に受け入れられる倍精度浮動小数点型 (Double) の最大値です。 取り込んだ値がここに入力した値と完全に一致する場合は、値が受け入れられます。 この制約を使用する場合、[!UICONTROL 排他的な最大値]「 」制約は空白のままにする必要があります。 |
| [!UICONTROL 排他的な最小値] | [!UICONTROL Double] | 取り込み時に受け入れられる倍精度浮動小数点型 (Double) の最大値です。 取り込んだ値がここに入力した値と完全に一致する場合、値は拒否されます。 この制約を使用する場合、[!UICONTROL 最小値]&quot; （非排他）制約は空白のままにする必要があります。 |
| [!UICONTROL 排他的な最大値] | [!UICONTROL Double] | 取り込み時に受け入れられる倍精度浮動小数点型 (Double) の最大値です。 取り込んだ値がここに入力した値と完全に一致する場合、値は拒否されます。 この制約を使用する場合、[!UICONTROL 最大値]&quot; （非排他）制約は空白のままにする必要があります。 |

{style=&quot;table-layout:auto&quot;}

## 特別なフィールドタイプ {#special}

右側のレールには、選択したフィールドの特別な役割を指定するためのチェックボックスがいくつか用意されています。 これらのオプションの一部の使用例には、データモデリング戦略とダウンストリームの Platform サービスの使用方法に関する重要な考慮事項が含まれています。

これらの特殊なタイプについて詳しくは、次のドキュメントを参照してください。

* [[!UICONTROL 必須]](./required.md)
* [[!UICONTROL 配列]](./array.md)
* [[!UICONTROL 列挙]](./enum.md)
* [[!UICONTROL ID]](./identity.md) （文字列フィールドに対してのみ使用可能）
* [[!UICONTROL 関係]](./relationship.md) （文字列フィールドに対してのみ使用可能）

技術的には特別なフィールドタイプではありませんが、 [オブジェクトタイプフィールドの定義](./object.md) スキーマ構造の場合に、ネストされたサブフィールドの定義について詳しくは、を参照してください。

## 次の手順

このガイドでは、UI で XDM フィールドを定義する方法の概要を示しました。 フィールドは、クラスとフィールドグループを使用してのみスキーマに追加できます。 UI でこれらのリソースを管理する方法について詳しくは、作成と編集に関するガイドを参照してください [クラス](../resources/classes.md) および [フィールドグループ](../resources/field-groups.md).

の機能の詳細については、 [!UICONTROL スキーマ] ワークスペース ( [[!UICONTROL スキーマ] workspace の概要](../overview.md).
