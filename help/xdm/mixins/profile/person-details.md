---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;individual profile;fields;schemas;Schemas;Schema design;mixin;mixin;person;person details;profile person details;person;
solution: Experience Platform
title: 人口統計詳細ミックスイン
topic: overview
description: このドキュメントでは、人口統計の詳細ミックスインの概要を説明します。
translation-type: tm+mt
source-git-commit: f9d8021643e72e3fbb5315b54a19815dcdaaa702
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 24%

---


# [!UICONTROL 人口統計詳細] ミックスイン

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、 [mixin名の更新に関するドキュメントを参照してください](../name-updates.md) 。

[!UICONTROL 「人口統計詳細] 」は [[!DNL XDM Individual Profile] クラスの標準ミックスインです](../../classes/individual-profile.md)。 ミックスインは、ルートレベルの `person` オブジェクトを提供します。このオブジェクトのサブフィールドは、個々の人に関する情報を記述します。

<img src="../../images/mixins/profile-person-details.png" width="600" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `person.name` | [個人名](../../data-types/person-name.md) | サブフィールドに、人の名前の様々な要素が記述されるオブジェクト。 |
| `person.birthDate` | Date | ISO 8601のタイムスタンプの形式で、人が生まれた完全な日付。 |
| `person.birthDayAndMonth` | 文字列 | 生年月日（月日）。MM-DD 形式で指定します。このフィールドは、生年月日の日と月がわかるが、年がわからない場合に使用します。 |
| `person.birthYear` | 整数 | 1989年など、人が生まれた年。 このフィールドは、正式な生年月日ではなく、年齢のみがわかる場合に使用します。 |
| `person.gender` | 文字列 | 個人の性別ID。 |
| `person.martialStatus` | 文字列 | 重要な人との関係を表します。 |
| `person.nationality` | 文字列 | ISO 3166-1 Alpha-2コードを使用して表される、個人とその州との法的関係。 |
| `person.taxId` | 文字列 | 米国のTINやスペインのCIF/NIFなど、個人の税/会計ID。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-person-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-person-details.schema.json)オングストローム