---
solution: Experience Platform
title: UIでのXDMスキーマの書き出し
description: Adobe Experience Platformユーザーインターフェイスで、既存のスキーマを別のサンドボックスまたはIMS組織にエクスポートする方法を説明します。
topic-legacy: user guide
type: Tutorial
exl-id: c467666d-55bc-4134-b8f4-7758d49c4786
translation-type: tm+mt
source-git-commit: 5d449c1ca174cafcca988e9487940eb7550bd5cf
workflow-type: tm+mt
source-wordcount: '502'
ht-degree: 0%

---

# UIでのXDMスキーマの書き出し

スキーマライブラリ内のすべてのリソースは、IMS組織内の特定のサンドボックスに含まれます。 場合によっては、サンドボックスとIMS組織の間でエクスペリエンスデータモデル(XDM)リソースを共有する必要があります。

このニーズに対処するために、Adobe Experience PlatformUIの[!UICONTROL スキーマ]ワークスペースを使用して、スキーマライブラリ内の任意のスキーマに対するエクスポートペイロードを生成できます。 その後、このペイロードをスキーマレジストリAPIの呼び出しで使用して、スキーマ（およびすべての依存リソース）をターゲットサンドボックスとIMS組織にインポートできます。

>[!NOTE]
>
>スキーマレジストリAPIを使用して、クラス、ミックスイン、データ型などのスキーマに加えて、他のリソースも書き出すことができます。 詳しくは、[export/import endpoints](../api/export-import.md)のガイドを参照してください。

## 前提条件

Platform UIではXDMリソースの書き出しが可能ですが、ワークフローを完了するには、スキーマレジストリAPIを使用して他のサンドボックスまたはIMS Orgsにこれらのリソースを読み込む必要があります。 このガイドに従う前に、必要な認証ヘッダーに関する重要な情報については、[スキーマレジストリAPI](../api/getting-started.md)の使い始めに関するガイドを参照してください。

## エクスポートペイロードの生成

Platform UIの左側のナビゲーションで「**[!UICONTROL スキーマ]**」を選択します。 [!UICONTROL スキーマ]ワークスペース内で、エクスポートするスキーマを探し、[!DNL Schema Editor]で開きます。

>[!TIP]
>
>XDMリソースを探す方法の詳細は、[XDMリソース](./explore.md)のガイドを参照してください。

スキーマを開いたら、キャンバスの右上にある「JSONをコピー&#x200B;]**」アイコン（![コピーアイコン](../images/ui/export/icon.png)）を選択します。**[!UICONTROL 

![](../images/ui/export/copy-json.png)

これにより、スキーマー構造に基づいて生成されたJSONペイロードをクリップボードにコピーします。 上記の「[!DNL Loyalty Members]」スキーマに対して、次のJSONが生成されます。

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

ペイロードは配列の形をとり、各配列項目は、エクスポートするカスタムXDMリソースを表すオブジェクトです。 上記の例では、「[!DNL Loyalty details]」カスタムミックスインと「[!DNL Loyalty Members]」スキーマが含まれています。 スキーマが使用するコアリソースは、すべてのサンドボックスおよびIMS組織で使用できるので、エクスポートに含まれません。

組織のテナントIDの各インスタンスは、ペイロードに`<XDM_TENANTID_PLACEHOLDER>`と表示されます。 これらのプレースホルダは、次の手順でスキーマを読み込んだ場所に応じて、適切なテナントID値に自動的に置き換えられます。

## APIを使用したリソースの読み込み

スキーマの書き出しJSONをコピーしたら、スキーマレジストリAPIの`/import`エンドポイントへのPOSTリクエストのペイロードとして使用できます。 スキーマを目的のIMS組織とサンドボックスに送信する呼び出しを設定する方法の詳細については、API](../api/export-import.md#import)での[XDMリソースのインポートに関するセクションを参照してください。

## 次の手順

このガイドに従うことで、XDMスキーマを別のIMS組織またはサンドボックスに正常に書き出すことができます。 [!UICONTROL スキーマ] UIの機能について詳しくは、[[!UICONTROL スキーマ] UIの概要](./overview.md)を参照してください。
