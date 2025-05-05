---
title: ヘルスケア薬品スキーマフィールドグループ
description: ヘルスケア薬品スキーマフィールドグループについて説明します。
exl-id: 3423d067-fe8c-44e5-a6f9-ce0458d26ebc
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 7%

---

# [!UICONTROL &#x200B; ヘルスケア薬品 &#x200B;] スキーマフィールドグループ

[!UICONTROL &#x200B; ヘルスケア薬品 &#x200B;] は、[[!UICONTROL &#x200B; 投薬 &#x200B;] クラス ](../../classes/medication.md) の標準スキーマフィールドグループです。 ブランド名、ロット番号、数量などの詳細を収集する単一のオブジェクトタイプのフィールド `medication` を提供します。

![](../../images/field-groups/healthcare-medication.png)

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `ingredients` | オブジェクトの配列 | 薬に存在する成分を一覧表示します。 各オブジェクトには、次のプロパティが含まれます。 <ul><li>`isActive`: （ブール値）この成分がこの薬でまだアクティブに使用されているかどうかを示します。</li><li>`name`: （文字列）原料の名前。</li><li>`quantity`: （文字列）薬に含まれる成分の量。</li></ul> |
| `brandName` | 文字列 | 薬のブランド名。 |
| `codes` | 文字列の配列 | この薬を識別するコードのリスト。 |
| `dosageUnitNumber` | Double | 薬の投与単位番号。 |
| `dosageUnitOfMeasurement` | 文字列 | 投与数の測定単位。 |
| `expiryDate` | 日時 | 投薬の有効期限。 |
| `genericName` | 文字列 | 薬の一般名。 |
| `lotNumber` | 文字列 | 薬のバッチの一意の ID。 |
| `manufacturerName` | 文字列 | 製薬会社の名前。 |
| `quantity` | Double | パッケージに含まれる薬物の量。 |
| `status` | 文字列 | 薬物/薬が有効かどうかを示す一般的なステータス。 |
| `volume` | Double | 薬の分量。 |

{style="table-layout:auto"}

フィールドグループについて詳しくは、[ 公開 XDM リポジトリ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/medication/healthcare-medication.schema.json) を参照してください。
