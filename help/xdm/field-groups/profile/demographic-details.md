---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；個人；人物の詳細；プロファイル人物の詳細；人物；
solution: Experience Platform
title: 人口統計詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、人口統計の詳細スキーマフィールドグループの概要を説明します。
exl-id: 588c044c-b80d-4cb9-9f97-92f040d54bb4
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 26%

---


# [!UICONTROL 人口統計の詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL 人口統計の詳細] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md). フィールドグループは、ルートレベルを提供します `person` 個人に関する情報を表すサブフィールドを持つオブジェクト。

![](../../images/field-groups/demographic-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `person.name` | [氏名](../../data-types/person-name.md) | サブフィールドが個人の名前の様々な要素を表すオブジェクト。 |
| `person.birthDate` | 日付 | ISO 8601 タイムスタンプの形式で、人が生まれた完全な日付。 |
| `person.birthDayAndMonth` | 文字列 | 生年月日（月日）。MM-DD 形式で指定します。このフィールドは、生年月日の日と月がわかるが、年がわからない場合に使用します。 |
| `person.birthYear` | 整数 | 世紀（1989 年など）を含む、人が生まれた年。 このフィールドは、正式な生年月日ではなく、年齢のみがわかる場合に使用します。 |
| `person.gender` | 文字列 | 人物の性別アイデンティティ。 |
| `person.martialStatus` | 文字列 | 人物と重要な他者との関係を表します。 |
| `person.nationality` | 文字列 | ISO 3166-1 Alpha-2 コードを使用して表される、個人とその国との法的関係。 |
| `person.taxId` | 文字列 | 人物の税/会計 ID （米国の TIN やスペインの CIF/NIF など）。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-person-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-person-details.schema.json)