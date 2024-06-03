---
solution: Experience Platform
title: UI での XDM スキーマの書き出し
description: Adobe Experience Platform ユーザーインターフェイスで既存のスキーマを別のサンドボックスまたは組織に書き出す方法について説明します。
type: Tutorial
exl-id: c467666d-55bc-4134-b8f4-7758d49c4786
source-git-commit: 0f0842c1d14ce42453b09bf97e1f3690448f6e9a
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 0%

---

# UI での XDM スキーマの書き出し {#export-xdm-schemas-in-the-UI}

>[!CONTEXTUALHELP]
>id="platform_xdm_copyjsonstructure"
>title="JSON 構造をコピー"
>abstract="JSON 構造をクリップボードにコピーして、選択したスキーマの書き出しペイロードを生成します。 この機能を使用して、スキーマライブラリ内の任意のスキーマの詳細を書き出します。 この書き出された JSON を使用して、スキーマと関連リソースを別のサンドボックスまたは組織に読み込むことができます。 これにより、異なる環境間でのスキーマの共有と再利用が簡単かつ効率的になります。"

スキーマライブラリ内のすべてのリソースは、組織内の特定のサンドボックスに格納されます。 場合によっては、サンドボックスと組織の間で Experience Data Model （XDM）リソースを共有する必要があります。

このニーズに対応するには、 [!UICONTROL スキーマ] Adobe Experience Platform UI のワークスペースを使用すると、スキーマライブラリ内の任意のスキーマの書き出しペイロードを生成できます。 その後、このペイロードを Schema Registry API の呼び出しで使用して、スキーマ（およびすべての依存リソース）をターゲットサンドボックスおよび組織に読み込むことができます。

>[!NOTE]
>
>また、Schema Registry API を使用して、クラス、スキーマフィールドグループ、データタイプなどのスキーマに加えて、他のリソースを書き出すこともできます。 を参照してください。 [書き出しエンドポイントガイド](../api/export.md) を参照してください。

## 前提条件

Platform UI では XDM リソースを書き出すことができますが、ワークフローを完了するには、スキーマレジストリ API を使用して、これらのリソースを他のサンドボックスまたは組織に読み込む必要があります。 のガイドを参照してください [スキーマレジストリ API の概要](../api/getting-started.md) このガイドに従う前に、必要な認証ヘッダーに関する重要な情報を確認してください。

## エクスポートペイロードを生成 {#generate-export-payload}

書き出しペイロードは、の詳細パネルから Platform UI で生成できます [!UICONTROL 参照] タブ、またはスキーマエディターのスキーマのキャンバスから直接。

書き出しペイロードを生成するには、次を選択します **[!UICONTROL スキーマ]** 左側のナビゲーションの 内 [!UICONTROL スキーマ] ワークスペースで、書き出すスキーマの行を選択して、スキーマの詳細を右側のサイドバーに表示します。

>[!TIP]
>
>のガイドを参照してください [xdm リソースの調査](./explore.md) 探している XDM リソースを見つける方法の詳細については、を参照してください。

次に、 **[!UICONTROL JSON をコピー]** アイコン （![コピーアイコン](../images/ui/export/icon.png)）を選択できます。

![スキーマ行を含むスキーマワークスペースと [!UICONTROL JSON にコピー] ハイライト表示](../images/ui/export/copy-json.png)

これにより、スキーマ構造に基づいて生成された JSON ペイロードがクリップボードにコピーされます。 「」の場合[!DNL Loyalty Members]上記のスキーマでは、次の JSON が生成されます。

+++選択すると、サンプルの JSON ペイロードを展開できます

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

ペイロードは、次を選択してコピーすることもできます [!UICONTROL 詳細] をスキーマエディターの右上に表示します。 ドロップダウンメニューには、次の 2 つのオプションがあります。 [!UICONTROL JSON 構造をコピー] および [!UICONTROL スキーマを削除].

>[!NOTE]
>
>プロファイルに対して有効になっている場合や、スキーマに関連付けられたデータセットがある場合は、スキーマを削除できません。

![スキーマエディター [!UICONTROL 詳細] および [!UICONTROL JSON にコピー] ハイライト表示](../images/ui/export/schema-editor-copy-json.png)

ペイロードは配列の形式を取り、各配列項目は、書き出すカスタム XDM リソースを表すオブジェクトです。 上記の例では、「[!DNL Loyalty details]「カスタムフィールドグループと」[!DNL Loyalty Members]」スキーマが含まれています。 スキーマで使用されるコアリソースは、すべてのサンドボックスと組織で使用できるので、書き出しに含まれません。

組織のテナント ID の各インスタンスは、次のように表示されます `<XDM_TENANTID_PLACEHOLDER>` ペイロード内。 これらのプレースホルダーは、次の手順でスキーマを読み込む場所に応じて、適切なテナント ID 値に自動的に置き換えられます。

## API を使用したリソースの読み込み {#import-resource-with-api}

スキーマの書き出し JSON をコピーしたら、それをPOSTリクエストのペイロードとして使用し、 `/rpc/import` スキーマレジストリ API のエンドポイント。 を参照してください。 [インポートエンドポイントガイド](../api/import.md) スキーマを目的の組織およびサンドボックスに送信するための呼び出しを設定する方法の詳細については、を参照してください。

## 次の手順

このガイドに従うと、XDM スキーマを別の組織またはサンドボックスに正常に書き出すことができます。 の機能について詳しくは、を参照してください [!UICONTROL スキーマ] UI。を参照してください。 [[!UICONTROL スキーマ] UI の概要](./overview.md).
