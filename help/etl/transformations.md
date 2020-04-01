---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: ETL変換の例
topic: overview
translation-type: tm+mt
source-git-commit: 4817162fe2b7cbf4ae4c1ed325db2af31da5b5d3

---


# ETL変換の例

この記事では、抽出、変換、読み込み(ETL)開発者が直面する変換の例を示します。

## 階層へのCSVのフラット化

### サンプルファイル

サンプルのCSVおよびJSONファイルは、アドビが管理するパブリックETLリファレンスGitHubリポジトリから入手できます。

- [CRM_プロファイル.csv](https://github.com/adobe/experience-platform-etl-reference/blob/master/example_files/CRM_profiles.csv)
- [CRM_プロファイル.json](https://github.com/adobe/experience-platform-etl-reference/blob/master/example_files/CRM_profiles.json)

### CSVの例

次のCRMデータは、次の形式でエクスポートされまし `CRM_profiles.csv`た：

```shell
TITLE   F_NAME  L_NAME  GENDER  DOB EMAIL   CRMID   ECID    LOYALTYID   ECID2   PHONE   STREET  CITY    STATE   COUNTRY ZIP LAT LONG
Mr  Ewart   Bennedsen   M   2004-09-25  ebennedsenex@jiathis.com    71a16013-d805-7ece-9ac4-8f2cd66e8eaa    87098882279810196101440938110216748923  2e33192000007456-0365c00000000000   55019962992006103186215643814973128178  256-284-7231    72 Buhler Crossing  Anniston    Alabama US  36205   33.708276   -85.7922905
Dr  Novelia Ansteys F   1987-10-31  nansteysdk@spotify.com  2eeb6532-82e1-0d58-8955-bf97de66a6f5    50829196174854544323574004005273946998  2e3319208000765b-3811c00000000001   65233136134594262632703695260919939885  704-181-6371    79 Northfield Hill  Charlotte   North Carolina  US  28299   35.2188655  -80.8108885
Mr  Ulises  Mochan  M   1996-03-20  umochanco@gnu.org   6f393075-addb-bdd6-73f8-31c393b700f5    70086119428645095847094710218289660855  2e33192080003023-26b2000000000002   82011353387947708954389153068944017636  720-837-4159    00671 Mifflin Trail Lacolle Qu√©bec CA  E5A 45.08338    -73.36585
Mrs Friederike  Durrell F   1979-01-3   fdurrellbj@utexas.edu   33d018ec-5fed-f1a3-56aa-079370a9511b    50164729868919217963697788808932473456  2e33192080006dfc-0cdf400000000003   64452712468609735658703639722261004071  798-528-3458    47 Fremont Hill Independencia   Veracruz Llave  MX  91891   19.3803931  -99.1476905
Rev Evita   Bingall F   1974-02-28  ebingallod@mac.com  8c93db88-f328-8efb-dc73-d5654d371cbe    74973364195185450328832136951985519627  2e331920800038db-0559e00000000004   58945501950285346322834356669253860483  397-178-5897    56 Crescent Oaks Court  Buenavista  Oaxaca  MX  71730   19.4458447  -99.1497665
Mr  Eugenie Bechley F   1969-05-19  ebechley9r@telegraph.co.uk  b0c76a3f-6526-0ad0-e050-48143b687d18    67119779213799783658184754966135750376  2e331920800001a4-24b2800000000005   59715249079109455676103900762283358508  718-374-7456    5760 Southridge Junction    Staten Island   New York    US  10310   40.6307451  -74.1181235
Dr  Cammi   Haslen  F   1973-12-17  chaslenqv@ehow.com  56059cd5-5006-ce5f-2f5f-15b4d856a204    61747117963243728095047674165570746095  2e33192080007c25-2ec0600000000006   86268258269066295956223980330791223320  865-538-8291    83 Veith Street Knoxville   Tennessee   US  37995   35.95   -84.05
```

### マッピング

CRMデータのマッピング要件を次の表に示し、次の変換を含めます。
- プロパティの識別 `identityMap` 列
- 生年月日(DOB)から年月日まで
- 文字列を重複または短い整数に変換します。

| CSV列 | XDMパス | データの形式設定 |
| ---------- | -------- | --------------- |
| タイトル | person.name.cortecyTitle | 文字列としてコピー |
| F_NAME | person.name.firstName | 文字列としてコピー |
| L_NAME | person.name.lastName | 文字列としてコピー |
| 性別 | person.gender | 性別を対応するperson.gender列挙値に変換 |
| DOB | person.birthDayAndMonth:&quot;MM-DD&quot;<br/>person.birthDate:&quot;YYYY-MM-DD&quot;<br/>person.birthYear:YYYY | Transform birthDayAndMonth as<br/>stringTransform birthDate as<br/>stringTransform birthYear as short int |
| EMAIL | personalEmail.address | 文字列としてコピー |
| CRMID | identityMap.CRMID[{&quot;id&quot;:x, primary:false}] | identityMapのCRMID配列に文字列としてコピーし、Primaryをfalseに設定します。 |
| ECID | identityMap.ECID[{&quot;id&quot;:x, primary:false}] | identityMapのECID配列の最初のエントリに文字列としてコピーし、Primaryをfalseに設定します。 |
| LOYALTYID | identityMap.LOYALTYID[{&quot;id&quot;:x, primary:true}] | identityMapのLOYALTYID配列に文字列としてコピーし、Primaryをtrueに設定します。 |
| ECID2 | identityMap.ECID[{&quot;id&quot;:x, primary:false}] | identityMapのECID配列の2番目のエントリに文字列としてコピーし、Primaryをfalseに設定します。 |
| 電話 | homePhone.number | 文字列としてコピー |
| 通り | homeAddress.street1 | 文字列としてコピー |
| 市区町村 | homeAddress.city | 文字列としてコピー |
| 州 | homeAddress.stateProvince | 文字列としてコピー |
| 国 | homeAddress.country | 文字列としてコピー |
| ZIP | homeAddress.postalCode | 文字列としてコピー |
| LAT | homeAddress.latitude | 変換重複 |
| LONG | homeAddress.longitude | 変換重複 |


### Output XDM

次のサンプルは、に示すように、CSVの最初の2行をXDMに変換したものです `CRM_profiles.json`。

```json
{
   "person": {
      "name": {
         "courtesyTitle": "Mr",
         "firstName": "Ewart",
         "lastName": "Bennedsen"
      },
      "gender": "male",
      "birthDayAndMonth": "09-25",
      "birthDate": "2004-09-25",
      "birthYear": 2004
   },
   "identityMap": {
      "CRMID": [{
         "id": "71a16013-d805-7ece-9ac4-8f2cd66e8eaa",
         "primary": false
      }],
      "ECID": [{
         "id": "87098882279810196101440938110216748923",
         "primary": false
      },
      {
         "id": "55019962992006103186215643814973128178",
         "primary": false
      }],
      "LOYALTYID": [{
         "id": "2e33192000007456-0365c00000000000",
         "primary": true
      }]
   },
   "homePhone": {
      "number": "256-284-7231"
   },
   "personalEmail": {
      "address": "ebennedsenex@jiathis.com"
   },
   "homeAddress": {
      "street1": "72 Buhler Crossing",
      "city": "Anniston",
      "stateProvince": "Alabama",
      "country": "US",
      "postalCode": "36205",
      "_schema": {
         "latitude": 33.708276,
         "longitude": -85.7922905
      }
   }
},{
   "person": {
      "name": {
         "courtesyTitle": "Dr",
         "firstName": "Novelia",
         "lastName": "Ansteys"
      },
      "gender": "female",
      "birthDayAndMonth": "10-31",
      "birthDate": "1987-10-31",
      "birthYear": 1987
   },
   "identityMap": {
      "CRMID": [{
         "id": "2eeb6532-82e1-0d58-8955-bf97de66a6f5",
         "primary": false
      }],
      "ECID": [{
         "id": "50829196174854544323574004005273946998",
         "primary": false
      },
      {
         "id": "65233136134594262632703695260919939885",
         "primary": false
      }],
      "LOYALTYID": [{
         "id": "2e3319208000765b-3811c00000000001",
         "primary": true
      }]
   },
   "homePhone": {
      "number": "704-181-6371"
   },
   "personalEmail": {
      "address": "nansteysdk@spotify.com"
   },
   "homeAddress": {
      "street1": "79 Northfield Hill",
      "city": "Charlotte",
      "stateProvince": "North Carolina",
      "country": "US",
      "postalCode": "28299",
      "_schema": {
         "latitude": 35.2188655,
         "longitude": -80.8108888
      }
   }
}
```

## 「Dataframe to XDM」スキーマ

データフレーム（Parketファイルなど）の階層は、アップロード先のXDMスキーマの階層と一致する必要があります。

### データフレームの例

次の例のデータフレームの構造は、XDM個々のプロファイルクラスを実装するスキーマにマップされ、そのタイプのスキーマに関連付けられた最も一般的なフィールドが含まれています。

```python
[
    StructField("person", StructType(
        [
            StructField("name", StructType(
                [
                    StructField("courtesyTitle", StringType()),
                    StructField("firstName", StringType()),
                    StructField("lastName", StringType())
                ]
            )),
            StructField("gender", StringType()),
            StructField("birthDayAndMonth", StringType()),
            StructField("birthDate", StringType()),
            StructField("birthYear", LongType())
        ]
    )),
    StructField("identityMap", MapType(
        StructField("CRMID", ArrayType(
            StructType(
                [
                    StructField("id", StringType()),
                    StructField("primary", BooleanType())
                ]
            )
        )),
        StructField("ECID", ArrayType(
            StructType(
                [
                    StructField("id", StringType()),
                    StructField("primary", BooleanType())
                ]
            )
        )),
        StructField("LOYALTYID", ArrayType(
            StructType(
                [
                    StructField("id", StringType()),
                    StructField("primary", BooleanType())
                ]
            )
        ))
    )),
    StructField("homePhone", StructType(
        [
            StructField("number", StringType())
        ]
    )),
    StructField("personalEmail", StructType(
        [
            StructField("address", StringType())
        ]
    )),
    StructField("homeAddress", StructType(
        [
            StructField("street1", StringType()),
            StructField("city", StringType()),
            StructField("stateProvince", StringType()),
            StructField("country", StringType()),
            StructField("postalCode", StringType()),
            StructField("_schema", StructType(
                [
                    StructField("latitude", DoubleType()),
                    StructField("latitude", DoubleType()),
                ]
            ))
        ]
    ))    
]
```

Adobe Experience Platformで使用するデータフレームを作成する場合は、フィールドが正しくマップされるように、階層構造が既存のXDMスキーマと完全に一致するようにすることが重要です。

## IDマップ

### IDの配列

```json
[
  {
    "xdm:id": "someone1@example.com",
    "xdm:namespace": {
      "xdm:code": "Email"
    }
  },
  {
    "xdm:id": "2eeb6532-82e1-0d58-8955-bf97de66a6f5",
    "xdm:namespace": {
      "xdm:code": "CRMID"
    }
  },
  {
    "xdm:id": "2e3319208000765b-3811c00000000001",
    "xdm:namespace": {
      "xdm:code": "LOYALTYID"
    }
  }
]
```

### マッピング

次の表に、IDの配列のマッピング要件を示します。

| IDフィールド | identityMapフィールド | データタイプ |
| -------------- | ----------------- | --------- |
| identitys[0].id | identityMapEmail[][{"id"}] | 文字列としてコピー |
| identitys[1].id | identityMapCRMID[][{"id"}] | 文字列としてコピー |
| identitys[2].id | identityMapLOYALTYID[][{"id"}] | 文字列としてコピー |

### Output XDM

XDMに変換されたIDの配列を以下に示します。

```JSON
"identityMap": {
      "Email": [{
         "id": "someone1@example.com"
      }],
      "CRMID": [{
         "id": "2eeb6532-82e1-0d58-8955-bf97de66a6f5"
      }],
      "LOYALTYID": [{
         "id": "2e3319208000765b-3811c00000000001"
      }]
   }
```

