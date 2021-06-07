---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；ロイヤルティの詳細；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: Loyalty Detailsスキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「ロイヤルティ詳細」スキーマフィールドグループの概要を説明します。
source-git-commit: fe49560a69c4c02835f00e4ebc0a5b9dc88eae90
workflow-type: tm+mt
source-wordcount: '305'
ht-degree: 5%

---


# [!UICONTROL Loyalty Detailsschemaフィ] ールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL ロイヤリ] ティ [[!DNL XDM Individual Profile] クラスの標準スキーマフィールドグル](../../classes/individual-profile.md)ープの詳細。フィールドグループには、顧客ロイヤルティプログラムでの個人のメンバーシップに関する情報を取り込む、1つのオブジェクトタイプフィールド`loyalty`が用意されています。

![](../../images/field-groups/loyalty-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `pointsExpiration` | オブジェクトの配列 | 期限切れになるロイヤルティポイント（またはロイヤルティポイントのグループ）と、期限切れになる日付をリストします。 各配列項目は、次の2つのプロパティを含むオブジェクトである必要があります。 <ul><li>`pointsExpirationDate`:ポイントが期限切れになるISO 8601のdatetime。</li><li>`pointsExpiring`:関連付けられた有効期限時点で期限切れになるポイント残高。</li></ul> |
| `joinDate` | DateTime | ユーザーがロイヤルティプログラムに参加した日時のISO 8601 datetime。 |
| `loyaltyID` | 文字列の配列 | ロイヤルティプログラムメンバーに関連付けられたロイヤルティプログラムIDを表します。 |
| `points` | Double | ロイヤルティメンバーのロイヤルティポイントまたは賞の現在の残高。 |
| `pointsRedeemed` | ダブル | ロイヤルティメンバーが購入に対して適用したポイントまたは引き換えたポイントの数。 |
| `program` | 文字列 | 個人が登録されているロイヤルティプログラムの名前。 |
| `status` | 文字列 | `active`、`disabled`、`suspended`など、個人のロイヤルティメンバーシップの現在のステータス。 |
| `tier` | 文字列 | 個人が登録されているロイヤルティプログラム層を取り込みます。 |
| `upgradeDate` | 文字列 | ロイヤルティメンバーが最新の層レベルにアップグレードされた日付。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-loyalty-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-loyalty-details.schema.json)
