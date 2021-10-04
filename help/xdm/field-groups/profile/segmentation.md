---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ；スキーマ；XDM；個々のプロファイル；フィールド；スキーマ；スキーマ；スキーマ；セグメント；segmentMembership；セグメントメンバーシップ；スキーマデザイン；マップ；マップ；
solution: Experience Platform
title: セグメントメンバーシップの詳細スキーマフィールドグループ
topic-legacy: overview
description: このドキュメントでは、「セグメントメンバーシップの詳細」スキーマフィールドグループの概要を説明します。
exl-id: 4d463f3a-2247-4307-8afe-9527e7fd72a7
source-git-commit: afe748d443aad7b6da5b348cd569c9e806e4419b
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 4%

---


# [!UICONTROL Segment Membership ] Detailsschema フィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[ フィールドグループ名の更新 ](../name-updates.md) のドキュメントを参照してください。

[!UICONTROL セグメントのメ] ンバーシップ [[!DNL XDM Individual Profile] クラスの標準スキーマフィールドグ](../../classes/individual-profile.md)ループの詳細。フィールドグループは、セグメントのメンバーシップに関する情報（個人が属するセグメント、最終認定時間、メンバーシップが有効になるまでなど）を取り込む単一のマップフィールドを提供します。

>[!WARNING]
>
>`segmentMembership` フィールドは、このフィールドグループを使用してプロファイルスキーマに手動で追加する必要がありますが、このフィールドに手動で値を入力または更新しないでください。 セグメント化ジョブが実行されると、各プロファイルの `segmentMembership` マップが自動的に更新されます。

<img src="../../images/data-types/profile-segmentation.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `segmentMembership` | マップ | 個人のセグメントメンバーシップを表す map オブジェクト。 このオブジェクトの構造について、以下で詳しく説明します。 |

{style=&quot;table-layout:auto&quot;}

次の例は、特定のプロファイルに対してシステムが入力した `segmentMembership` マップです。 セグメントのメンバーシップは、オブジェクトのルートレベルのキーで示される名前空間で並べ替えられます。 次に、各名前空間の下の個々のキーは、プロファイルがメンバーとして含まれるセグメントの ID を表します。 各セグメントオブジェクトには、メンバーシップに関する詳細を提供する複数のサブフィールドが含まれます。

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
| `xdm:version` | このプロファイルが適合するセグメントのバージョン。 |
| `xdm:lastQualificationTime` | このプロファイルが最後にセグメントに適合した時刻のタイムスタンプ。 |
| `xdm:validUntil` | セグメントのメンバーシップが有効と見なされなくなった日時のタイムスタンプ。 |
| `xdm:status` | 現在のリクエストの一部としてセグメントのメンバーシップが認識されたかどうかを示します。 次の値を使用できます。 <ul><li>`existing`:プロファイルは、リクエストの前に既にセグメントの一部であり、引き続きメンバーシップを維持します。</li><li>`realized`:プロファイルは、現在のリクエストの一部としてセグメントに入っています。</li><li>`exited`:プロファイルは、現在のリクエストの一部としてセグメントから退出しています。</li></ul> |
| `xdm:payload` | 一部のセグメントメンバーシップには、メンバーシップに直接関連する追加の値を記述するペイロードが含まれています。 各メンバーシップに指定できるペイロードは、特定の型の 1 つだけです。 `xdm:payloadType` ペイロードのタイプ (`boolean`、 `number`、 `propensity`または `string`) を示し、兄弟プロパティはペイロードタイプの値を提供します。 |

{style=&quot;table-layout:auto&quot;}

フィールドグループについて詳しくは、パブリック XDM リポジトリを参照してください。

* [入力例](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.schema.json)
