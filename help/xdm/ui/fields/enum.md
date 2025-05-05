---
keywords: Experience Platform；ホーム；人気のトピック；api;API;XDM;XDM システム；エクスペリエンスデータモデル；データモデル；ui；ワークスペース；列挙；フィールド；
solution: Experience Platform
title: UI での列挙フィールドと推奨値の定義
description: Experience Platform ユーザーインターフェイスで文字列フィールドの列挙と推奨値を定義する方法について説明します。
exl-id: 67ec5382-31de-4f8d-9618-e8919bb5a472
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1257'
ht-degree: 8%

---

# UI で列挙と推奨値を定義 {#enums-and-suggested-values}

>[!CONTEXTUALHELP]
>id="platform_xdm_enum_suggestedvalue"
>title="列挙と推奨値"
>abstract="**列挙**&#x200B;は、文字列フィールドを制約して、事前定義済みの値のセットに一致するデータのみを取り込みます。各列挙制約には、セグメント化 UI の属性ドロップダウンに入力する&#x200B;**表示名**&#x200B;を割り当てることができます。フィールドの&#x200B;**推奨値**&#x200B;は、取り込みを制限するものではなく、セグメント化に表示される表示名を決定するだけです。共通のクラスまたはフィールドグループに属するフィールドを共有する複数のスキーマがあり、各スキーマ間でそのフィールドに対して異なる列挙または推奨値を定義した場合、これらの値は結合されて、結合スキーマに追加されます。"

エクスペリエンスデータモデル（XDM）では、文字列フィールドに受け入れられる値や提案される値の定義済みセットを指定して、そのフィールドに取り込む値やセグメント化での動作を、より詳細に制御できます。

**[!UICONTROL 列挙]** 文字列フィールドに取り込むことができる値を、事前定義済みのセットに制限します。 列挙フィールドにデータを取り込もうとすると、その値が設定で定義されている値と一致しない場合、取り込みは拒否されます。

列挙とは異なり、「**[!UICONTROL 推奨値]**」オプションを使用すると、取り込む値を制限しない文字列フィールドの推奨値セットを示すことができます。 代わりに、文字列フィールドを属性として含める場合、推奨値は、[ セグメント化 UI](../../../segmentation/ui/overview.md) で使用できる事前定義済みの値に影響を与えます。

Adobe Experience Platform ユーザーインターフェイスで [ 新しいフィールドを定義 ](./overview.md#define) し、タイプを [!UICONTROL &#x200B; 文字列 &#x200B;] に設定すると、そのフィールドの [enum](#enum) または [ 推奨値 ](#suggested-values) を定義するオプションが提供されます。

![UI の文字列フィールドに対して「列挙と推奨値」オプションが有効になっている様子を示す画像 ](../../images/ui/fields/enum/enum-options-selected.png)

このドキュメントでは、[!UICONTROL &#x200B; スキーマ &#x200B;] UI ワークスペースで列挙と推奨値を定義する方法を説明します。 UI での設定方法やダウンストリーム効果など、列挙と推奨値の概要については、次のビデオをご覧ください。

>[!VIDEO](https://video.tv.adobe.com/v/3413679/?quality=12&learn=on&captions=jpn)

## 列挙の定義 {#enum}

**[!UICONTROL 列挙と推奨値]** を選択してから、「**[!UICONTROL 列挙]** を選択します。 追加のコントロールが表示され、列挙の値の制約を指定できます。 制約を追加するには、「**[!UICONTROL 行を追加]**」を選択します。

![UI で「列挙」オプションが選択されていることを示す画像 ](../../images/ui/fields/enum/enum-add-row.png)

「**[!UICONTROL 値]**」列で、フィールドの制約対象となる正確な値を指定する必要があります。 オプションで、制約にも人間にとってわかりやすい **[!UICONTROL 表示名]** を指定できます。これは、セグメント化での値の表現方法に影響します。

引き続き **[!UICONTROL 行を追加]** を使用して、必要な制約とオプションラベルを列挙に追加するか、以前に追加した行の横にある削除アイコン（![ 削除アイコンの画像 ](/help/images/icons/remove-circle.png)）を選択して削除します。 終了したら、「**[!UICONTROL 適用]** を選択して、変更をスキーマに適用します。

![UI の文字列フィールドに入力されている列挙値と表示名を示す画像 ](../../images/ui/fields/enum/enum-confirm.png)

キャンバスが更新され、変更が反映されます。 今後、このスキーマを参照する際は、右側のパネル内の列挙フィールドの制約を表示して編集できます。

## 推奨値を定義 {#suggested-values}

**[!UICONTROL 列挙と推奨値]** を選択してから、**[!UICONTROL 推奨値]** を選択して、追加のコントロールを表示します。 ここから **[!UICONTROL 行を追加]** を選択して、推奨値の追加を開始します。

![UI で「推奨値」オプションが選択されていることを示す画像 ](../../images/ui/fields/enum/suggested-add-row.png)

**[!UICONTROL 表示名]** 列の下で、セグメント化 UI に表示する値にわかりやすい名前を付けます。 推奨値をさらに追加するには、「**[!UICONTROL 行を追加]**」を再度選択し、必要に応じてプロセスを繰り返します。 以前に追加した行を削除するには、該当する行の横にある ![ 削除アイコン ](/help/images/icons/remove-circle.png) を選択します。

終了したら、「**[!UICONTROL 適用]** を選択して、変更をスキーマに適用します。

![UI の文字列フィールドに入力されている列挙値と表示名を示す画像 ](../../images/ui/fields/enum/suggested-confirm.png)

>[!NOTE]
>
>フィールドの更新された推奨値がセグメント化 UI に反映されるまでに、約 5 分の遅延があります。

### 標準フィールドの推奨値の管理

標準 XDM コンポーネントの一部のフィールドには、[[!UICONTROL XDM ExperienceEvent] クラス ](../../classes/experienceevent.md) の `eventType` など、独自の推奨値が含まれています。 標準フィールドに推奨値を追加作成することはできますが、組織で定義されていない推奨値を変更または削除することはできません。 UI で標準フィールドを表示すると、推奨値は表示されますが、読み取り専用です。

![UI の文字列フィールドに入力されている列挙値と表示名を示す画像 ](../../images/ui/fields/enum/suggested-standard.png)

標準フィールドに新しい推奨値を追加するには、「**[!UICONTROL 行を追加]**」を選択します。 組織が以前に追加した推奨値を削除するには、該当する行の横にある ![ 削除アイコン ](/help/images/icons/remove-circle.png) を選択します。

![UI の文字列フィールドに入力されている列挙値と表示名を示す画像 ](../../images/ui/fields/enum/suggested-standard-add.png)

<!-- ### Removing suggested values for standard fields

Only suggested values that you define can be removed from a standard field. Existing suggested values can be disabled so that they no longer appear in the segmentation dropdown, but they cannot be removed outright.

For example, consider a profile schema where the a suggested value for the standard `person.gender` field is disabled:

![Image showing the enum values and display names filled out for the string field in the UI](../../images/ui/fields/enum/standard-enum-disabled.png)

In this example, the display name "[!UICONTROL Non-specific]" is now disabled from being shown in the segmentation dropdown list. However, the value `non_specific` is still part of the list of enumerated fields and is therefore still allowed on ingestion. In other words, you cannot disable the actual enum value for the standard field as it would go against the principle of only allowing changes that make a field less restrictive.

See the [section below](#evolution) for more information on the rules for updating enums and suggested values for existing schema fields. -->

## 列挙と推奨値の進化ルール {#evolution}

列挙フィールドを含むスキーマを使用してデータをExperience Platformに取り込んだ後、スキーマ定義に加えたさらなる変更は、システム内に既にあるデータに従う必要があります。 通常、既存のフィールドに加えた変更は、そのフィールドのみを制限 **軽減** できます。 フィールドには、既に設定されている制限よりも厳しい制限を適用することはできません。

列挙と推奨値に関しては、次のルールが取り込み後に適用されます。

* 標準フィールドとカスタムフィールドの推奨値を、既存の推奨値で追加すること **できます**。
* 既存の推奨値を使用して、カスタムフィールドから推奨値を削除 **できます**。
* 既存のカスタム列挙フィールドに新しい列挙値を追加 **できます**。
* カスタムフィールドの列挙値を推奨値のみに切り替える **CAN** か、列挙のない文字列または推奨値に変換します。 **この切り替えは、適用後に元に戻すことはできません。**
* 標準フィールドから列挙や推奨値を削除することは **できません**。
* 既存の列挙がないフィールドに列挙値を追加することは **できません**。
* カスタムフィールドには、既存の列挙値の一部を削除することはできま **ん**。
* 推奨値から列挙に切り替えることはできま **ん**。

## 列挙と推奨値のルールの結合 {#merging}

複数のスキーマが異なる設定で同じ列挙フィールドを使用し、それらのスキーマが結合に含まれている場合、列挙の違いの紐付け方法に関しては、特定のルールが適用されます。 正確なルールは、スキーマが（`eventType` のように）同じ標準フィールドを参照しているか、異なるフィールドグループで同じカスタムフィールドパスを参照しているかによって異なります。

同じ標準フィールドを参照する場合：

* その他の推奨値は、和集合に **追加** されます。
* 同じ列挙キーの推奨値に対して行われた更新は、和集合では **UPDATED** です。

異なるフィールドグループの同じカスタムフィールドパスを参照する場合：

* その他の推奨値は、和集合に **追加** されます。
* 複数のスキーマで同じ追加の推奨値が定義されている場合、それらの値は和集合内で **MERGED** になります。 つまり、結合後に同じ推奨値が 2 回表示されることはありません。

## 検証の制限

現在のシステム制限により、取り込み中にシステムによって列挙が検証されない場合が 2 つあります。

1. 列挙は、[ 配列フィールド ](./array.md) で定義されます。
1. 列挙がスキーマ階層の複数のレベルで定義されています。

## 次の手順

このガイドでは、UI で文字列フィールドの列挙と推奨値を定義する方法について説明しました。 スキーマレジストリ API を使用して列挙と推奨値を管理する方法について詳しくは、次の [ チュートリアル ](../../tutorials/suggested-values.md) を参照してください。

[!DNL Schema Editor] で他の XDM フィールドタイプを定義する方法については、[UI でのフィールドの定義 ](./overview.md#special) の概要を参照してください。
