---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；個々のプロファイル；フィールド；スキーマ;スキーマ;スキーマ設計；フィールドグループ；個人；個人の詳細；プロファイルの個人の詳細；個人；
solution: Experience Platform
title: 人口統計詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「人口統計詳細スキーマ」フィールドグループの概要を説明します。
exl-id: 588c044c-b80d-4cb9-9f97-92f040d54bb4
translation-type: tm+mt
source-git-commit: f5beb57acf4cc1827bf08b549cd2b3a02fd82b18
workflow-type: tm+mt
source-wordcount: '279'
ht-degree: 21%

---


# [!UICONTROL 人口統計] 詳細スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL 人口統計] 詳細は、その [[!DNL XDM Individual Profile] クラスの標準スキーマフィールドグループ](../../classes/individual-profile.md)。フィールドグループは、ルートレベルの`person`オブジェクトを提供します。このオブジェクトのサブフィールドは、個々の人に関する情報を示します。

![](../../images/field-groups/demographic-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `person.name` | [個人名](../../data-types/person-name.md) | サブフィールドに、人の名前の様々な要素が記述されるオブジェクト。 |
| `person.birthDate` | 日付 | ISO 8601のタイムスタンプの形式で、人が生まれた完全な日付。 |
| `person.birthDayAndMonth` | 文字列 | 生年月日（月日）。MM-DD 形式で指定します。このフィールドは、生年月日の日と月がわかるが、年がわからない場合に使用します。 |
| `person.birthYear` | 整数 | 1989年など、人が生まれた年。 このフィールドは、正式な生年月日ではなく、年齢のみがわかる場合に使用します。 |
| `person.gender` | 文字列 | 個人の性別ID。 |
| `person.martialStatus` | 文字列 | 重要な人との関係を表します。 |
| `person.nationality` | 文字列 | ISO 3166-1 Alpha-2コードを使用して表される、個人とその州との法的関係。 |
| `person.taxId` | 文字列 | 米国のTINやスペインのCIF/NIFなど、個人の税/会計ID。 |

フィールドグループの詳細については、public XDM repositoryを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-person-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-person-details.schema.json)