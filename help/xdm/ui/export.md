---
solution: Experience Platform
title: UIでのXDM スキーマの書き出し
description: Adobe Experience Platform ユーザーインターフェイスで、既存のスキーマを別のサンドボックスまたは組織に書き出す方法について説明します。
type: Tutorial
exl-id: c467666d-55bc-4134-b8f4-7758d49c4786
source-git-commit: 67ae12b0a410d50c25f4e044b8430b70249670eb
workflow-type: tm+mt
source-wordcount: '690'
ht-degree: 10%

---

# UI での XDM スキーマの書き出し {#export-xdm-schemas-in-the-UI}

>[!CONTEXTUALHELP]
>id="platform_xdm_copyjsonstructure"
>title="JSON 構造をコピー"
>abstract="JSON 構造をクリップボードにコピーして、選択したスキーマの書き出しペイロードを生成します。この機能を使用して、スキーマライブラリ内の任意のスキーマの詳細を書き出します。この書き出した JSON を使用して、スキーマと関連リソースを別のサンドボックスまたは組織に読み込むことができます。これにより、異なる環境間でのスキーマの共有と再利用が簡単で効率的になります。"

スキーマライブラリ内のすべてのリソースは、組織内の特定のサンドボックスに含まれています。 場合によっては、サンドボックスと組織間でExperience Data Model （XDM）リソースを共有したい場合があります。

このニーズに対応するために、Adobe Experience Platform UIの[!UICONTROL Schemas] ワークスペースでは、スキーマライブラリ内の任意のスキーマのエクスポートペイロードを生成できます。 このペイロードは、Schema Registry APIの呼び出しで使用して、スキーマ（およびすべての依存リソース）をターゲットサンドボックスと組織にインポートできます。

>[!NOTE]
>
>また、Schema Registry APIを使用して、スキーマに加えて、クラス、スキーマフィールドグループ、データタイプなどの他のリソースを書き出すこともできます。 詳しくは、[ エクスポートエンドポイントガイド ](../api/export.md)を参照してください。

## 前提条件

Experience Platform UIではXDM リソースを書き出すことができますが、ワークフローを完了するには、Schema Registry APIを使用してそれらのリソースを他のサンドボックスや組織に読み込む必要があります。 このガイドに従う前に、必要な認証ヘッダーに関する重要な情報については、[ スキーマレジストリ APIの使用を開始する](../api/getting-started.md)に関するガイドを参照してください。

>[!NOTE]
>
>**削除**&#x200B;や&#x200B;**JSON構造をコピー**&#x200B;などのアクションが見つからない場合は、カスタム （テナント定義）リソースを使用して、テーブル行メニューまたは詳細ビュー（**[!UICONTROL More]**）からアクセスしていることを確認してください。 アクションの可用性は、権限と使用制限によっても異なります。 [ スキーマ、クラス、フィールドグループ、およびデータタイプの管理：アクションと削除](./explore.md#xdm-resource-actions)を参照してください。

## 書き出しペイロードの生成 {#generate-export-payload}

書き出しペイロードは、[!UICONTROL Browse] タブの詳細パネルから、またはスキーマエディターのスキーマのキャンバスから直接、Experience Platform UIで生成できます。

書き出しペイロードを生成するには、左側のナビゲーションで「**[!UICONTROL Schemas]**」を選択します。 [!UICONTROL Schemas] ワークスペース内で、書き出すスキーマの行を選択して、右側のサイドバーにスキーマの詳細を表示します。

>[!TIP]
>
>探しているXDM リソースを見つける方法の詳細については、[XDM リソースの探索](./explore.md)に関するガイドを参照してください。

次に、使用可能なオプションから「**[!UICONTROL Copy JSON]**」アイコン（![ コピーアイコン ](/help/images/icons/copy.png)）を選択します。

![ スキーマ行と[!UICONTROL Copy to JSON]がハイライト表示されたスキーマワークスペース。](../images/ui/export/copy-json.png)

これにより、スキーマ構造に基づいて生成されたJSON ペイロードがクリップボードにコピーされます。 上記の「[!DNL Loyalty Members]」スキーマの場合、次のJSONが生成されます。

+++サンプル JSON ペイロードを展開するには、を選択します

```json
[
  {
    "$id": "https://ns.adobe.com/<XDM_TENANTID_PLACEHOLDER>/mixins/9ecfd881d0053568d277b792e4d24c6b70ffa7782bd31265",
    "meta:altId": "_<XDM_TENANTID_PLACEHOLDER>.mixins.9ecfd881d0053568d277b792e4d24c6b70ffa7782bd31265",
    "meta:resourceType": "mixins",
    "version": "1.0",
    "title": "Loyalty details",
    "type": "object",
    "description": "",
    "definitions": {
      "customFields": {
        "type": "object",
        "properties": {
          "_<XDM_TENANTID_PLACEHOLDER>": {
            "type": "object",
            "properties": {
              "loyalty": {
                "title": "Loyalty",
                "description": "",
                "type": "object",
                "isRequired": false,
                "required": [
                  
                ],
                "properties": {
                  "loyaltyId": {
                    "title": "Loyalty ID",
                    "description": "",
                    "type": "string",
                    "isRequired": false,
                    "required": [
                      
                    ],
                    "meta:xdmType": "string"
                  },
                  "memberSince": {
                    "title": "Member Since",
                    "description": "",
                    "type": "string",
                    "isRequired": false,
                    "required": [
                      
                    ],
                    "format": "date",
                    "meta:xdmType": "date"
                  },
                  "points": {
                    "title": "Points",
                    "description": "",
                    "type": "integer",
                    "isRequired": false,
                    "required": [
                      
                    ],
                    "meta:xdmType": "int"
                  },
                  "loyaltyLevel": {
                    "title": "Loyalty Level",
                    "description": "",
                    "type": "string",
                    "isRequired": false,
                    "required": [
                      
                    ],
                    "enum": [
                      "platinum",
                      "gold",
                      "silver",
                      "bronze"
                    ],
                    "meta:enum": {
                      "platinum": "Platinum",
                      "gold": "Gold",
                      "silver": "Silver",
                      "bronze": "Bronze"
                    },
                    "meta:xdmType": "string"
                  }
                },
                "meta:xdmType": "object"
              }
            },
            "meta:xdmType": "object"
          }
        },
        "meta:xdmType": "object"
      }
    },
    "allOf": [
      {
        "$ref": "#/definitions/customFields",
        "type": "object",
        "meta:xdmType": "object"
      }
    ],
    "meta:extensible": true,
    "meta:abstract": true,
    "meta:intendedToExtend": [
      
    ],
    "meta:xdmType": "object",
    "meta:sandboxId": "1bd86660-c5da-11e9-93d4-6d5fc3a66a8e",
    "meta:sandboxType": "production"
  },
  {
    "$id": "https://ns.adobe.com/<XDM_TENANTID_PLACEHOLDER>/schemas/1e5a739ded8fd1d766a0e06e881a38031874dddd1c7020ad",
    "meta:altId": "_<XDM_TENANTID_PLACEHOLDER>.schemas.1e5a739ded8fd1d766a0e06e881a38031874dddd1c7020ad",
    "meta:resourceType": "schemas",
    "version": "1.4",
    "title": "Loyalty Members",
    "type": "object",
    "description": "Describes customers who are members of a loyalty program.",
    "allOf": [
      {
        "$ref": "https://ns.adobe.com/xdm/context/profile",
        "type": "object",
        "meta:xdmType": "object"
      },
      {
        "$ref": "https://ns.adobe.com/xdm/context/profile-person-details",
        "type": "object",
        "meta:xdmType": "object"
      },
      {
        "$ref": "https://ns.adobe.com/xdm/context/profile-personal-details",
        "type": "object",
        "meta:xdmType": "object"
      },
      {
        "$ref": "https://ns.adobe.com/<XDM_TENANTID_PLACEHOLDER>/mixins/9ecfd881d0053568d277b792e4d24c6b70ffa7782bd31265",
        "type": "object",
        "meta:xdmType": "object"
      },
      {
        "$ref": "https://ns.adobe.com/xdm/mixins/profile-consents",
        "type": "object",
        "meta:xdmType": "object"
      }
    ],
    "meta:extensible": false,
    "meta:abstract": false,
    "meta:extends": [
      "https://ns.adobe.com/xdm/context/profile-person-details",
      "https://ns.adobe.com/xdm/context/profile-personal-details",
      "https://ns.adobe.com/xdm/common/auditable",
      "https://ns.adobe.com/xdm/data/record",
      "https://ns.adobe.com/xdm/context/profile",
      "https://ns.adobe.com/<XDM_TENANTID_PLACEHOLDER>/mixins/9ecfd881d0053568d277b792e4d24c6b70ffa7782bd31265",
      "https://ns.adobe.com/xdm/mixins/profile-consents"
    ],
    "meta:xdmType": "object",
    "meta:class": "https://ns.adobe.com/xdm/context/profile",
    "meta:sandboxId": "1bd86660-c5da-11e9-93d4-6d5fc3a66a8e",
    "meta:sandboxType": "production",
    "meta:immutableTags": [
      
    ]
  }
]
```

+++

ペイロードは、スキーマエディターの右上にある「[!UICONTROL More]」を選択してコピーすることもできます。 ドロップダウンメニューには、[!UICONTROL Copy JSON structure]と[!UICONTROL Delete schema]の2つのオプションがあります。

>[!NOTE]
>
>プロファイルに対してスキーマが有効になっている場合、または関連するデータセットがある場合は、スキーマを削除できません。

![[!UICONTROL More]と[!UICONTROL Copy to JSON]を含むスキーマエディターがハイライト表示されています。](../images/ui/export/schema-editor-copy-json.png)

ペイロードは配列の形式を取り、各配列項目は書き出すカスタム XDM リソースを表すオブジェクトです。 上記の例では、「[!DNL Loyalty details]」カスタムフィールドグループと「[!DNL Loyalty Members]」スキーマが含まれています。 スキーマで使用されるコアリソースは、すべてのサンドボックスと組織で使用できるため、書き出しには含まれません。

組織のテナント IDの各インスタンスは、ペイロードに`<XDM_TENANTID_PLACEHOLDER>`として表示されます。 これらのプレースホルダーは、次の手順でスキーマをインポートする場所に応じて、適切なテナント ID値に自動的に置き換えられます。

## APIを使用したリソースのインポート {#import-resource-with-api}

スキーマの書き出しJSONをコピーしたら、それをSchema Registry APIの`/rpc/import` エンドポイントへのPOST リクエストのペイロードとして使用できます。 目的の組織とサンドボックスにスキーマを送信するように呼び出しを設定する方法について詳しくは、[ インポートエンドポイントガイド ](../api/import.md)を参照してください。

## 次の手順

このガイドに従うことで、XDM スキーマを別の組織またはサンドボックスに正常にエクスポートできました。 [!UICONTROL Schemas] UIの機能について詳しくは、[[!UICONTROL Schemas] UIの概要](./overview.md)を参照してください。
