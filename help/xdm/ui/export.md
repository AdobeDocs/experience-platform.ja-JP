---
solution: Experience Platform
title: UIでのXDMスキーマの書き出し
description: Adobe Experience Platformユーザーインターフェイスで、既存のスキーマを別のサンドボックスまたはIMS組織に書き出す方法を説明します。
topic-legacy: user guide
type: Tutorial
exl-id: c467666d-55bc-4134-b8f4-7758d49c4786
source-git-commit: e4bf5bb77ac4186b24580329699d74d653310d93
workflow-type: tm+mt
source-wordcount: '505'
ht-degree: 0%

---

# UIでのXDMスキーマの書き出し

スキーマライブラリ内のすべてのリソースは、IMS組織内の特定のサンドボックスに含まれます。 サンドボックスとIMS組織の間でエクスペリエンスデータモデル(XDM)リソースを共有する必要が生じる場合があります。

このニーズに対処するために、Adobe Experience Platform UIの[!UICONTROL スキーマ]ワークスペースでは、スキーマライブラリ内の任意のスキーマに対する書き出しペイロードを生成できます。 その後、このペイロードをスキーマレジストリAPIの呼び出しで使用して、スキーマ（およびすべての依存リソース）をターゲットサンドボックスとIMS組織に読み込むことができます。

>[!NOTE]
>
>また、スキーマレジストリAPIを使用して、クラス、スキーマフィールドグループ、データ型など、スキーマに加えて他のリソースを書き出すこともできます。 詳しくは、[書き出し/読み込みエンドポイント](../api/export-import.md)のガイドを参照してください。

## 前提条件

Platform UIではXDMリソースを書き出すことができますが、ワークフローを完了するには、スキーマレジストリAPIを使用して、これらのリソースを他のサンドボックスまたはIMS組織に読み込む必要があります。 このガイドに従う前に、必要な認証ヘッダーに関する重要な情報については、[スキーマレジストリAPI](../api/getting-started.md)の使用の手引きを参照してください。

## 書き出しペイロードの生成

プラットフォームUIで、左側のナビゲーションから「**[!UICONTROL スキーマ]**」を選択します。 [!UICONTROL スキーマ]ワークスペース内で、エクスポートするスキーマを探し、[!DNL Schema Editor]で開きます。

>[!TIP]
>
>探しているXDMリソースの見つけ方について詳しくは、[XDMリソース](./explore.md)のガイドを参照してください。

スキーマを開いたら、キャンバスの右上にある「**[!UICONTROL JSONをコピー]**」アイコン（![アイコンをコピー](../images/ui/export/icon.png)）を選択します。

![](../images/ui/export/copy-json.png)

これにより、JSONペイロードがクリップボードにコピーされ、スキーマ構造に基づいて生成されます。 上記の「[!DNL Loyalty Members]」スキーマの場合、次のJSONが生成されます。

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

ペイロードは配列の形式を取り、各配列項目は書き出すカスタムXDMリソースを表すオブジェクトです。 上記の例では、「[!DNL Loyalty details]」カスタムフィールドグループと「[!DNL Loyalty Members]」スキーマが含まれます。 スキーマが使用するコアリソースは、すべてのサンドボックスとIMS組織で使用できるので、書き出しには含まれません。

組織のテナントIDの各インスタンスは、ペイロードに`<XDM_TENANTID_PLACEHOLDER>`と表示されます。 これらのプレースホルダーは、次の手順でスキーマを読み込む場所に応じて、適切なテナントID値に自動的に置き換えられます。

## APIを使用したリソースの読み込み

スキーマの書き出しJSONをコピーしたら、その書き出しJSONを、スキーマレジストリAPIの`/import`エンドポイントへのPOSTリクエストのペイロードとして使用できます。 目的のIMS Orgおよびサンドボックスにスキーマを送信するための呼び出しの設定方法について詳しくは、「APIでのXDMリソースの読み込み](../api/export-import.md#import)」の節を参照してください。[

## 次の手順

このガイドに従うことで、XDMスキーマを別のIMS組織またはサンドボックスに正常に書き出すことができました。 [!UICONTROL スキーマ] UIの機能について詳しくは、[[!UICONTROL スキーマ] UIの概要](./overview.md)を参照してください。
