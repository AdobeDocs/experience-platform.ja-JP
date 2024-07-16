---
solution: Experience Platform
title: セグメントメンバーシップの詳細スキーマフィールドグループ
description: セグメントメンバーシップの詳細スキーマフィールドグループについて説明します。
exl-id: 4d463f3a-2247-4307-8afe-9527e7fd72a7
source-git-commit: de8e944cfec3b52d25bb02bcfebe57d6a2a35e39
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 20%

---


# [!UICONTROL  セグメントメンバーシップの詳細 ] スキーマフィールドグループ

>[!NOTE]
>
>複数のスキーマフィールドグループの名前が変更されました。 詳しくは、[フィールドグループ名の更新](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL  セグメントメンバーシップの詳細 ] は、[[!DNL XDM Individual Profile]  クラス ](../../classes/individual-profile.md) の標準スキーマフィールドグループです。 フィールドグループは、セグメントメンバーシップに関する情報（個人が属するセグメント、最終選定時間、メンバーシップの有効期限など）を取得する単一のマップフィールドを提供します。

>[!WARNING]
>
>`segmentMembership` フィールドは、このフィールドグループを使用してプロファイルスキーマに手動で追加する必要がありますが、このフィールドに手動で入力したり、更新したりしないでください。 セグメント化ジョブの実行時に、システムによって各プロファイルの `segmentMembership` マップが自動的に更新されます。

<img src="../../images/data-types/profile-segmentation.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `segmentMembership` | マップ | 個人のセグメントメンバーシップを記述するマップオブジェクト。このオブジェクトの構造については、以下で詳しく説明します。 |

{style="table-layout:auto"}

次に、特定のプロファイルに対してシステムが入力した `segmentMembership` マップの例を示します。 セグメントメンバーシップは、オブジェクトのルートレベルキーで示される名前空間で並べ替えられます。 次に、各名前空間の下の個々のキーは、プロファイルがメンバーとして属するセグメントの ID を表します。 各セグメントオブジェクトには、メンバーシップに関する詳細を提供する次のサブフィールドが含まれています。

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
| `xdm:version` | このプロファイルが選定したセグメントのバージョン。 |
| `xdm:lastQualificationTime` | 前回、このプロファイルがセグメントに選定された際のタイムスタンプ。 |
| `xdm:validUntil` | セグメントメンバーシップが有効であると見なされなくなった際のタイムスタンプ。 外部オーディエンスの場合、このフィールドが設定されていないと、セグメントメンバーシップは `lastQualificationTime` から 30 日間のみ保持されます。 |
| `xdm:status` | セグメントメンバーシップが現在のリクエストの一環として実現されたかどうかを示す文字列フィールド。以下の値を使用できます。 <ul><li>`realized`：プロファイルがセグメントに適合します。</li><li>`exited`：プロファイルは、現在のリクエストの一環としてセグメントから外れています。</li></ul> |
| `xdm:payload` | 一部のセグメントメンバーシップには、メンバーシップに直接関連する追加の値を記述するペイロードが含まれています。 各メンバーシップに指定できる特定のタイプのペイロードは 1 つだけです。 `xdm:payloadType` はペイロードのタイプ（`boolean`、`number`、`propensity` または `string`）を示し、その兄弟プロパティはペイロードタイプの値を提供します。 |

{style="table-layout:auto"}

>[!NOTE]
>
>`lastQualificationTime` に基づき、30 日を超えて「`exited`」ステータスにあるセグメントメンバーシップは、削除の対象となります。

フィールドグループについて詳しくは、公開 XDM リポジトリを参照してください。

* [ 入力された例 ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.example.1.json)
* [ 完全なスキーマ ](https://github.com/adobe/xdm/blob/master/components/fieldgroups/profile/profile-personal-details.schema.json)
