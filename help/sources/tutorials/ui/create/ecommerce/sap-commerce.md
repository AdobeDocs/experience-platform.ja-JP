---
title: UI での SAP Commerce ソース接続の作成
description: Adobe Experience Platform UI を使用して SAP Commerce ソース接続を作成する方法について説明します。
exl-id: 6484e51c-77cd-4dbd-9c68-0a4e3372da33
source-git-commit: e402a58f51de49b26f9d279cebf551ec11e4698f
workflow-type: tm+mt
source-wordcount: '959'
ht-degree: 28%

---

# UI での [!DNL SAP Commerce] ソース接続の作成

以下のチュートリアルでは、Adobe Experience Platform ユーザーインターフェイスを使用して [!DNL SAP Commerce] サブスクリプション請求 [[!DNL SAP]  連絡先と顧客データを取り込むための ](https://www.sap.com/products/financial-management/subscription-billing.html) ソース接続を作成する手順を説明します。

## はじめに {#getting-started}

このチュートリアルは、 Experience Platform の次のコンポーネントを実際に利用および理解しているユーザーを対象としています。

* [[!DNL Experience Data Model (XDM)]  システム](../../../../../xdm/home.md)：[!DNL Experience Platform] が顧客体験データの整理に使用する標準化されたフレームワーク。
   * [スキーマ構成の基本](../../../../../xdm/schema/composition.md)：スキーマ構成の主要な原則やベストプラクティスなど、XDM スキーマの基本的な構成要素について学びます。
   * [スキーマエディターのチュートリアル](../../../../../xdm/tutorials/create-schema-ui.md)：スキーマエディター UI を使用してカスタムスキーマを作成する方法を説明します。
* [[!DNL Real-Time Customer Profile]](../../../../../profile/home.md)：複数のソースからの集計データに基づいて、統合されたリアルタイムの顧客プロファイルを提供します。

有効な [!DNL SAP Commerce] アカウントを既にお持ちの場合は、このドキュメントの残りの部分をスキップし、[データフローの設定](../../dataflow/ecommerce.md)に関するチュートリアルに進んでください。

### 必要な資格情報の収集 {#gather-credentials}

[!DNL SAP Commerce] をExperience Platformに接続するには、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 |
| --- | --- |
| クライアント ID | サービスキーからの `clientId` の値。 |
| クライアントシークレット | サービスキーからの `clientSecret` の値。 |
| トークンエンドポイント | サービスキーからの `url` の値は、`https://subscriptionbilling.authentication.eu10.hana.ondemand.com` と似ています。 |
| 領域 | データセンターの場所。 領域は `url` 内に存在し、値は `eu10` または `us10` に類似しています。 例えば、`url` が `https://eu10.revenue.cloud.sap/api` の場合、`eu10` が必要になります。 |

詳しくは、[[!DNL SAP Commerce]  ドキュメント ](https://help.sap.com/docs/CLOUD_TO_CASH_OD/987aec876092428f88162e438acf80d6/c5fcaf96daff4c7a8520188e4d8a1843.html) を参照してください。

### Experience Platform スキーマの作成 {#create-platform-schema}

[!DNL SAP Commerce] ソース接続を作成する前に、まずソースに使用するExperience Platform スキーマを作成する必要もあります。 スキーマの作成方法に関する包括的な手順については、[Experience Platform スキーマの作成 ](../../../../../xdm/schema/composition.md) に関するチュートリアルを参照してください。

次のセクションを展開すると、スキーマの例が表示されます。

+++ スキーマの例を表示

```
{
  "_extconndev": {
    "addresses": [
      {
        "addressUUID": "{ADDRESS_UUID}",
        "city": "Burnaby",
        "country": "Canada",
        "email": "chandni@acme.com",
        "houseNumber": "27",
        "isDefault": false,
        "phone": "123-456-7890",
        "postalCode": "V3J 1X9",
        "state": "British Columbia",
        "street": "Beresford"
      }
    ],
    "changedAt": "1687204041",
    "changedBy": "vero@acme.com",
    "contactNumber": "123-456-7980",
    "corporateInfo": {
      "company": "acme"
    },
    "createAt": "1687204041",
    "createdBy": "vero@acme.com",
    "customReferences": [
      {
        "id": "Sample value",
        "typeCode": "Sample value"
      }
    ],
    "customerNumber": "Sample value",
    "customerType": "Sample value",
    "defaultAddress": {
      "addressUUID": "Sample value",
      "city": "North Vancouver",
      "country": "Canada",
      "email": "chandni@acme.come",
      "houseNumber": "34",
      "isDefault": false,
      "phone": "123-456-7890",
      "postalCode": "V7H 2P1",
      "state": "British Columbia",
      "street": "Maple"
    },
    "externalObjectReferences": [
      {
        "externalId": "{EXTERNAL_ID}",
        "externalIdTypeCode": "{EXTERNAL_ID_TYPE_CODE}",
        "externalSystemId": "{EXTERNAL_SYSTEM_ID}"
      }
    ],
    "markets": [
      {
        "active": false,
        "country": "USA",
        "currency": "USD",
        "marketId": "Sample value",
        "priceinfo": {
          "incoterms": "{INCO_TERMS}",
          "incotermsLocation": "{INCO_TERMS_LOCATION}",
          "priceGroup": "{PRICE_GROUP}",
          "priceListType": "{PRICE_LIST_TYPE}"
        },
        "salesArea": {
          "distributionChannel": "{DISTRIBUTION_CHANNEL}",
          "division": "{DIVISION}",
          "salesOrganization": "{SALES_ORGANIZATION}"
        }
      }
    ],
    "personalInfo": {
      "firstName": "Chandni",
      "lastName": "Kaur"
    }
  },
  "_id": "/uri-reference",
  "_repo": {
    "createDate": "2004-10-23T12:00:00-06:00",
    "modifyDate": "2004-10-23T12:00:00-06:00"
  },
  "createdByBatchID": "/uri-reference",
  "modifiedByBatchID": "/uri-reference",
  "personID": "{PERSON_ID}",
  "repositoryCreatedBy": "kevin@acme.com",
  "repositoryLastModifiedBy": "kevin@acme.com"
}
```

+++

## [!DNL SAP Commerce] アカウントを接続 {#connect-account}

Experience Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、「[!UICONTROL &#x200B; ソース &#x200B;] ワークスペースにアクセスします。 [!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

*e コマース* カテゴリで、「**[!UICONTROL SAP Commerce]**」を選択し、次に「**[!UICONTROL データを追加]**」を選択します。

![SAP Commerce カードを含むカタログのExperience Platform UI のスクリーンショット ](../../../../images/tutorials/create/ecommerce/sap-commerce/catalog-card.png)

**[!UICONTROL Connect SAP Commerce アカウント]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント {#existing-account}

既存のアカウントを使用するには、新しいデータフローを作成する [!DNL SAP Commerce] アカウントを選択し、「**[!UICONTROL 次へ]**」を選択して続行します。

![SAP Commerce アカウントを既存のアカウントと接続するためのExperience Platform UI のスクリーンショット ](../../../../images/tutorials/create/ecommerce/sap-commerce/existing.png)

### 新規アカウント {#new-account}

新しいアカウントを作成する場合は、「**[!UICONTROL 新しいアカウント]**」を選択し、続けて名前、説明（オプション）、の認証情報を指定します。 終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![SAP Commerce アカウントを新しいアカウントに接続するためのExperience Platform UI のスクリーンショット ](../../../../images/tutorials/create/ecommerce/sap-commerce/new.png)

### データの選択 {#select-data}

最後に、Experience Platformに取り込むオブジェクトタイプを選択する必要があります。

| オブジェクトタイプ | 説明 |
| --- | --- |
| `Customers` | サブスクリプションを持つエンティティ。 |
| `Contacts` | 顧客の連絡先の詳細。 |

>[!BEGINTABS]

>[!TAB  顧客 ]

顧客データを取り込むには、オブジェクトタイプとして **[!UICONTROL 顧客]** を選択してから、**[!UICONTROL 次へ]** を選択します。

![ お客様のオプションが選択された設定を示す、SAP CommerceのExperience Platform UI のスクリーンショット ](../../../../images/tutorials/create/ecommerce/sap-commerce/configuration-customers.png)

>[!TAB  連絡先 ]

連絡先データを取り込むには、オブジェクトタイプとして **[!UICONTROL 連絡先]** を選択してから、**[!UICONTROL 次へ]** を選択します。

![ 連絡先オプションが選択された設定を示す、SAP CommerceのExperience Platform UI のスクリーンショット ](../../../../images/tutorials/create/ecommerce/sap-commerce/configuration-contacts.png)

>[!ENDTABS]

## 次の手順 {#next-steps}

このチュートリアルでは、[!DNL SAP Commerce] アカウントとの接続を確立しました。次のチュートリアルに進み、[ データをExperience Platformに取り込むためのデータフローの設定 ](../../dataflow/ecommerce.md) を行いましょう。

## その他のリソース {#additional-resources}

以下の節では、[!DNL SAP Commerce] ソースを使用する際に参照できるその他のリソースを示します。

### マッピング {#mapping}

Experience Platformは、選択したターゲットスキーマまたはデータセットに基づいて、自動マッピングされたフィールドに対してインテリジェントなレコメンデーションを提供します。 マッピングルールは、ユースケースに合わせて手動で調整できます。 必要に応じて、フィールドを直接マッピングするか、データ準備機能を使用してソースデータを変換して計算値を導き出すかを選択できます。マッパーインターフェイスと計算フィールドの使用に関する包括的な手順については、[ データ準備 UI ガイド ](../../../../../data-prep/ui/mapping.md) を参照してください。

データフローのマッピング設定は、スキーマと、取り込むように選択したオブジェクトタイプによって異なります。

>[!BEGINTABS]

>[!TAB  顧客 ]

顧客データの場合、[!DNL SAP Commerce] は [ API の ](https://api.sap.com/api/BusinessPartner_APIs/path/GET_customers)customers[ エンドポイントと ](https://api.sap.com/api/BusinessPartner_APIs/path/GET_relationships-customer-contacts)customer-contacts relationship[!DNL SAP Business Partners] エンドポイントを使用してデータを取得します

以下は、顧客データのデータフローのマッピング設定 [!DNL SAP Commerce] 例です。

| ターゲットフィールド | 説明 |
| --- | --- |
| `customerNumber` | 顧客の番号。 |
| `corporateInfo` | 顧客の番号。 |
| `customerType` | 顧客タイプ。 |
| `createdAt` | 顧客がいつ作成されたかを示すタイムスタンプ。 |
| `changedAt` | 顧客が最後に更新された日時を示すタイムスタンプ。 |
| `markets[*].country` | 顧客のマーケットは、配列オブジェクトとして取得されます。 |
| `addresses[*].email` | 顧客の複数のアドレスに関連付けられたメールで、配列オブジェクトとして取得されます。 |
| `addresses[*].city` | 顧客の複数のアドレスに関連付けられた市区町村で、配列オブジェクトとして取得されます。 |
| `addresses[*].addressUUID` | 顧客の複数のアドレスに関連付けられた ID。配列オブジェクトとして取得されます。 |
| `externalObjectReferences[*].externalSystemId` | 追加データ。配列オブジェクトとして取得されます。 |
| `externalObjectReferences[*].externalId` | 追加データ。配列オブジェクトとして取得されます。 |
| `customReferences[*].id` | 追加データ。配列オブジェクトとして取得されます。 |
| `customReferences[*].typeCode` | 追加データ。配列オブジェクトとして取得されます。 |

![ ソースワークフローのマッピングステップ ](../../../../images/tutorials/create/ecommerce/sap-commerce/mapping-customers.png)

>[!TAB  連絡先 ]

連絡先データの場合、[!DNL SAP Commerce] は [ API の ](https://api.sap.com/api/BusinessPartner_APIs/path/GET_contacts)contacts[!DNL SAP Business Partners] エンドポイントを使用してデータを取得します。

以下は、連絡先データのデータフローのマッピング設定 [!DNL SAP Commerce] 例です。

| ターゲットフィールド | 説明 |
| --- | --- |
| `contactNumber` | 連絡先の番号。 |
| `createdAt` | 連絡先が作成された日時を示すタイムスタンプ。 |
| `changedAt` | 連絡先が最後に更新された日時を示すタイムスタンプ。 |
| `personalInfo.lastName` | 連絡先の姓。 |
| `personalInfo.firstName` | 連絡先の名。 |
| `externalObjectReferences[*].externalSystemId` | 追加データ。配列オブジェクトとして取得されます。 |
| `externalObjectReferences[*].externalId` | 追加データ。配列オブジェクトとして取得されます。 |
| `externalObjectReferences[*].externalIdTypeCode` | 追加データ。配列オブジェクトとして取得されます。 |

![ ソースワークフローのマッピングステップ ](../../../../images/tutorials/create/ecommerce/sap-commerce/mapping-contacts.png)

>[!ENDTABS]
