---
keywords: Experience Platform;home;popular topics;schema;Schema;XDM;individual profile;fields;schemas;Schemas;Schema design;field group;field group;person;person details;profile person details;person;
solution: Experience Platform
title: 人口統計詳細スキーマフィールドグループ
topic-legacy: overview
description: This document provides an overview of the Demographic Details schema field group.
exl-id: 588c044c-b80d-4cb9-9f97-92f040d54bb4
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 22%

---


# [!UICONTROL 人口統計詳] 細スキーマフィールドグループ

>[!NOTE]
>
>The names of several schema field groups have changed. 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL Demographic Details] is a standard schema field group for the [[!DNL XDM Individual Profile] class](../../classes/individual-profile.md). フィールドグループは、個人に関する情報を表すルートレベルの`person`オブジェクトを提供します。

![](../../images/field-groups/demographic-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `person.name` | [ユーザー名](../../data-types/person-name.md) | An object whose sub-fields describe various elements of a person&#39;s name. |
| `person.birthDate` | 日付 | The full date a person was born on, in the form of an ISO 8601 timestamp. |
| `person.birthDayAndMonth` | 文字列 | 生年月日（月日）。MM-DD 形式で指定します。このフィールドは、生年月日の日と月がわかるが、年がわからない場合に使用します。 |
| `person.birthYear` | 整数 | The year a person was born, including the century (such as 1989). このフィールドは、正式な生年月日ではなく、年齢のみがわかる場合に使用します。 |
| `person.gender` | 文字列 | The gender identity of the person. |
| `person.martialStatus` | 文字列 | 重要な人との人の関係を表します。 |
| `person.nationality` | 文字列 | ISO 3166-1 Alpha-2コードを使用して表される、個人とその州との法的関係。 |
| `person.taxId` | 文字列 | The tax/fiscal ID of the person, such the TIN in the US or the CIF/NIF in Spain. |

{style=&quot;table-layout:auto&quot;}

For more details on the field group, refer to the public XDM repository:

* [Populated example](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-person-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-person-details.schema.json)