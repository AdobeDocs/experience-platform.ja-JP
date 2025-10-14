---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；個人プロファイル；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；ユーザー；ユーザーの詳細；プロファイルのユーザーの詳細；ユーザー；
solution: Experience Platform
title: デモグラフィックの詳細スキーマフィールドグループ
description: デモグラフィックの詳細スキーマフィールドグループについて説明します。
exl-id: 588c044c-b80d-4cb9-9f97-92f040d54bb4
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '261'
ht-degree: 40%

---


# [!UICONTROL &#x200B; 人口統計の詳細 &#x200B;] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL &#x200B; デモグラフィックの詳細 &#x200B;] は、[[!DNL XDM Individual Profile]  クラス &#x200B;](../../classes/individual-profile.md) の標準スキーマフィールドグループです。 フィールドグループは、ルートレベルの `person` オブジェクトを提供し、そのサブフィールドは個人に関する情報を記述します。

![](../../images/field-groups/demographic-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `person.name` | [&#x200B; 人物名 &#x200B;](../../data-types/person-name.md) | サブフィールドが人物の名前の様々な要素を記述するオブジェクト。 |
| `person.birthDate` | 日付 | 人物の誕生の完全な日付（ISO 8601 タイムスタンプの形式）。 |
| `person.birthDayAndMonth` | 文字列 | 生年月日（月日）。MM-DD 形式で指定します。このフィールドは、生年月日の日と月がわかるが、年がわからない場合に使用します。 |
| `person.birthYear` | 整数 | 世紀を含む、個人が生まれた年（1989 年など）。 このフィールドは、正式な生年月日ではなく、年齢のみがわかる場合に使用します。 |
| `person.gender` | 文字列 | 人物の性自認。 |
| `person.martialStatus` | 文字列 | 人物と重要な他者との関係を表します。 |
| `person.nationality` | 文字列 | ISO 3166-1 Alpha-2 コードを使用して表される、個人とその国との間の法的関係。 |
| `person.taxId` | 文字列 | 個人の税金/会計 ID （米国の TIN やスペインのCIF/NIF など）。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [&#x200B; 入力された例 &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-person-details.example.1.json)
* [&#x200B; 完全なスキーマ &#x200B;](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-person-details.schema.json)