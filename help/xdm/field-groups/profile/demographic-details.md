---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；スキーマデザイン；フィールドグループ；フィールドグループ；ユーザー；個人の詳細；プロファイルの詳細；個人；
solution: Experience Platform
title: 人口統計詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、人口統計の詳細スキーマフィールドグループの概要を説明します。
exl-id: 588c044c-b80d-4cb9-9f97-92f040d54bb4
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 22%

---


# [!UICONTROL 人口統計の] 詳細スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[ フィールドグループ名の更新 ](../name-updates.md) のドキュメントを参照してください。

[!UICONTROL 人口統] 計クラスの標準スキーマフィールドグル [[!DNL XDM Individual Profile] ープ](../../classes/individual-profile.md)。フィールドグループは、個人に関する情報を表すルートレベルの `person` オブジェクトを提供します。

![](../../images/field-groups/demographic-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `person.name` | [ユーザー名](../../data-types/person-name.md) | サブフィールドが人の名前の様々な要素を表すオブジェクト。 |
| `person.birthDate` | 日付 | ISO 8601 のタイムスタンプの形式で、人が生まれた完全な日付。 |
| `person.birthDayAndMonth` | 文字列 | 生年月日（月日）。MM-DD 形式で指定します。このフィールドは、生年月日の日と月がわかるが、年がわからない場合に使用します。 |
| `person.birthYear` | 整数 | 1989 年など、人が生まれた年。 このフィールドは、正式な生年月日ではなく、年齢のみがわかる場合に使用します。 |
| `person.gender` | 文字列 | 個人の性別 ID。 |
| `person.martialStatus` | 文字列 | 重要な相手との人の関係を表します。 |
| `person.nationality` | 文字列 | 個人とその州との法的関係は、ISO 3166-1 Alpha-2 コードを使用して表されます。 |
| `person.taxId` | 文字列 | 個人の税/会計 ID（米国では TIN、スペインでは CIF/NIF など）。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-person-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-person-details.schema.json)