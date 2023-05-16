---
description: Experience PlatformDestination SDKでは、ペブルテンプレートを使用するので、Experience Platformから書き出されたデータを、宛先で必要な形式に変換できます。
title: Destination SDKでサポートされる変換関数
source-git-commit: ab87a2b7190a0365729ba7bad472fde7a489ec02
workflow-type: tm+mt
source-wordcount: '579'
ht-degree: 5%

---


# Destination SDKでサポートされる変換関数

Experience PlatformDestination SDKの用途 [[!DNL Pebble] テンプレート](https://pebbletemplates.io/)を使用すると、Experience Platformから書き出されたデータを、宛先で必要な形式に変換できます。

Experience Platform [!DNL Pebble] の標準バージョンとは異なり、実装にはいくつかの変更があります。 [!DNL Pebble]. また、次のような標準の関数に加えて、 [!DNL Pebble]Adobeは、Destination SDKで使用できる関数をいくつか作成しました。

>[!IMPORTANT]
>
>Destination SDKでサポートされるすべてのパラメーター名と値は **大文字と小文字を区別**. 大文字と小文字の区別に関するエラーを避けるには、ドキュメントに示すように、パラメーターの名前と値を正確に使用してください。

## 使用場所 {#where-to-use}

このページで後述する、サポートされる関数を使用すると、 [メッセージ変換テンプレートの作成](../../testing-api/streaming-destinations/create-template.md) 宛先に書き出されたExperience Platformの

メッセージ変換テンプレートは、 [宛先サーバーの設定](templating-specs.md) ストリーミング先用

## 前提条件 {#prerequisites}

このリファレンスページの概念と機能を理解するには、 [メッセージ形式](message-format.md) 最初にドキュメントを作成します。 次を理解する必要があります。 [プロファイルの構造](message-format.md#profile-structure) Experience Platformで [!DNL Pebble] 変換するテンプレートと、書き出されたデータ。

以下に説明する関数に進む前に、「 」セクションのテンプレートの例を確認してください。 [ID、属性、セグメントメンバーシップの変換にテンプレート言語を使用する](message-format.md#using-templating). この例は非常に単純で複雑なものから始まります

## サポート [!DNL Pebble] 関数 {#supported-functions}

次の [!DNL Pebble] タグセクション、Destination SDKは次の項目のみをサポートします。

* [フィルター](https://pebbletemplates.io/wiki/tag/filter/)
* [for](https://pebbletemplates.io/wiki/tag/for/)
* [if](https://pebbletemplates.io/wiki/tag/if/)
* [set](https://pebbletemplates.io/wiki/tag/set/)

>[!TIP]
>
>使用 `for` は、 *配列* または *マップ* 要素を作成します。 配列を繰り返し処理する場合、要素を直接取得できます。 マップを繰り返し処理すると、キーと値のペアを持つ各マップエントリが得られます。
>
> * 配列要素の例を考えてみましょう。 [identityMap](message-format.md#identities) 名前空間では、次のような要素を繰り返し処理できます。 `identityMap.gaid`, `identityMap.email`、または類似したもの
> * map 要素の例については、次の点を考えてください。 [segmentMembership](message-format.md#segment-membership).


次の [!DNL Pebble] フィルタセクション、Destination SDKは、すべての関数をサポートします。 次の例は、 `date` 関数は、Destination SDK内で使用できます。

次の [!DNL Pebble] 関数セクション、Adobe *not* 支持する [範囲](https://pebbletemplates.io/wiki/function/range/) 関数に置き換えます。

## 例 `date` 関数が使用される {#date-function}

例を示すには [!DNL Pebble] 関数は、Destination SDKで使用されます。[Pebble ドキュメント内のリンク](https://pebbletemplates.io/wiki/filter/date/)) は、タイムスタンプの形式を変換するために使用されます。

### ユースケース

次を変更する： `lastQualificationTime` デフォルトのタイムスタンプ [ISO 8601](https://ja.wikipedia.org/wiki/ISO_8601) の値を指定します。Experience Platformが、宛先で優先される別の値に書き出します。

### 例

#### 必要情報

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

## Adobe {#functions-added-by-adobe}

次に示す既製の関数に加えて、 [!DNL Pebble]を参照してください。データのエクスポートに使用できる、Adobeで作成された追加の関数を以下に示します。

### `addedSegments` および `removedSegments` 関数 {#addedsegments-removedsegments-functions}

#### ユースケース

これらの関数は、プロファイルに追加またはプロファイルから削除されたセグメントのリストを取得するために使用できます。

#### 例

##### 必要情報

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

### Added and removed segments filters {#added-and-removed-segmnts-filters}

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

これでどちらか分かりました [!DNL Pebble] 関数は、Destination SDKでサポートされているほか、関数を使用して、必要に応じて書き出されるデータの形式を調整する方法もサポートされています。 次に、次のページを確認します。

* [メッセージ変換テンプレートの作成とテスト](../../testing-api/streaming-destinations/create-template.md)
* [テンプレートレンダリング API の操作](../../testing-api/streaming-destinations/render-template-api.md)
