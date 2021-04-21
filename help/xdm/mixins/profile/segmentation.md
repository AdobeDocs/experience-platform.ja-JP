---
keywords: Experience Platform；ホーム；人気のあるトピック；スキーマ;スキーマ;XDM；個々のプロファイル；フィールド；スキーマ;スキーマ；セグメント；メンバーシップ；セグメントのメンバーシップ；スキーマデザイン；マップ；
solution: Experience Platform
title: セグメントメンバーシップの詳細Mixin
topic-legacy: overview
description: このドキュメントでは、セグメントメンバーシップの詳細ミックスインの概要を説明します。
exl-id: 4d463f3a-2247-4307-8afe-9527e7fd72a7
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '406'
ht-degree: 1%

---

# [!UICONTROL セグメントのメンバーシップ] の詳細Mixin

>[!NOTE]
>
>いくつかのミックスインの名前が変更されました。 詳しくは、[mixin name updates](../name-updates.md)のドキュメントを参照してください。

[!UICONTROL セグメントのメンバーシップ] の詳細は、 [[!DNL XDM Individual Profile] クラスの標準のmixinです](../../classes/individual-profile.md)。mixinは、個人が属するセグメント、最後の資格日時、およびメンバーシップが有効になるまでの期間など、セグメントのメンバーシップに関する情報をキャプチャする単一のマップフィールドを提供します。

>[!WARNING]
>
>`segmentMembership`フィールドは、このミックスインを使用してプロファイルスキーマに手動で追加する必要がありますが、このフィールドの入力や更新を手動で行うことは避けてください。 セグメント化ジョブの実行時に、各プロファイルの`segmentMembership`マップが自動的に更新されます。

<img src="../../images/data-types/profile-segmentation.png" width="400" /><br />

| プロパティ | データタイプ | 説明 |
| --- | --- | --- |
| `segmentMembership` | マップ | 個人のセグメントのメンバーシップを表すmapオブジェクト。 このオブジェクトの構造については、以下で詳しく説明します。 |

次の例は、特定のプロファイルに対してシステムが入力した`segmentMembership`マップの例です。 セグメントのメンバーシップは、オブジェクトのルートレベルキーで示される名前空間で並べ替えられます。 次に、各名前空間の個々のキーは、プロファイルが属するセグメントのIDを表します。 各セグメントオブジェクトには、メンバーシップに関する詳細を提供する次のような複数のサブフィールドが含まれています。

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
| `xdm:version` | このプロファイルが資格を持つセグメントのバージョン。 |
| `xdm:lastQualificationTime` | このプロファイルが最後にセグメントに適用された時間のタイムスタンプ。 |
| `xdm:validUntil` | セグメントのメンバーシップが有効であると見なされなくなったときのタイムスタンプ。 |
| `xdm:status` | セグメントのメンバーシップが現在のリクエストの一部として実現されたかどうかを示します。 次の値を指定できます。 <ul><li>`existing`:プロファイルは、リクエストの前に既にセグメントの一部であり、引き続きメンバーシップを維持します。</li><li>`realized`:プロファイルは、現在のリクエストの一部としてセグメントを入力しています。</li><li>`exited`:プロファイルは、現在のリクエストの一部としてセグメントを終了しています。</li></ul> |
| `xdm:payload` | 一部のセグメントメンバーシップには、メンバーシップに直接関連する追加の値を記述するペイロードが含まれます。 メンバーシップごとに1つの型のペイロードのみを指定できます。 `xdm:payloadType` ペイロード(、`boolean`、 `number`、または `propensity` `string`)のタイプを示し、その兄弟プロパティはペイロードタイプの値を提供します。 |

Mixinの詳細については、パブリックXDMリポジトリを参照してください。

* [入力済みの例](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-personal-details.example.1.json)
* [フルスキーマ](https://github.com/adobe/xdm/blob/master/components/mixins/profile/profile-personal-details.schema.json)
