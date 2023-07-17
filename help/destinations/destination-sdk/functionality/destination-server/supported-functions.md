---
description: Experience Platform Destination SDK は、Experience Platform から書き出されたデータを宛先に必須の形式に変換できる、Pebble テンプレートを使用します。
title: Destination SDK でサポートされる変換関数
source-git-commit: d6402f22ff50963b06c849cf31cc25267ba62bb1
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 91%

---


# Destination SDK でサポートされる変換関数

Experience Platform Destination SDK は、Experience Platform から書き出されたデータを宛先に必須の形式に変換できる、[[!DNL Pebble] テンプレート](https://pebbletemplates.io/)を使用します。

Experience Platform [!DNL Pebble] 実装には、[!DNL Pebble] が提供する標準バージョンと比較して、いくつかの変更があります。また、[!DNL Pebble] が提供する標準関数に加えて、アドビは、Destination SDK で使用できるいくつかの追加の関数を作成しています。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## 使用する場所 {#where-to-use}

Experience Platform から宛先に書き出されたデータ用に[メッセージ変換テンプレートを作成](../../testing-api/streaming-destinations/create-template.md)する場合、このページで後述する、サポートされる関数を使用します。

メッセージ変換テンプレートは、ストリーミング宛先の[宛先サーバー設定](templating-specs.md)で使用されます。

## 前提条件 {#prerequisites}

このリファレンスページの概念および関数を理解するには、最初に[メッセージ形式](message-format.md)ドキュメントを参照してください。変換するための [!DNL Pebble] テンプレートおよび書き出されたデータを使用する前に、Experience Platform の[プロファイルの構造](message-format.md#profile-structure)を理解する必要があります。

以下に説明する関数に進む前に、「 」セクションのテンプレートの例を確認してください。 [ID、属性、オーディエンスのメンバーシップ変換にテンプレート言語を使用する](message-format.md#using-templating). そこに記載されている例は、非常にシンプルなものから始まり、だんだん複雑になっていきます。

## サポートされる [!DNL Pebble] 関数 {#supported-functions}

[!DNL Pebble] タグセクションからは、Destination SDK は、以下のみをサポートします。

* [filter](https://pebbletemplates.io/wiki/tag/filter/)
* [for](https://pebbletemplates.io/wiki/tag/for/)
* [if](https://pebbletemplates.io/wiki/tag/if/)
* [set](https://pebbletemplates.io/wiki/tag/set/)

>[!TIP]
>
>テンプレートで&#x200B;*配列*&#x200B;または&#x200B;*マップ*&#x200B;要素を反復する場合、`for` の使用方法が異なります。配列を反復する場合、要素を直接取得できます。マップを反復する場合、各マップエントリ（キーと値のペアを持つ）を取得します。
>
> * 配列要素の例の場合、[identityMap](message-format.md#identities) 名前空間の ID について考慮します。ここでは、`identityMap.gaid` や `identityMap.email` などの要素を反復できます。
> * マッピング要素の例の場合、[segmentMembership](message-format.md#segment-membership) について考慮します。

[!DNL Pebble] フィルターセクションからは、Destination SDK は、すべての関数をサポートします。後述の例では、Destination SDK 内で `date` 関数をどのように使用できるかを示します。

[!DNL Pebble] 関数セクションからは、アドビは [range](https://pebbletemplates.io/wiki/function/range/) 関数をサポート&#x200B;*しません*。

## `date` 関数の使用例 {#date-function}

Destination SDK での [!DNL Pebble] 関数の使用例として、date 関数（[Pebble ドキュメントへのリンク](https://pebbletemplates.io/wiki/filter/date/)）を使用してタイムスタンプの形式を変換する方法について、以下を参照してください。

### ユースケース

Experience Platform が書き出すデフォルトの [ISO 8601](https://ja.wikipedia.org/wiki/ISO_8601) 値から宛先で優先される別の値に `lastQualificationTime` タイムスタンプを変更します。

### 例

#### 入力

```json
{
"lastQualificationTime": "2022-02-08T18:34:24.000+0000"
}
```

#### 形式

```java
{{ lastQualificationTime | date(existingFormat="yyyy-MM-dd'T'HH:mm:sss.SSSX", format="yyyy-MM-dd'T'HH:mm:ssX") }}
```

#### 出力

```json
{
"lastQualificationTime": "2022-02-21T18:34:24Z"
}
```

## アドビが追加した関数 {#functions-added-by-adobe}

[!DNL Pebble] が提供する標準関数に加えて、データ書き出しに使用できる、アドビが作成した追加の関数について、以下を参照してください。

### `addedSegments` および `removedSegments` 関数 {#addedsegments-removedsegments-functions}

#### ユースケース

これらの関数は、プロファイルに追加またはプロファイルから削除されたオーディエンスのリストを取得するために使用できます。

#### 例

##### 入力

```json
{
  "identityMap": {
    "myIdNamespace": [
      {
        "id": "external_id1"
      },
      {
        "id": "external_id2"
      }
    ]
  },
  "segmentMembership": {
    "ups": {
      "111111": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "realized"
      },
      "222222": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "exited"
      },
      "333333": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "realized"
      }
    }
  }
}
```

##### 形式

```java
added: {% for s in addedSegments(segmentMembership.ups) %}<{{s.key}}>{% endfor %}; removed: {% for s in removedSegments(segmentMembership.ups) %}<{{s.key}}>{% endfor %}
```

##### 出力

```json
added: <111111><333333>; removed: <222222>
```

<!--

### Added and removed audiences filters {#added-and-removed-segmnts-filters}

#### Use case {#use-case}

These filters are similar to `addedSegments` and `removedSegments`, described above. The only difference is that they are implemented as filters as opposed to functions.

#### Example {#example}

##### Input {#input}

```json
{
  "identityMap": {
    "myIdNamespace": [
      {
        "id": "external_id1"
      },
      {
        "id": "external_id2"
      }
    ]
  },
  "segmentMembership": {
    "ups": {
      "111111": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "realized"
      },
      "222222": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "exited"
      },
      "333333": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "realized"
      }
    }
  }
}
```

##### Format {#format}

```java
added: {% for s in input.profile.segmentMembership.ups | added %}<{{s.key}}>{% endfor %};|removed: {% for s in input.profile.segmentMembership.ups | removed %}<{{s.key}}>{% endfor %};
```

##### Output {#output}

```json
added: <111111><333333>;|removed: <222222>;
```

-->

## 次の手順 {#next-steps}

Destination SDK でサポートされる [!DNL Pebble] 関数と、それらを使用して、書き出されたデータの形式をニーズに合わせて調整する方法について確認しました。次に、以下のページを確認する必要があります。

* [メッセージ変換テンプレートの作成とテスト](../../testing-api/streaming-destinations/create-template.md)
* [テンプレートレンダリング API の操作](../../testing-api/streaming-destinations/render-template-api.md)
