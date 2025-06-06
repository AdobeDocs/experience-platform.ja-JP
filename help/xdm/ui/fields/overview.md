---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；フィールド；
solution: Experience Platform
title: UI での XDM フィールドの定義
description: Experience Platform ユーザーインターフェイスで XDM フィールドを定義する方法を説明します。
exl-id: 2adb03d4-581b-420e-81f8-e251cf3d9fb9
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1607'
ht-degree: 1%

---

# UI での XDM フィールドの定義

Adobe Experience Platform ユーザーインターフェイスの [!DNL Schema Editor] を使用すると、カスタム Experience Data Model （XDM）クラスおよびスキーマフィールドグループ内で独自のフィールドを定義できます。 このガイドでは、各フィールドタイプで使用可能な設定オプションなど、UI で XDM フィールドを定義する手順を説明します。

## 前提条件

このガイドでは、XDM システムに関する十分な知識が必要です。 クラスとフィールドグループが XDM スキーマにフィールドを提供する方法については、Experience Platform エコシステムでの XDM の役割の概要および [ スキーマ構成の基本 ](../../schema/composition.md) を参照してください [XDM の概要 ](../../home.md)。

このガイドには必要ありませんが、[UI でのスキーマの作成 ](../../tutorials/create-schema-ui.md) に関するチュートリアルに従って、[!DNL Schema Editor] の様々な機能を理解することをお勧めします。

## フィールドを追加するリソースを選択 {#select-resource}

UI で新しい XDM フィールドを定義するには、まず [!DNL Schema Editor] でスキーマを開く必要があります。 [!DNL Schema Library] で現在使用可能なスキーマに応じて、[ 新しいスキーマを作成 ](../resources/schemas.md#create) または [ 編集する既存のスキーマを選択 ](../resources/schemas.md#edit) を選択できます。

[!DNL Schema Editor] を開くと、フィールドを追加するためのコントロールがキャンバスに表示されます。 これらのコントロールは、スキーマ名の横に表示されるほか、選択したクラスまたはフィールドグループの下で定義されたオブジェクトタイプフィールドにも表示されます。

![ 追加アイコンがハイライト表示されたスキーマエディター。](../../images/ui/fields/overview/select-resource.png)

>[!WARNING]
>
>標準フィールドグループで提供されるオブジェクトにフィールドを追加しようとすると、そのフィールドグループはカスタムフィールドグループに変換され、元のフィールドグループは使用できなくなります。 詳しくは、スキーマ UI ガイドの [ 標準フィールドグループへのフィールドの追加 ](../resources/schemas.md#custom-fields-for-standard-groups) の節を参照してください。

リソースに新しいフィールドを追加するには、キャンバスでスキーマ名の横にある **プラス （+）** アイコン、またはフィールドを定義するオブジェクトタイプフィールドの横にある「+」アイコンを選択します。

![ 追加アイコンがハイライト表示されたスキーマエディター。](../../images/ui/fields/overview/plus-icon.png)

フィールドをスキーマに直接追加するか、その構成要素のクラスとフィールドグループに追加するかによって、フィールドの追加に必要な手順は異なります。 このドキュメントでは、スキーマ内のそのフィールドの表示場所に関係なく、フィールドのプロパティを設定する方法について重点的に説明します。 スキーマにフィールドを追加できる様々な方法について詳しくは、スキーマ UI ガイドの次の節を参照してください。

* [フィールドグループへのフィールドの追加](../resources/schemas.md#add-fields)
* [フィールドをスキーマに直接追加](../resources/schemas.md#add-individual-fields)

## フィールドのプロパティの定義 {#define}

**プラス（+）** アイコンを選択すると、「**[!UICONTROL 名称未設定フィールド]** プレースホルダーがキャンバスに表示されます。

![ 新しい名称未設定フィールドがハイライト表示されたスキーマエディター。](../../images/ui/fields/overview/new-field.png)

**[!UICONTROL フィールドプロパティ]** の下の右側のパネルで、新しいフィールドの詳細を設定できます。 各フィールドには次の情報が必要です。

| フィールドプロパティ | 説明 |
| --- | --- |
| [!UICONTROL &#x200B; フィールド名 &#x200B;] | フィールドの一意でわかりやすい名前。 スキーマが保存されると、フィールドの名前は変更できません。 この値は、コード内および他のダウンストリームアプリケーションでフィールドを識別および参照するために使用されます <br><br> この名前は、キャメルケースで記述することが理想的です。 英数字またはアンダースコア文字が含まれる場合がありますが、アンダースコアで始まる **ない** 場合があります。<ul><li>**正解**:`fieldName`</li><li>**指定できます：** `field_name2`、`fieldName_3`</li><li>**間違った形式**: `_fieldName`</li></ul> |
| [!UICONTROL 表示名] | フィールドの表示名。 これは、スキーマエディターキャンバス内のフィールドを表すために使用される名前です。 [ 表示名の切り替え ](../resources/schemas.md#display-name-toggle) を使用して、フィールド名を表示名に変更できます。 |
| [!UICONTROL タイプ] | フィールドに含まれるデータのタイプ。 このドロップダウンメニューから、XDM でサポートされている [ 標準スカラータイプ ](../../schema/field-constraints.md) の 1 つ、または [!DNL Schema Registry] で以前に定義されている複数フィールド [ データタイプ ](../resources/data-types.md) の 1 つを選択できます。<br> メモ：「マップ」データタイプを選択すると、「[!UICONTROL &#x200B; マップ値タイプ &#x200B;]」プロパティが表示されます。<br><br> また、**[!UICONTROL 高度なタイプ検索]** を選択して、既存のデータタイプを検索およびフィルタリングし、目的のタイプを見つけやすくすることもできます。 |
| [!UICONTROL &#x200B; 値タイプをマッピング &#x200B;] | フィールドのデータタイプとして [!UICONTROL &#x200B; マップ &#x200B;] を選択する場合、この値は必須です。 マップに使用できる値は [!UICONTROL &#x200B; 文字列 &#x200B;] および [!UICONTROL &#x200B; 整数 &#x200B;] です。 使用可能なオプションのドロップダウンリストから値を選択します。<br> タイプ固有のフィールドプロパティ [ について詳しくは ](#type-specific-properties) フィールド定義の概要を参照してください。 |

{style="table-layout:auto"}

各フィールドに説明とメモを入力することもできます。 「**[!UICONTROL 説明]**」フィールドを使用してコンテキストを追加し、マップデータタイプの機能を説明します。 これにより、実装のメンテナンス性と読みやすさが向上します。 最初の説明を補完するメモを追加することもできます。 これにより、開発者がコードベースのコンテキスト内でマップを効果的に理解、維持および利用するのに役立つ、より詳細で具体的な情報が提供されます。 |


>[!NOTE]
>
>フィールドに選択した **[!UICONTROL タイプ]** によっては、追加の設定コントロールが右側のパネルに表示される場合があります。 これらのコントロールについて詳しくは、[ タイプ固有のフィールドプロパティ ](#type-specific-properties) の節を参照してください。
>
>右側のパネルには、特別なフィールドタイプを指定するためのチェックボックスもあります。 詳しくは、[ 特別なフィールドタイプ ](#special) の節を参照してください。

フィールドの設定が完了したら、「**[!UICONTROL 適用]**」を選択します。

![ スキーマエディターの [!UICONTROL &#x200B; フィールドプロパティ &#x200B;] セクションがハイライト表示されている様子 ](../../images/ui/fields/overview/field-details.png)

キャンバスが更新され、一意のテナント ID を名前空間とするオブジェクト内に配置された、新しく追加されたフィールドが表示されます（以下の例に `_tenantId` 示します）。 Adobeが提供するクラスおよびフィールドグループの他のフィールドとの競合を防ぐために、スキーマに追加されたすべてのカスタムフィールドはこの名前空間内に自動的に配置されます。 右側のパネルに、フィールドのパスとその他のプロパティが表示されるようになりました。

![ スキーマ図の新しいフィールドと、[!UICONTROL &#x200B; フィールドプロパティ &#x200B;] セクションの対応するパスがハイライト表示されます。](../../images/ui/fields/overview/field-added.png)

引き続き上記の手順に従って、スキーマにさらにフィールドを追加できます。 スキーマを保存すると、基本クラスとフィールドグループも、変更がある場合は保存されます。

>[!NOTE]
>
>1 つのスキーマのフィールドグループまたはクラスに対して行った変更は、それらを使用する他のすべてのスキーマに反映されます。

## タイプ固有のフィールドのプロパティ {#type-specific-properties}

新しいフィールドを定義する際に、フィールドに選択した **[!UICONTROL タイプ]** に応じて、追加の設定オプションが右側のパネルに表示される場合があります。 次の表に、これらの追加のフィールドプロパティと、互換性のあるタイプの概要を示します。

| フィールドプロパティ | 互換性のあるタイプ | 説明 |
| --- | --- | --- |
| [!UICONTROL &#x200B; 値タイプをマッピング &#x200B;] | [!UICONTROL マップ] | [!UICONTROL &#x200B; マップの値のタイプ &#x200B;] プロパティは、「[!UICONTROL &#x200B; タイプ &#x200B;]」ドロップダウンオプションから「マップの値」を選択した場合にのみ UI に表示されます。 マップには文字列タイプと整数値タイプを選択できます。<br>![ タイプおよびマップ値タイプのフィールドがハイライト表示されたスキーマエディター。](../../images/ui/fields/overview/map-type.png " タイプおよびマップ値タイプのフィールドがハイライト表示されたスキーマエディター。"){width="100" zoomable="yes"}<br> メモ：API で作成された、文字列タイプでも整数タイプでもないマップデータタイプは、「[!UICONTROL Complex]」データタイプとして表示されます。 UI を使用して「[!UICONTROL Complex]」データタイプを作成することはできません。 |
| [!UICONTROL &#x200B; パターン &#x200B;] | [!UICONTROL 文字列] | 取り込み中に受け入れるために、このフィールドの値が準拠する必要がある [ 正規表現 ](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)。 |
| [!UICONTROL 形式] | [!UICONTROL 文字列] | 値が準拠する必要がある文字列の事前定義済み形式のリストから選択します。 使用可能な形式は次のとおりです。 <ul><li>[[!UICONTROL &#x200B; 日時 &#x200B;]](https://tools.ietf.org/html/rfc3339)</li><li>[[!UICONTROL &#x200B; 電子メール &#x200B;]](https://tools.ietf.org/html/rfc2822)</li><li>[[!UICONTROL &#x200B; ホスト名 &#x200B;]](https://tools.ietf.org/html/rfc1123#page-13)</li><li>[[!UICONTROL ipv4]](https://tools.ietf.org/html/rfc791)</li><li>[[!UICONTROL ipv6]](https://tools.ietf.org/html/rfc2460)</li><li>[[!UICONTROL uri]](https://tools.ietf.org/html/rfc3986)</li><li>[[!UICONTROL uri-reference]](https://tools.ietf.org/html/rfc3986#section-4.1)</li><li>[[!UICONTROL url-template]](https://tools.ietf.org/html/rfc6570)</li><li>[[!UICONTROL json-pointer]](https://tools.ietf.org/html/rfc6901)</li></ul> |
| [!UICONTROL &#x200B; 最小長 &#x200B;] | [!UICONTROL 文字列] | 取り込み中に値を受け入れるために文字列に含める必要がある最小文字数。 |
| [!UICONTROL &#x200B; 最大長 &#x200B;] | [!UICONTROL 文字列] | 取り込み中に値を受け入れるために文字列に含める必要がある最大文字数。 |
| [!UICONTROL &#x200B; 最小値 &#x200B;] | [!UICONTROL 倍精度浮動小数点] | 取り込み中に受け入れられる Double の最小値。 取り込まれた値がここに入力した値と完全に一致する場合、値は受け入れられます。 この制約を使用する場合、「排他的最小値 」制約は空白のままにする必要があります 。 |
| [!UICONTROL &#x200B; 最大値 &#x200B;] | [!UICONTROL 倍精度浮動小数点] | 取り込み中に受け入れられる Double の最大値。 取り込まれた値がここに入力した値と完全に一致する場合、値は受け入れられます。 この制約を使用する場合、「排他的最大値 [!UICONTROL &#x200B; 制約は空白のままにする必要 &#x200B;] あります。 |
| [!UICONTROL &#x200B; 排他的最小値 &#x200B;] | [!UICONTROL 倍精度浮動小数点] | 取り込み中に受け入れられる Double の最大値。 取り込まれた値がここに入力した値と完全に一致する場合、値は拒否されます。 この制約を使用する場合、「[!UICONTROL &#x200B; 最小値 &#x200B;]」（非排他的）制約は空白のままにする必要があります。 |
| [!UICONTROL &#x200B; 専用最大値 &#x200B;] | [!UICONTROL 倍精度浮動小数点] | 取り込み中に受け入れられる Double の最大値。 取り込まれた値がここに入力した値と完全に一致する場合、値は拒否されます。 この制約を使用する場合、「[!UICONTROL &#x200B; 最大値 &#x200B;]」（非排他的）制約は空白のままにする必要があります。 |

{style="table-layout:auto"}

## 特殊なフィールドタイプ {#special}

右側のパネルには、選択したフィールドの特別な役割を指定するためのチェックボックスがいくつか表示されます。 これらのオプションの一部のユースケースでは、データモデリング方法と、ダウンストリーム Experience Platform サービスの使用方法に関する重要な考慮事項が含まれます。

これらの特殊タイプの詳細については、次のドキュメントを参照してください。

* [マップ](./map.md)
* [[!UICONTROL 必須]](./required.md)
* [[!UICONTROL 配列]](./array.md)
* [[!UICONTROL &#x200B; 列挙 &#x200B;]](./enum.md)
* [[!UICONTROL ID]](./identity.md) （文字列フィールドでのみ使用可能）
* [[!UICONTROL &#x200B; 関係 &#x200B;]](./relationship.md) （文字列フィールドでのみ使用可能）

技術的には特別なフィールドタイプではありませんが、[ オブジェクトタイプのフィールドの定義 ](./object.md) に関するガイドにアクセスして、スキーマ構造に応じたネストされたサブフィールドの定義について詳しく確認することもお勧めします。

## 次の手順

このガイドでは、UI で XDM フィールドを定義する方法の概要を説明しました。 フィールドは、クラスとフィールドグループを使用してのみスキーマに追加できます。 UI でこれらのリソースを管理する方法について詳しくは、[ クラス ](../resources/classes.md) および [ フィールドグループ ](../resources/field-groups.md) の作成と編集に関するガイドを参照してください。

[!UICONTROL &#x200B; スキーマ &#x200B;] ワークスペースの機能について詳しくは、[[!UICONTROL &#x200B; スキーマ &#x200B;] ワークスペースの概要 ](../overview.md) を参照してください。
