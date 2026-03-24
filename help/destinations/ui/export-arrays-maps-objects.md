---
title: Real-Time CDPからの配列、マップおよびオブジェクトの書き出し
type: Tutorial
description: Real-Time CDPからクラウドストレージの宛先に配列、マップ、オブジェクトを書き出す方法について説明します。
exl-id: ff13d8b7-6287-4315-ba71-094e2270d039
source-git-commit: d946d3dbb09c1fe0163fba3a892b4c0f1b331f87
workflow-type: tm+mt
source-wordcount: '1056'
ht-degree: 14%

---

# [!DNL Real-Time CDP]から配列、マップ、オブジェクトを書き出す {#export-arrays-cloud-storage}

>[!AVAILABILITY]
>
>配列およびその他の複雑なオブジェクトをクラウドストレージの宛先に書き出す機能は、一般的に次の宛先で使用できます：[[!DNL Azure Data Lake Storage Gen2]](../../destinations/catalog/cloud-storage/adls-gen2.md)、[[!DNL Data Landing Zone]](../../destinations/catalog/cloud-storage/data-landing-zone.md)、[[!DNL Google Cloud Storage]](../../destinations/catalog/cloud-storage/google-cloud-storage.md)、[[!DNL Amazon S3]](../../destinations/catalog/cloud-storage/amazon-s3.md)、[[!DNL Azure Blob]](../../destinations/catalog/cloud-storage/azure-blob.md)、[[!DNL SFTP]](../../destinations/catalog/cloud-storage/sftp.md)。
>
>さらに、[Amazon Kinesis](/help/destinations/catalog/cloud-storage/amazon-kinesis.md)、[HTTP API](/help/destinations/catalog/streaming/http-destination.md)、[Azure Event Hubs](/help/destinations/catalog/cloud-storage/azure-event-hubs.md)の宛先にmap-type フィールドを書き出すことができます。


[!DNL Real-Time CDP]から[ クラウドストレージの宛先](/help/destinations/catalog/cloud-storage/overview.md)に配列、マップ、オブジェクトを書き出す方法について説明します。 さらに、マップタイプのフィールドを[ エンタープライズ宛先](/help/destinations/destination-types.md#advanced-enterprise-destinations)に書き出すことができ、[ エッジパーソナライゼーション宛先](/help/destinations/destination-types.md#edge-personalization-destinations)に制限があります。 このドキュメントでは、書き出しワークフロー、この機能によって有効になるユースケース、および既知の制限事項について説明します。 以下の表を参照して、宛先タイプごとに使用可能な機能を理解します。

| 宛先のタイプ | 配列、マップ、その他のカスタムオブジェクトを書き出す機能 |
|---|---|
| Adobeで作成されたクラウドストレージの宛先（Amazon S3、Azure Blob、Azure Data Lake Storage Gen2、Data Landing Zone、Google Cloud Storage、SFTP） | はい。宛先接続を設定する際に、「配列、マップおよびオブジェクトの書き出しを有効にする」トグルをオンにします。 |
| ファイルベースのメールマーケティング宛先（[!DNL Adobe Campaign]、Oracle Eloqua、Oracle Responsys、Salesforce Marketing Cloud） | × |
| 既存のカスタムパートナーで構築されたクラウドストレージの宛先（Destination SDKで構築されたカスタムファイルベースの宛先） | × |
| エンタープライズ宛先（Amazon Kinesis、Azure Event Hubs、HTTP API） | 部分的に。 アクティベーションワークフローのマッピングステップで、マップタイプオブジェクトを選択して書き出すことができます。 |
| ストリーミング宛先（例：Facebook、Braze、Google Customer Matchなど） | × |
| エッジパーソナライゼーション宛先 | × |

{style="table-layout:auto"}

Experience Platformからの配列、マップ、その他のオブジェクトタイプの書き出しについて知りたい内容については、このページを参照してください。

## 一番下の行を前面へ {#bottom-line}

このセクションの機能に関する最も重要な情報を取得し、詳細については、以下のドキュメントの他のセクションに進んでください。

* クラウドストレージの宛先の場合、配列、マップ、オブジェクトを書き出す機能は、**配列、マップ、オブジェクトを書き出す**&#x200B;切り替えスイッチの選択によって異なります。 詳しくは、[ ページ ](#export-arrays-maps-objects-toggle)を参照してください。
* 配列、マップ、オブジェクトを`JSON`および`Parquet` ファイルのクラウドストレージの宛先に書き出すことができます。 エンタープライズおよびエッジパーソナライゼーションの宛先の場合、書き出されたデータタイプは`JSON`です。 個人と見込み客のオーディエンスはサポートされていますが、アカウントオーディエンスはサポートされていません。
* ファイルベースのクラウドストレージの宛先の場合、*は*&#x200B;配列、マップ、オブジェクトをCSV ファイルに書き出すことができますが、計算フィールド機能を使用し、`array_to_string`関数を使用して文字列に連結することによってのみ可能です。

## Experience Platformの配列やその他のオブジェクトタイプ {#arrays-strings-other-objects}

Experience Platformでは、[XDM スキーマ ](/help/xdm/home.md)を使用して、様々なフィールドタイプを管理できます。 配列エクスポートのサポートが追加される前は、Experience Platformの文字列などの単純なキーと値のペア型フィールドを目的の宛先にエクスポートできました。 以前に書き出しでサポートされていたフィールドの例は`personalEmail.address`:`johndoe@acme.org`です。

Experience Platformのその他のフィールドタイプには、配列フィールドがあります。 Experience Platform UI[での配列フィールドの管理について詳しくは、](/help/xdm/ui/fields/array.md)を参照してください。 次の例のような配列オブジェクトを書き出せるようになりました。

```js
organizations = [{
  id: 123,
  orgName: "Acme Inc",
  founded: 1990,
  latestInteraction: "2024-02-16"
}, {
  id: 456,
  orgName: "Superstar Inc",
  founded: 2004,
  latestInteraction: "2023-08-25"
}, {
  id: 789,
  orgName: 'Energy Corp',
  founded: 2021,
  latestInteraction: "2024-09-08"
}]
```

配列に加えて、Experience Platformから目的のクラウドストレージの宛先にマップやオブジェクトを書き出すこともできます。 Experience Platformの[maps](/help/xdm/ui/fields/map.md)と[ オブジェクト ](/help/xdm/ui/fields/object.md)の詳細をご覧ください。

## 前提条件 {#prerequisites}

[Connect](/help/destinations/ui/connect-destination.md)を目的のクラウドストレージ宛先に接続し、クラウドストレージ宛先[の](/help/destinations/ui/activate-batch-profile-destinations.md) アクティベーション手順を進め、[ マッピング ](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)手順に進みます。 目的のクラウド宛先に接続する場合は、**[!UICONTROL Export arrays, maps, objects]**&#x200B;切り替えをオンにする必要があります。 詳細については、以下の節を参照してください。

>[!NOTE]
>
>エンタープライズおよびエッジパーソナライゼーションの宛先の場合、マップタイプフィールドの書き出しサポートは、**[!UICONTROL Export arrays, maps, objects]** トグルを選択しなくても利用できます。 これらの種類の宛先に接続する場合、この切り替えスイッチは使用できないか、必要ありません。

## 配列、マップ、オブジェクトの書き出し切替スイッチ {#export-arrays-maps-objects-toggle}

>[!CONTEXTUALHELP]
>id="platform_destinations_export_arrays_maps_objects"
>title="配列、マップ、オブジェクトの書き出し"
>abstract="<p> この設定を<b>オン</b>に切り替えると、配列、マップ、オブジェクトを JSON または Parquet ファイルに書き出すことができます。マッピングステップのソースフィールドビューでこれらのオブジェクトタイプを選択できます。切替スイッチをオンにすると、マッピング手順で計算フィールドオプションを使用できません。</p><p>この切替スイッチを<b>オフ</b>にすると、計算フィールドオプションを使用して、オーディエンスをアクティブ化する際に様々なデータ変換関数を適用できます。ただし、配列、マップ、オブジェクトを JSON または Parquet ファイルに書き出すことはでき<i>ない</i>ので、その目的のためには別の宛先を設定する必要があります。</p>"

ファイルベースのクラウドストレージの宛先に接続する場合、**[!UICONTROL Export arrays, maps, objects]**&#x200B;の切り替えをオンまたはオフに設定できます。

![配列、マップ、オブジェクトの書き出しの切り替えスイッチは、オンまたはオフ設定で表示されるほか、ポップオーバーも強調表示されます。](/help/destinations/assets/ui/export-arrays-calculated-fields/export-objects-toggle.gif)

この設定を&#x200B;**オン**&#x200B;に切り替えると、配列、マップ、オブジェクトを JSON または Parquet ファイルに書き出すことができます。クラウドストレージの宛先に対してオーディエンスをアクティブ化する場合、[ マッピング手順](/help/destinations/ui/activate-batch-profile-destinations.md#mapping)のソースフィールドビューで、これらのオブジェクトタイプを選択できます。 ただし、この設定をオンにすると、計算済みフィールド オプションを使用してアクティベーション時にデータを変換することはできません。

この切替スイッチを&#x200B;**オフ**&#x200B;にすると、計算フィールドオプションを使用して、オーディエンスをアクティブ化する際に様々なデータ変換関数を適用できます。ただし、配列、マップ、オブジェクトをJSONまたはParquet ファイルに書き出すことはできず、その目的のために別の宛先を設定する必要があります。

## 配列、マップ、オブジェクトの書き出し切り替え&#x200B;*on* {#export-arrays-maps-objects-toggle-on}

この設定をオンにすると、アクティブ化ワークフローのマッピングステップでソースフィールドセレクターを使用してオブジェクト全体（例：`person.name`）と配列を選択して書き出すことができます。

![ アクティブ化ワークフローのマッピング手順で、ソースフィールドセレクターを使用してオブジェクトを選択します。](/help/destinations/assets/ui/export-arrays-calculated-fields/select-object.gif)

このオプションを選択すると、ユーザーインターフェイスは計算フィールドの使用をユーザーにブロックし、**[!UICONTROL Add calculated fields]** コントロールは次のように無効になります。 データ変換に計算フィールドを使用するには、切り替えスイッチをオフにして宛先接続を設定します。

![計算フィールド コントロールは無効です。](/help/destinations/assets/ui/export-arrays-calculated-fields/calculated-fields-disabled.png)

## 配列、マップ、オブジェクトの書き出し切り替え&#x200B;*off* {#export-arrays-maps-objects-toggle-off}

このオプションを&#x200B;*off*&#x200B;に設定すると、オーディエンスをアクティブ化する際に、「計算フィールド」オプションを使用し、様々なデータ変換関数を適用できます。 ただし、配列、マップ、オブジェクトをJSONまたはParquet ファイルに書き出すことはできず、その目的のために別の宛先を設定する必要があります。

*は、計算フィールド機能を使用して配列、マップ、およびオブジェクトをCSV ファイルに書き出し、*&#x200B;関数を使用して文字列に連結することができます。 `array_to_string`この関数の使用について[詳細](#array-to-string-function-export-arrays)を読む。

計算フィールドを使用して、クラウドストレージの宛先に書き出されたデータに対して[変換を実行する方法について詳しくは、こちらを参照してください](/help/destinations/ui/data-transformations-calculated-fields.md)。

## 書き出されたファイルのサンプル {#sample-exported-files}

この機能を使用すると、データがExperience Platformから構造を保持するParquet ファイルとJSON ファイルを書き出すことができます。 書き出されたJSON ファイルの例を以下に示します。

+++ 選択すると、書き出されたJSON ファイルが表示されます。

```json
{
  "person_name_firstName": "John",
  "person_name_lastName": "Smith",
  "_acmeinc_customer_hs_main_address_scalar": "Oak Avenue No 12",
  "_acmeinc_customer_hs_locations_array": [
    "home address 12",
    "office address 12"
  ],
  "_acmeinc_customer_hs_date_array": [
    "2024-11-14",
    "2024-11-15"
  ],
  "_acmeinc_customer_hs_customer_obj_emails_array0": "john.smith@example.com",
  "_acmeinc_customer_hs_customer_obj": {
    "emails_array": [
      "john.smith@example.com",
      "j.smith@example.com"
    ],
    "name_scalar": "John Smith"
  },
  "_acmeinc_customer_hs_addresses_array_obj": [
    {
      "is_primary": true,
      "streetName_scalar": "Maple Street",
      "streetNo_int": 12
    },
    {
      "is_primary": false,
      "streetName_scalar": "Pine Road",
      "streetNo_int": 45
    }
  ],
  "_acmeinc_customer_hs_addresses_array_obj0": {
    "is_primary": true,
    "streetName_scalar": "Maple Street",
    "streetNo_int": 12
  }
}
```

+++
