---
solution: Experience Platform
title: セグメントメンバーシップの詳細スキーマフィールドグループ
description: このドキュメントでは、「セグメントメンバーシップの詳細」スキーマフィールドグループの概要を説明します。
exl-id: 4d463f3a-2247-4307-8afe-9527e7fd72a7
source-git-commit: 8ae18565937adca3596d8663f9c9e6d84b0ce95a
workflow-type: tm+mt
source-wordcount: '419'
ht-degree: 19%

---


# [!UICONTROL セグメントメンバーシップの詳細] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL セグメントメンバーシップの詳細] は、 [[!DNL XDM Individual Profile] クラス](../../classes/individual-profile.md). フィールドグループは、セグメントメンバーシップに関する情報（個人が属するセグメント、最終認定時間、メンバーシップが有効になるまで）を取得する単一のマップフィールドを提供します。

>[!WARNING]
>
>また、 `segmentMembership` フィールドは、このフィールドグループを使用してプロファイルスキーマに手動で追加する必要があります。このフィールドに手動で入力または更新しないでください。 システムによって、 `segmentMembership` セグメント化ジョブの実行時に各プロファイルのマッピングをおこなう。

<img src="../../images/data-types/profile-segmentation.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `segmentMembership` | マップ | 個人のセグメントメンバーシップを記述するマップオブジェクト。このオブジェクトの構造について、以下で詳しく説明します。 |

{style="table-layout:auto"}

次に例を示します `segmentMembership` 特定のプロファイルに対してシステムが入力したマップ。 セグメントメンバーシップは、名前空間で並べ替えられます。名前空間は、オブジェクトのルートレベルのキーで示されます。 次に、各名前空間の下の個々のキーは、プロファイルがメンバーとして含まれるセグメントの ID を表します。 各セグメントオブジェクトには、メンバーシップに関する詳細を提供するいくつかのサブフィールドが含まれます。

```json
{
  "xdm:segmentMembership": {
    "AAM": {
      "04a81716-43d6-4e7a-a49c-f1d8b3129ba9": {
        "xdm:version": "15",
        "xdm:lastQualificationTime": "2018-04-26T15:52:25+00:00",
        "xdm:validUntil": "2019-04-26T15:52:25+00:00",
        "xdm:status": "realized",
        "xdm:payload": {
          "xdm:payloadBooleanValue": true,
          "xdm:payloadType": "boolean"
        }
      },
      "53cba6b2-a23b-454a-8069-fc41308f1c0f": {
        "xdm:version": "3",
        "xdm:lastQualificationTime": "2018-04-26T15:52:25+00:00",
        "xdm:validUntil": "2018-04-27T15:52:25+00:00",
        "xdm:status": "realized",
        "xdm:payload": {
          "xdm:payloadPropensityValue": 0.5,
          "xdm:payloadType": "propensity"
        }
      }
    },
    "Email": {
      "abcd@adobe.com": {
        "xdm:version": "1",
        "xdm:lastQualificationTime": "2017-09-26T15:52:25+00:00",
        "xdm:validUntil": "2017-12-26T15:52:25+00:00",
        "xdm:status": "exited"
      }
    }
  }
}
```

| プロパティ | 説明 |
| --- | --- |
| `xdm:version` | このプロファイルが適合するセグメントのバージョン。 |
| `xdm:lastQualificationTime` | 前回、このプロファイルがセグメントに選定された際のタイムスタンプ。 |
| `xdm:validUntil` | セグメントメンバーシップが有効であると見なされなくなったときのタイムスタンプ。 外部オーディエンスの場合、このフィールドを設定しないと、セグメントのメンバーシップは、 `lastQualificationTime`. |
| `xdm:status` | セグメントメンバーシップが現在のリクエストの一環として実現されたかどうかを示す文字列フィールド。以下の値を使用できます。 <ul><li>`realized`:プロファイルがセグメントの対象になります。</li><li>`exited`：プロファイルは、現在のリクエストの一環としてセグメントから外れています。</li></ul> |
| `xdm:payload` | 一部のセグメントメンバーシップには、メンバーシップに直接関連する追加の値を記述するペイロードが含まれています。 各メンバーシップに指定できるペイロードは、1 つのタイプのみです。 `xdm:payloadType` ペイロードのタイプを示します (`boolean`, `number`, `propensity`または `string`) の代わりに、兄弟プロパティがペイロードタイプの値を提供します。 |

{style="table-layout:auto"}

>[!NOTE]
>
>次に属する任意のセグメントメンバーシップ `exited` 30 日を超えるステータス ( `lastQualificationTime`、は削除される可能性があります。

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力された例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.schema.json)
