---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；ロイヤルティの詳細；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: ロイヤルティ詳細スキーマフィールドグループ
description: 「ロイヤルティ詳細」スキーマフィールドグループの詳細を説明します。
exl-id: 12c9fef5-4f9e-49b5-894f-f4938bb95c23
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 12%

---

# [!UICONTROL ロイヤルティの詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL ロイヤルティの詳細] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md). フィールドグループには、単一のオブジェクトタイプのフィールドが用意されています。 `loyalty`：顧客ロイヤリティプログラムの個人のメンバーシップに関する情報を取り込みます。

![](../../images/field-groups/loyalty-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `pointsExpiration` | オブジェクトの配列 | 期限切れになるロイヤルティポイント（またはロイヤルティポイントのグループ）と、期限切れになる日付が表示されます。 各配列項目は、次の 2 つのプロパティを含むオブジェクトである必要があります。 <ul><li>`pointsExpirationDate`：ポイントが期限切れになる ISO 8601 の datetime。</li><li>`pointsExpiring`：関連する有効期限時に期限が切れるポイント残高。</li></ul> |
| `joinDate` | 日時 | 人物がロイヤルティプログラムに参加した日時の ISO 8601 datetime。 |
| `loyaltyID` | 文字列の配列 | ロイヤルティプログラムメンバーに関連付けられたロイヤルティプログラム ID を表します。 |
| `points` | Double | ロイヤルティメンバーのロイヤルティポイントまたは賞の現在の残高。 |
| `pointsRedeemed` | Double | ロイヤルティメンバーが購入に対して適用した、または引き換えたポイントの数。 |
| `program` | 文字列 | 人物が登録されているロイヤルティプログラムの名前。 |
| `status` | 文字列 | 人物のロイヤルティメンバーシップの現在のステータス（例： ） `active`, `disabled`または `suspended`. |
| `tier` | 文字列 | 人物が登録されているロイヤルティプログラム層をキャプチャします。 |
| `upgradeDate` | 文字列 | ロイヤルティメンバーが最新の階層レベルにアップグレードされた日付。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-loyalty-details.example.1.json)
* [完全なスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-loyalty-details.schema.json)
