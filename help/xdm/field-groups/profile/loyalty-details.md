---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；ロイヤルティの詳細；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: 「ロイヤルティ詳細」スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「ロイヤルティ詳細」スキーマフィールドグループの概要を説明します。
exl-id: 12c9fef5-4f9e-49b5-894f-f4938bb95c23
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 5%

---

# [!UICONTROL Loyalty Detailsschema フィ] ールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[ フィールドグループ名の更新 ](../name-updates.md) のドキュメントを参照してください。

[!UICONTROL ロイヤリ] ティ [[!DNL XDM Individual Profile] クラスの標準スキーマフィールドグル](../../classes/individual-profile.md)ープの詳細。フィールドグループには、顧客ロイヤルティプログラムでの個人のメンバーシップに関する情報を取り込む、単一のオブジェクトタイプフィールド `loyalty` が用意されています。

![](../../images/field-groups/loyalty-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `pointsExpiration` | オブジェクトの配列 | 期限切れになるロイヤルティポイント（またはロイヤルティポイントのグループ）と、期限切れになる日付をリストします。 各配列項目は、次の 2 つのプロパティを含むオブジェクトである必要があります。 <ul><li>`pointsExpirationDate`:ポイントが期限切れになる ISO 8601 の datetime。</li><li>`pointsExpiring`:関連付けられた有効期限の時点で期限切れになるポイント残高。</li></ul> |
| `joinDate` | DateTime | ISO 8601 の datetime です。この日時は、ユーザーがロイヤルティプログラムに参加した日時です。 |
| `loyaltyID` | 文字列の配列 | ロイヤルティプログラムメンバーに関連付けられたロイヤルティプログラム ID を表します。 |
| `points` | Double | ロイヤルティメンバーのロイヤルティポイントまたは賞の現在の残高。 |
| `pointsRedeemed` | ダブル | ロイヤルティメンバーが購入に適用したポイントまたは引き換えたポイントの数。 |
| `program` | 文字列 | 個人が登録されているロイヤルティプログラムの名前。 |
| `status` | 文字列 | `active`、`disabled`、`suspended` など、個人のロイヤルティメンバーシップの現在のステータス。 |
| `tier` | 文字列 | 個人が登録されているロイヤルティプログラム層を取り込みます。 |
| `upgradeDate` | 文字列 | ロイヤルティメンバーが最新の階層レベルにアップグレードされた日付。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-loyalty-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-loyalty-details.schema.json)
