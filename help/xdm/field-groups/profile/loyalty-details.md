---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；ロイヤルティの詳細；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: Loyalty Detailsスキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「ロイヤルティ詳細」スキーマフィールドグループの概要を説明します。
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 5%

---


# [!UICONTROL Loyalty Details] schema field group

>[!NOTE]
>
>The names of several schema field groups have changed. 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL Loyalty Details] is a standard schema field group for the [[!DNL XDM Individual Profile] class](../../classes/individual-profile.md). フィールドグループには、顧客ロイヤルティプログラムでの個人のメンバーシップに関する情報を取り込む、1つのオブジェクトタイプフィールド`loyalty`が用意されています。

![](../../images/field-groups/loyalty-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `pointsExpiration` | Array of objects | 期限切れになるロイヤルティポイント（またはロイヤルティポイントのグループ）と、期限切れになる日付をリストします。 Each array item must be an object that contains the following two properties: <ul><li>`pointsExpirationDate`:ポイントが期限切れになるISO 8601のdatetime。</li><li>`pointsExpiring`:関連付けられた有効期限時点で期限切れになるポイント残高。</li></ul> |
| `joinDate` | DateTime | ユーザーがロイヤルティプログラムに参加した日時のISO 8601 datetime。 |
| `loyaltyID` | 文字列の配列 | ロイヤルティプログラムメンバーに関連付けられたロイヤルティプログラムIDを表します。 |
| `points` | Double | ロイヤルティメンバーのロイヤルティポイントまたは賞の現在の残高。 |
| `pointsRedeemed` | Double | ロイヤルティメンバーが購入に対して適用したポイントまたは引き換えたポイントの数。 |
| `program` | 文字列 | The name of the loyalty program in which the person is enrolled. |
| `status` | 文字列 | `active`、`disabled`、`suspended`など、個人のロイヤルティメンバーシップの現在のステータス。 |
| `tier` | 文字列 | 個人が登録されているロイヤルティプログラム層を取り込みます。 |
| `upgradeDate` | 文字列 | The date when the loyalty member was upgraded to their most recent tier level. |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [Populated example](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-loyalty-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-loyalty-details.schema.json)
