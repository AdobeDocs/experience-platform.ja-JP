---
title: Real-Time CDPからクラウドストレージの宛先への配列、マップ、オブジェクトの書き出し
type: Tutorial
description: Real-Time CDPからクラウドストレージの宛先に配列、マップ、オブジェクトを書き出す方法を説明します。
exl-id: ff13d8b7-6287-4315-ba71-094e2270d039
source-git-commit: 6122ddc078101c26061e8662de3fcdcb1cb65992
workflow-type: tm+mt
source-wordcount: '884'
ht-degree: 0%

---

# Real-Time CDPからクラウドストレージの宛先への配列、マップ、オブジェクトの書き出し {#export-arrays-cloud-storage}

>[!AVAILABILITY]
>
>アレイをクラウドストレージの宛先に書き出す機能は、[[!DNL Azure Data Lake Storage Gen2]](../../destinations/catalog/cloud-storage/adls-gen2.md)、[[!DNL Data Landing Zone]](../../destinations/catalog/cloud-storage/data-landing-zone.md)、[[!DNL Google Cloud Storage]](../../destinations/catalog/cloud-storage/google-cloud-storage.md)、[[!DNL Amazon S3]](../../destinations/catalog/cloud-storage/amazon-s3.md)、[[!DNL Azure Blob]](../../destinations/catalog/cloud-storage/azure-blob.md)、[[!DNL SFTP]](../../destinations/catalog/cloud-storage/sftp.md)、

Real-Time CDPから [ クラウドストレージの宛先 ](/help/destinations/catalog/cloud-storage/overview.md) に配列を書き出す方法を説明します。 このドキュメントでは、書き出しワークフロー、この機能で有効になるユースケース、既知の制限事項について説明します。

配列、マップ、その他のオブジェクトタイプをExperience Platformから書き出す方法について知りたい場合は、このページを参照してください。

## ボトムラインを前面に

この節の機能に関する最も重要な情報を取得します。詳細については、このドキュメントの他の節に進みます。

* 配列、マップ、オブジェクトを書き出す機能は、「配列、マップ、オブジェクトを書き出し **切替スイッチの選択によっ** 異なります。 詳しくは、このページの後半 [ を参照してください ](#export-arrays-maps-objects-toggle)。
* 配列、マップおよびオブジェクトは、`JSON` および `Parquet` ファイル内のクラウドストレージの宛先にのみ書き出すことができます。 人物および見込み客オーディエンスはサポートされますが、アカウントオーディエンスはサポートされません。
* 配列、マップ、オブジェクトを CSV ファイルに書き出す *ことができますが* そのためには、計算フィールド機能を使用し、`array_to_string` 関数を使用してそれらを文字列に連結します。

## Platform の配列およびその他のオブジェクトタイプ {#arrays-strings-other-objects}

Experience Platformでは、[XDM スキーマ ](/help/xdm/home.md) を使用して、様々なフィールドタイプを管理できます。 配列の書き出しのサポートを追加する前は、文字列などの単純なキーと値のペアのフィールドをExperience Platformから目的の宛先に書き出すことができました。 以前に書き出し用にサポートされていたフィールドの例は、`personalEmail.address`:`johndoe@acme.org` です。

Experience Platformのその他のフィールドタイプには、配列フィールドが含まれます。 詳しくは、[Experience Platform UI での配列フィールドの管理 ](/help/xdm/ui/fields/array.md) を参照してください。 次の例のような配列オブジェクトを書き出せるようになりました。

```
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

様々な関数を使用して、配列の要素へのアクセス、配列の変換とフィルタリング、配列要素の文字列への結合などを行う方法については、後述の [ 広範な例 ](#examples) を参照してください。

配列に加えて、Experience Platformから目的のクラウドストレージの宛先にマップやオブジェクトを書き出すこともできます。 詳しくは、Experience Platformの [ マップ ](/help/xdm/ui/fields/map.md) および [ オブジェクト ](/help/xdm/ui/fields/object.md) を参照してください。

## 前提条件 {#prerequisites}

[ 接続 ](/help/destinations/ui/connect-destination.md) を目的のクラウドストレージの宛先に対して行い、[ クラウドストレージの宛先のアクティベーション手順 ](/help/destinations/ui/activate-batch-profile-destinations.md) の手順を実行して、[ マッピング ](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) の手順に進みます。 目的のクラウドの宛先に接続する場合は、「配列、マップ、オブジェクトを書き出し **[!UICONTROL をオンに切り替える必要]** あります。 詳しくは、以下の節を参照してください。

## 配列、マップ、オブジェクトの書き出し切替スイッチ {#export-arrays-maps-objects-toggle}

>[!CONTEXTUALHELP]
>id="platform_destinations_export_arrays_maps_objects"
>title="配列、マップ、オブジェクトのエクスポート"
>abstract="<p> この設定 <b> オン </b> を切り替えて、配列、マップ、オブジェクトを JSON ファイルまたは Parquet ファイルに書き出せるようにします。 これらのオブジェクトタイプは、マッピングステップのソースフィールドビューで選択できます。 切替スイッチをオンにすると、マッピングステップの「計算フィールド」オプションを使用できません。</p><p>この切替スイッチ <b> オフ </b> を使用すると、オーディエンスをアクティブ化する際に、計算フィールドオプションを使用して、様々なデータ変換関数を適用できます。 ただし、配列、マップ、オブジェクトを JSON ファイルまたは Parquet ファイルに書き出すことは <i> できません </i>。その場合は、別の宛先を設定する必要があります。</p>"

クラウドストレージの宛先に接続する場合、「配列、マップ、オブジェクトを書き出し **[!UICONTROL をオンまたはオフに設定でき]** す。

![ 配列、マップ、オブジェクトの書き出しは、オンまたはオフの設定で表示され、ポップオーバーをハイライト表示します ](/help/destinations/assets/ui/export-arrays-calculated-fields/export-objects-toggle.gif)。

この設定 **オン** を切り替えて、配列、マップ、オブジェクトを JSON ファイルまたは Parquet ファイルに書き出せるようにします。 クラウドストレージの宛先に対してオーディエンスをアクティブ化する際、[ マッピング手順 ](/help/destinations/ui/activate-batch-profile-destinations.md#mapping) のソースフィールドビューで、これらのオブジェクトタイプを選択できます。 ただし、この設定をオンにすると、「計算フィールド」オプションを使用してアクティブ化時にデータを変換できなくなります。

この切替スイッチ **オフ** を使用すると、オーディエンスをアクティブ化する際に、計算フィールドオプションを使用して、様々なデータ変換関数を適用できます。 ただし、配列、マップ、オブジェクトを JSON ファイルまたは Parquet ファイルに書き出すことはできません。その場合は、別の宛先を設定する必要があります。

## 配列、マップ、オブジェクトの書き出しの切り替え *オン* {#export-arrays-maps-objects-toggle-on}

この設定をオンにすると、アクティベーションワークフローのマッピング手順のソースフィールドセレクターで選択して、オブジェクト全体（`person.name` など）と配列を書き出すことができます。

![ アクティベーションワークフローのマッピング手順のソースフィールドセレクターを使用してオブジェクトを選択します ](/help/destinations/assets/ui/export-arrays-calculated-fields/select-object.gif)。

このオプションを選択すると、ユーザーインターフェイスで計算フィールドの使用がブロックされ、以下に示すように、「**[!UICONTROL 計算フィールドの追加]** コントロールが無効になります。 データ変換に計算フィールドを使用するには、切り替えスイッチをオフにして宛先接続を設定します。

![ 計算フィールドのコントロールが無効。](/help/destinations/assets/ui/export-arrays-calculated-fields/calculated-fields-disabled.png)

## 配列、マップ、オブジェクトの書き出しの切り替え *オフ* {#export-arrays-maps-objects-toggle-off}

このオプションを *off* に設定すると、オーディエンスをアクティブ化する際に、「計算フィールド」オプションを使用して、様々なデータ変換関数を適用できます。 ただし、配列、マップ、オブジェクトを JSON ファイルまたは Parquet ファイルに書き出すことはできません。その場合は、別の宛先を設定する必要があります。

計算フィールド機能を使用して配列 *マップおよびオブジェクトを CSV ファイルに書き出し、`array_to_string` 関数を使用してそれらを文字列に連結すること* できます。 その関数の使用について ](#array-to-string-function-export-arrays) 詳細は、[ こちら。

詳しくは、計算フィールドを使用した [ クラウドストレージの宛先に書き出されたデータに対する変換の実行 ](/help/destinations/ui/data-transformations-calculated-fields.md) の操作を参照してください。

## 書き出されたファイルのサンプル {#sample-exported-files}

この機能を使用すると、データがExperience Platformの構造を保持している Parquet および JSON ファイルを書き出すことができます。 書き出された JSON ファイルの例を以下に示します。

+++ 「」を選択して、書き出された JSON ファイルを表示します。

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