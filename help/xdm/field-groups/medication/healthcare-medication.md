---
title: ヘルスケア薬スキーマフィールドグループ
description: このドキュメントでは、ヘルスケアの薬のスキーマフィールドグループの概要を説明します。
exl-id: 3423d067-fe8c-44e5-a6f9-ce0458d26ebc
source-git-commit: 2fd35c4ac29f43391f9dc03c636d20558b701be7
workflow-type: tm+mt
source-wordcount: '213'
ht-degree: 14%

---

# [!UICONTROL 医療薬] スキーマフィールドグループ

[!UICONTROL 医療薬] は、 [[!UICONTROL 薬] クラス](../../classes/medication.md). 単一のオブジェクトタイプフィールドを提供します。 `medication` ブランド名、ロット番号、数量などの詳細を取り込む

![](../../images/field-groups/healthcare-medication.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `ingredients` | オブジェクトの配列 | 薬の中に存在する成分のリストを表示します。 各オブジェクトには、次のプロパティが含まれます。 <ul><li>`isActive`:(Boolean) この成分がこの薬剤でまだ積極的に使用されているかどうかを示します。</li><li>`name`:(String) 原料の名前。</li><li>`quantity`:（文字列）薬剤に含まれる成分の量。</li></ul> |
| `brandName` | 文字列 | 薬物のブランド名。 |
| `codes` | 文字列の配列 | この薬を識別するコードのリスト。 |
| `dosageUnitNumber` | Double | 薬剤の投与単位番号。 |
| `dosageUnitOfMeasurement` | 文字列 | 剤数の測定単位。 |
| `expiryDate` | 日時 | 薬の有効期限。 |
| `genericName` | 文字列 | 薬物の総称。 |
| `lotNumber` | 文字列 | 薬物のバッチの一意の識別子。 |
| `manufacturerName` | 文字列 | 薬の製造元の名前。 |
| `quantity` | ダブル | パッケージ内の薬の量。 |
| `status` | 文字列 | 薬物/薬物が有効かどうかを示す一般的なステータス。 |
| `volume` | ダブル | 薬の量。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、 [パブリック XDM リポジトリ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/medication/healthcare-medication.schema.json).
