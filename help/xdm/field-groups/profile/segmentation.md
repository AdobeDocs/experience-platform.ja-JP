---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；セグメント；segmentMembership；セグメントメンバーシップ；スキーマデザイン；マップ；マップ；
solution: Experience Platform
title: セグメントメンバーシップの詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「セグメントメンバーシップの詳細」スキーマフィールドグループの概要を説明します。
exl-id: 4d463f3a-2247-4307-8afe-9527e7fd72a7
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 3%

---


# [!UICONTROL Segment Membership Details] schema field group

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL Segment Membership Details] is a standard schema field group for the [[!DNL XDM Individual Profile] class](../../classes/individual-profile.md). フィールドグループは、セグメントのメンバーシップに関する情報（個人が属するセグメント、最終認定時間、メンバーシップが有効になるまで）を取得する単一のマップフィールドを提供します。

>[!WARNING]
>
>While the `segmentMembership` field must be manually added to your profile schema using this field group, you should not attempt to manually populate or update this field. セグメント化ジョブの実行時に、各プロファイルの`segmentMembership`マップが自動的に更新されます。

<img src="../../images/data-types/profile-segmentation.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `segmentMembership` | マップ | 個人のセグメントメンバーシップを表すmapオブジェクト。 このオブジェクトの構造について、以下で詳しく説明します。 |

{style=&quot;table-layout:auto&quot;}

次に、特定のプロファイルに対してシステムが入力した`segmentMembership`マップの例を示します。 セグメントのメンバーシップは、オブジェクトのルートレベルキーで示される名前空間で並べ替えられます。 次に、各名前空間の下の個々のキーは、プロファイルがメンバーとして属するセグメントのIDを表します。 各セグメントオブジェクトには、メンバーシップに関する詳細を提供するいくつかのサブフィールドが含まれます。

```json
{
  "xdm:segmentMembership": {
    "AAM": {
      "04a81716-43d6-4e7a-a49c-f1d8b3129ba9": {
        "xdm:version": "15",
        "xdm:lastQualificationTime": "2018-04-26T15:52:25+00:00",
        "xdm:validUntil": "2019-04-26T15:52:25+00:00",
        "xdm:status": "existing",
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
| `xdm:version` | The version of the segment that this profile qualified for. |
| `xdm:lastQualificationTime` | A timestamp of the last time this profile qualified for the segment. |
| `xdm:validUntil` | A timestamp of when the segment membership should no longer be assumed to be valid. |
| `xdm:status` | セグメントのメンバーシップが現在のリクエストの一部として認識されたかどうかを示します。 次の値を使用できます。 <ul><li>`existing`:プロファイルは、リクエストの前に既にセグメントに含まれていて、引き続きメンバーシップを維持します。</li><li>`realized`:プロファイルは、現在のリクエストの一部としてセグメントに入っています。</li><li>`exited`:プロファイルは、現在のリクエストの一部としてセグメントから退出しています。</li></ul> |
| `xdm:payload` | Some segment memberships include a payload that describes additional values directly related to the membership. Only one payload of a given type can be provided for each membership. `xdm:payloadType` ペイロードのタイプ(`boolean`、 `number`、 `propensity`または `string`)を示し、兄弟プロパティはペイロードタイプの値を提供します。 |

{style=&quot;table-layout:auto&quot;}

For more details on the field group, refer to the public XDM repository:

* [Populated example](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.schema.json)
