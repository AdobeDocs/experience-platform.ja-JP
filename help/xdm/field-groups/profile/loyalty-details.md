---
keywords: Experience Platform；ホーム；人気のトピック；スキーマ；XDM；個人プロファイル；フィールド；スキーマ；スキーマ；ロイヤルティの詳細；スキーマデザイン；フィールドグループ；フィールドグループ；
solution: Experience Platform
title: ロイヤルティの詳細スキーマフィールドグループ
description: ロイヤルティの詳細スキーマフィールドグループについて説明します。
exl-id: 12c9fef5-4f9e-49b5-894f-f4938bb95c23
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 11%

---

# [!UICONTROL &#x200B; ロイヤルティの詳細 &#x200B;] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL &#x200B; ロイヤルティの詳細 &#x200B;] は、[[!DNL XDM Individual Profile]  クラス ](../../classes/individual-profile.md) の標準スキーマフィールドグループです。 フィールドグループは、単一のオブジェクトタイプのフィールド `loyalty` を提供し、顧客ロイヤルティプログラムにおける個人のメンバーシップに関連する情報を取得します。

![](../../images/field-groups/loyalty-details.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `pointsExpiration` | オブジェクトの配列 | 有効期限が切れるロイヤルティポイント（またはロイヤルティポイントのグループ）と、有効期限が切れる日付をリストします。 各配列項目は、次の 2 つのプロパティを含むオブジェクトである必要があります。 <ul><li>`pointsExpirationDate`：ポイントの有効期限が切れる ISO 8601 日時。</li><li>`pointsExpiring`：関連する満了日時点で満期になるポイント残高。</li></ul> |
| `joinDate` | 日時 | ユーザーがロイヤルティプログラムに参加した日時の ISO 8601。 |
| `loyaltyID` | 文字列の配列 | ロイヤルティプログラムメンバーに関連付けられたロイヤルティプログラム ID を表します。 |
| `points` | Double | ロイヤルティメンバーのロイヤルティポイントまたは特典の現在の残高。 |
| `pointsRedeemed` | Double | ロイヤルティメンバーが購入に対して適用した、または交換したポイント数。 |
| `program` | 文字列 | ユーザーが登録されているロイヤルティプログラムの名前。 |
| `status` | 文字列 | 人物のロイヤルティメンバーシップの現在のステータス（`active`、`disabled`、`suspended` など）。 |
| `tier` | 文字列 | 人物が登録されているロイヤルティプログラム層をキャプチャします。 |
| `upgradeDate` | 文字列 | ロイヤルティメンバーが最新の層レベルにアップグレードされた日付。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-loyalty-details.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-loyalty-details.schema.json)
