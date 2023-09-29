---
solution: Experience Platform
title: UI での XDM スキーマの書き出し
description: Adobe Experience Platformユーザーインターフェイスで、既存のスキーマを別のサンドボックスまたは組織に書き出す方法を説明します。
type: Tutorial
exl-id: c467666d-55bc-4134-b8f4-7758d49c4786
source-git-commit: d25042e80ca5f655a50deac6a65ce9168225d6e6
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 0%

---

# UI での XDM スキーマの書き出し

スキーマライブラリ内のすべてのリソースは、組織内の特定のサンドボックスに含まれます。 サンドボックスと組織の間で Experience Data Model(XDM) リソースを共有する必要が生じる場合があります。

このニーズに対処するには、 [!UICONTROL スキーマ] Adobe Experience Platform UI のワークスペースを使用すると、スキーマライブラリ内の任意のスキーマに対して書き出しペイロードを生成できます。 その後、このペイロードを Schema Registry API の呼び出しで使用して、スキーマ（およびすべての依存リソース）をターゲットサンドボックスと組織に読み込むことができます。

>[!NOTE]
>
>スキーマレジストリ API を使用して、クラス、スキーマフィールドグループ、データ型などのスキーマに加えて、他のリソースを書き出すこともできます。 詳しくは、 [書き出しエンドポイントガイド](../api/export.md) を参照してください。

## 前提条件

Platform UI では XDM リソースを書き出すことができますが、ワークフローを完了するには、スキーマレジストリ API を使用して、これらのリソースを他のサンドボックスまたは組織に読み込む必要があります。 に関するガイドを参照してください。 [スキーマレジストリ API の概要](../api/getting-started.md) を参照してください。

## 書き出しペイロードの生成 {#generate-export-payload}

書き出しペイロードは、Platform UI の [!UICONTROL 参照] 」タブをクリックするか、スキーマエディターのスキーマのキャンバスから直接アクセスします。

書き出しペイロードを生成するには、「 **[!UICONTROL スキーマ]** をクリックします。 内 [!UICONTROL スキーマ] ワークスペースで、書き出すスキーマの行を選択して、右側のサイドバーにスキーマの詳細を表示します。

>[!TIP]
>
>次のガイドを参照してください： [XDM リソースの調査](./explore.md) を参照してください。

次に、 **[!UICONTROL JSON をコピー]** アイコン (![コピーアイコン](../images/ui/export/icon.png)) を使用可能なオプションから削除します。

![スキーマ行と [!UICONTROL JSON にコピー] ハイライト表示されました。](../images/ui/export/copy-json.png)

これにより、スキーマ構造に基づいて生成された JSON ペイロードがクリップボードにコピーされます。 (「[!DNL Loyalty Members]」スキーマが上に表示されている場合、次の JSON が生成されます。

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

ペイロードは、 [!UICONTROL その他] をクリックします。 ドロップダウンメニューには次の 2 つのオプションがあります。 [!UICONTROL JSON 構造をコピーする] および [!UICONTROL スキーマを削除].

>[!NOTE]
>
>プロファイルに対して有効になっているか、関連するデータセットがある場合、スキーマは削除できません。

![次のスキーマエディター： [!UICONTROL その他] および [!UICONTROL JSON にコピー] ハイライト表示されました。](../images/ui/export/schema-editor-copy-json.png)

ペイロードは配列の形式を取り、各配列項目は、書き出すカスタム XDM リソースを表すオブジェクトです。 上記の例では、「[!DNL Loyalty details]&quot;カスタムフィールドグループと&quot;[!DNL Loyalty Members]」個のスキーマが含まれます。 スキーマが使用するコアリソースは、すべてのサンドボックスおよび組織で使用できるので、書き出しには含まれません。

組織のテナント ID の各インスタンスは、次のように表示されます。 `<XDM_TENANTID_PLACEHOLDER>` ペイロード内に含まれます。 これらのプレースホルダーは、次の手順でスキーマを読み込む場所に応じて、自動的に適切なテナント ID 値に置き換えられます。

## API を使用してリソースを読み込む

スキーマの書き出し JSON をコピーしたら、それをに対するPOSTリクエストのペイロードとして使用できます。 `/rpc/import` エンドポイント（スキーマレジストリ API 内） 詳しくは、 [インポートエンドポイントガイド](../api/import.md) を参照してください。

## 次の手順

このガイドに従うことで、XDM スキーマを別の組織またはサンドボックスに正常に書き出しました。 の機能の詳細については、 [!UICONTROL スキーマ] UI( [[!UICONTROL スキーマ] UI の概要](./overview.md).
