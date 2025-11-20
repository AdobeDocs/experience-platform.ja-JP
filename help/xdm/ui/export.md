---
solution: Experience Platform
title: UI での XDM スキーマの書き出し
description: Adobe Experience Platform ユーザー インターフェースで既存のスキーマを別のサンドボックスまたは組織にエクスポートする方法について説明します。
type: Tutorial
exl-id: c467666d-55bc-4134-b8f4-7758d49c4786
source-git-commit: fded2f25f76e396cd49702431fa40e8e4521ebf8
workflow-type: tm+mt
source-wordcount: '636'
ht-degree: 11%

---

# UI での XDM スキーマの書き出し {#export-xdm-schemas-in-the-UI}

>[!CONTEXTUALHELP]
>id="platform_xdm_copyjsonstructure"
>title="JSON 構造をコピー"
>abstract="JSON 構造をクリップボードにコピーして、選択したスキーマの書き出しペイロードを生成します。この機能を使用して、スキーマライブラリ内の任意のスキーマの詳細を書き出します。この書き出した JSON を使用して、スキーマと関連リソースを別のサンドボックスまたは組織に読み込むことができます。これにより、異なる環境間でのスキーマの共有と再利用が簡単で効率的になります。"

スキーマライブラリ内のすべてのリソースは、組織内の特定のサンドボックスに格納されます。 場合によっては、サンドボックスと組織の間で Experience Data Model （XDM）リソースを共有する必要があります。

このニーズに対応するために、Adobe Experience Platform UI の [!UICONTROL Schemas] ワークスペースでは、スキーマライブラリ内の任意のスキーマの書き出しペイロードを生成できます。 その後、このペイロードを Schema Registry API の呼び出しで使用して、スキーマ（およびすべての依存リソース）をターゲットサンドボックスおよび組織に読み込むことができます。

>[!NOTE]
>
>また、Schema Registry API を使用して、クラス、スキーマフィールドグループ、データタイプなどのスキーマに加えて、他のリソースを書き出すこともできます。 詳しくは、[&#x200B; 書き出しエンドポイントガイド &#x200B;](../api/export.md) を参照してください。

## 前提条件

Experience Platform UIではXDMリソースをエクスポートできますが、ワークフローを完了するには、スキーマ Registry API を使用してそれらのリソースを他のサンドボックスまたは組織にインポートする必要があります。 このガイドに従う前に、必要な認証ヘッダーに関する重要な情報については、 [スキーマレジストリ API の概要](../api/getting-started.md) のガイドを参照してください。

## 書き出しペイロードの生成 {#generate-export-payload}

書き出しペイロードは、 [!UICONTROL Browse] タブの詳細パネルからExperience Platform UIで生成することも、スキーマエディターのスキーマのキャンバスから直接生成することもできます。

書き出しペイロードを生成するには、左側のナビゲーションで「**[!UICONTROL Schemas]**」を選択します。 [!UICONTROL Schemas] ワークスペース内で、書き出すスキーマの行を選択して、右側のサイドバーにスキーマの詳細を表示します。

>[!TIP]
>
>探している XDM リソースを見つける方法について詳しくは、[XDM リソースの調査 &#x200B;](./explore.md) に関するガイドを参照してください。

次に、使用可能なオプションから **[!UICONTROL Copy JSON]** アイコン（![&#x200B; コピーアイコン &#x200B;](/help/images/icons/copy.png)）を選択します。

![&#x200B; スキーマ行と [!UICONTROL Copy to JSON] がハイライト表示されたスキーマワークスペース。](../images/ui/export/copy-json.png)

これにより、スキーマ構造に基づいて生成された JSON ペイロードがクリップボードにコピーされます。 上記の「[!DNL Loyalty Members]」スキーマの場合、次の JSON が生成されます。

+++サンプルの JSON ペイロードを選択して展開します

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

ペイロードは、スキーマエディターの右上にある「[!UICONTROL More]」を選択してコピーすることもできます。 ドロップダウンメニューには、[ [!UICONTROL Copy JSON structure] ] と [ [!UICONTROL Delete schema]] の 2 つのオプションがあります。

>[!NOTE]
>
>スキーマがプロフィールに対して有効になっている場合、またはデータセットが関連付けられている場合は削除できません。

![スキーマエディターで [!UICONTROL More] と [!UICONTROL Copy to JSON] がハイライト表示されます。](../images/ui/export/schema-editor-copy-json.png)

ペイロードは配列の形式を取り、各配列項目はエクスポートするカスタム XDM リソースを表すオブジェクトです。 上記の例では、「[!DNL Loyalty details]」カスタムフィールドグループと「[!DNL Loyalty Members]」スキーマが含まれています。 スキーマで使用されるコアリソースは、すべてのサンドボックスと組織で使用できるので、書き出しに含まれません。

組織のテナント ID の各インスタンスは、ペイロードでは `<XDM_TENANTID_PLACEHOLDER>` として表示されます。 これらのプレースホルダーは、次の手順でスキーマを読み込む場所に応じて、適切なテナント ID 値に自動的に置き換えられます。

## API を使用したリソースの読み込み {#import-resource-with-api}

スキーマの書き出し JSON をコピーしたら、Schema Registry API の `/rpc/import` エンドポイントに対する POST リクエストのペイロードとして使用できます。 スキーマを目的の組織およびサンドボックスに送信するための呼び出しを設定する方法について詳しくは、[&#x200B; インポートエンドポイントガイド &#x200B;](../api/import.md) を参照してください。

## 次の手順

このガイドに従うと、XDM スキーマを別の組織またはサンドボックスに正常に書き出すことができます。 [!UICONTROL Schemas] UI の機能について詳しくは、[[!UICONTROL Schemas] UI の概要を参照してください &#x200B;](./overview.md)
