---
title: 拡張機能マニフェスト
description: 拡張機能の適切な使用方法を Adobe Experience Platform に知らせる JSON マニフェストファイルの設定方法について説明します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: tm+mt
source-wordcount: '2647'
ht-degree: 99%

---

# 拡張機能マニフェスト

>[!NOTE]
>
>Adobe Experience Platform Launchは、Adobe Experience Platformのデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../term-updates.md)を参照してください。

拡張機能の基本ディレクトリには、`extension.json` という名前のファイルを作成する必要があります。 このファイルには、Adobe Experience Platform が拡張機能を適切に使用できるようにするための重要な詳細が含まれています。 内容の一部は、[npm の`package.json`](https://docs.npmjs.com/files/package.json) の形式に従って構成されます。

`extension.json` の例は、[Hello World 拡張機能](https://github.com/adobe/reactor-helloworld-extension/blob/master/extension.json) の GitHub リポジトリにあります。

拡張機能マニフェストは、次のプロパティで構成する必要があります。

| プロパティ | 説明 |
| --- | --- |
| `name` | 拡張機能の名前。他のすべての 拡張機能とは異なる名前を使用し、[命名規則](#naming-rules)に従う必要があります。 **これは、タグで識別子として使用されます。拡張機能を公開した後は変更しないでください。** |
| `platform` | 拡張機能のプラットフォーム。 現時点で使用できる値は `web` のみです。 |
| `version` | 拡張機能のバージョン。 [semver](http://semver.org/) のバージョニング形式に従う必要があります。 これは、[npm バージョンフィールド](https://docs.npmjs.com/files/package.json#version)と一致します。 |
| `displayName` | 人間が判読できる、拡張機能の名前。これは、Platform ユーザーに表示されます。「タグ」や「拡張機能」に言及する必要はありません。ユーザーは、タグ拡張機能が表示されることを既に知っています。 |
| `description` | 拡張機能の説明。これは、Platform ユーザーに表示されます。拡張機能によってユーザーが Web サイトに製品を実装できるようになる場合は、製品の動作を説明します。 「タグ」や「拡張機能」に言及する必要はありません。ユーザーは、タグ拡張機能が表示されることを既に知っています。 |
| `iconPath` *（オプション）* | 拡張機能用に表示されるアイコンの相対パス。 スラッシュで始めることはできません。拡張子 `.svg` が付いた SVG ファイルを参照する必要があります。 SVG は正方形にする必要があり、Platform で拡大縮小できます。 |
| `author` | 「author」は、次の構造を持つオブジェクトです。 <ul><li>`name`：拡張機能の作成者の名前。または、会社名を使用することもできます。</li><li>`url` *（オプション）*：拡張機能の作成者の詳細を参照できる URL。</li><li>`email` *（オプション）*：拡張機能の作成者のメールアドレス。</li></ul>この構造は、[npm 作成者フィールド](https://docs.npmjs.com/files/package.json#people-fields-author-contributors)のルールと一致します。 |
| `exchangeUrl` *（公開拡張機能に必要）* | Adobe Exchange での拡張機能のリストへの URL。パターン `https://www.adobeexchange.com/experiencecloud.details.######.html` と一致する必要があります。 |
| `viewBasePath` | すべてのビューおよびビュー関連のリソース（HTML、JavaScript、CSS、画像）を含むサブディレクトリの相対パス。 Platform は、このディレクトリを Web サーバー上でホストし、このディレクトリから iframe コンテンツを読み込みます。 これは必須フィールドで、先頭にスラッシュを使用することはできません。例えば、すべてのビューが `src/view/` 内に含まれている場合、`viewBasePath` の値は `src/view/` になります。 |
| `hostedLibFiles` *（オプション）* | ユーザーの多くは、タグ関連のすべてのファイルを自社サーバーでホストすることを好みます。 これにより、ユーザーは実行時のファイルの可用性に関する確実性を高めることができ、コードを簡単にスキャンしてセキュリティの脆弱性を調べることができます。 実行時に拡張機能のライブラリ部分で JavaScript ファイルを読み込む必要がある場合は、このプロパティを使用して、これらのファイルをリストすることをお勧めします。 リストに含まれるファイルは、タグのランタイムライブラリと一緒にホストされます。 その後、拡張機能では、[getHostedLibFileUrl](./turbine.md#get-hosted-lib-file) メソッドで取得した URL 経由でファイルを読み込むことができます。<br><br>このオプションには、ホストする必要があるサードパーティのライブラリファイルの相対パスを使用した配列が含まれます。 |
| `main` *（オプション）* | 実行時に実行する必要があるライブラリモジュールの相対パス。<br><br>このモジュールは常にランタイムライブラリに含まれ、実行されます。このモジュールは常にランタイムライブラリに含まれるため、モジュールが必須の場合は「メイン」モジュールのみを使用し、コードサイズを最小限に抑えることをお勧めします。<br><br>このモジュールが最初に実行される保証はありません。その前に他のモジュールが実行される場合があります。 |
| `configuration` *（オプション）* | 拡張機能の[拡張機能設定](./configuration.md)の部分について説明します。ユーザーが拡張機能のグローバル設定を指定する必要がある場合に必要です。 このフィールドの構造化方法の詳細については、「[付録](#config-object)」を参照してください。 |
| `events` *（オプション）* | [イベント](./web/event-types.md)タイプの定義の配列。 配列内の各オブジェクトの構造については、[タイプの定義](#type-definitions)に関する「付録」セクションを参照してください。 |
| `conditions` *（オプション）* | [条件](./web/condition-types.md)タイプの定義の配列。 配列内の各オブジェクトの構造については、[タイプの定義](#type-definitions)に関する「付録」セクションを参照してください。 |
| `actions` *（オプション）* | [アクション](./web/action-types.md)タイプの定義の配列。 配列内の各オブジェクトの構造については、[タイプの定義](#type-definitions)に関する「付録」セクションを参照してください。 |
| `dataElements` *（オプション）* | [データ要素](./web/data-element-types.md)タイプの定義の配列。 配列内の各オブジェクトの構造については、[タイプの定義](#type-definitions)に関する「付録」セクションを参照してください。 |
| `sharedModules` *（オプション）* | 共有モジュール定義オブジェクトの配列。 配列内の共有モジュールオブジェクトは、それぞれ次のように構造化する必要があります。 <ul><li>`name`：共有モジュールの名前。「[共有モジュール](./web/shared.md)」で説明されているように、この名前は、他の拡張機能から共有モジュールを参照する際に使用されることに注意してください。この名前は、どのユーザーインターフェイスにも表示されません。 拡張機能内の他の共有モジュールの名前とは重複しない一意の名前を使用し、[命名規則](#naming-rules)に従う必要があります。 **これは、タグで識別子として使用されます。拡張機能を公開した後は変更しないでください。**</li><li>`libPath`：共有モジュールの相対パス。スラッシュで始めることはできません。拡張子 `.js` が付いた JavaScript ファイルを参照する必要があります。</li></ul> |

## 付録

### 命名規則 {#naming-rules}

`extension.json` 内の `name` フィールドの値は、次のルールに従う必要があります。

* 214 文字以下にする必要があります。
* ドットまたはアンダースコアを先頭に使用することはできません。
* 大文字を含めることはできません。
* URL に使用できる文字のみを含める必要があります。

これらのルールは、[npm パッケージ名](https://docs.npmjs.com/files/package.json#name)ルールと一致します。

### 設定オブジェクトのプロパティ {#config-object}

設定オブジェクトは、次のように構造化する必要があります。

<table>
  <thead>
    <tr>
      <th>プロパティ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>viewPath</code></td>
      <td>拡張機能の設定表示の相対 URL。<code>viewBasePath</code> に対する相対位置であり、スラッシュで始まらないようにします。 拡張子 <code>.html</code> を持つ HTML ファイルを参照する必要があります。 クエリ文字列およびフラグメント識別子（ハッシュ）のサフィックスを使用できます。</td>
    </tr>
    <tr>
      <td><code>schema</code></td>
      <td>拡張機能の設定表示から保存される有効なオブジェクトの形式を記述する <a href="http://json-schema.org/">JSON スキーマ</a> のオブジェクト。 設定表示の開発者は、保存した settings オブジェクトがこのスキーマと一致することを確認する必要があります。このスキーマは、ユーザーが Platform サービスを使用してデータを保存しようとした場合の検証にも使用されます。<br><br>スキーマオブジェクトの例を次に示します。
<pre class="JSON language-JSON hljs">
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "properties": {
    "delay": {
      "type": "number",
      "minimum": 1
    }
  },
  "required": [
    "delay"
  ],
  "additionalProperties": false
}
</pre>
      手動でスキーマをテストするには、<a href="http://www.jsonschemavalidator.net/">JSON Schema validator</a> などのツールを使用することをお勧めします。</td>
    </tr>
    <tr>
      <td><code>transforms</code> <em>（オプション）</em></td>
      <td>オブジェクトの配列。各オブジェクトは、ランタイムライブラリに発行されるときに、対応するすべての settings オブジェクトで実行する必要がある変換を表します。 このプロパティが必要となる理由とその使い方の詳細ついては、<a href="#transforms">変換</a>の節を参照してください。</td>
    </tr>
  </tbody>
</table>

### タイプの定義 {#type-definitions}

タイプの定義とは、イベント、条件、アクションまたはデータ要素の各タイプを記述するために使用されるオブジェクトです。 このオブジェクトは、次の内容で構成されます。

<table>
  <thead>
    <tr>
      <th>プロパティ</th>
      <th>説明</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>name</code></td>
      <td>タイプの名前。拡張機能の中で一意の名前を指定する必要があります。名前は<a href="#naming-rules">名前付けルール</a>に従う必要があります。 <strong>これは、タグで識別子として使用されます。拡張機能を公開した後は変更しないでください。</strong></td>
    </tr>
    <tr>
      <td><code>displayName</code></td>
      <td>データ収集ユーザーインターフェイス内のタイプを表すために使用されるテキストです。 人間が判読できる必要があります。</td>
    </tr>
    <tr>
      <td><code>categoryName</code> <em>（オプション）</em></td>
      <td>指定された場合、<code>displayName</code> がデータ収集 UI 内の <code>categoryName</code> の下に一覧表示されます。 同じ <code>categoryName</code> を持つすべてのタイプが同じカテゴリに一覧表示されます。例えば、拡張機能で <code>keyUp</code> イベントタイプと <code>keyDown</code> イベントタイプが指定され、どちらのタイプも <code>Keyboard</code> の <code>categoryName</code> を持つ場合、ユーザーがルールを作成する際に使用可能なイベントタイプのリストから選択するときに、両方のイベントタイプが Keyboard カテゴリに表示されます。 <code>categoryName</code> の値は人間が判読できる必要があります。</td>
    </tr>
    <tr>
      <td><code>libPath</code></td>
      <td>タイプのライブラリモジュールの相対パスです。スラッシュで始めることはできません。拡張子 <code>.js</code> を持つ JavaScript ファイルを参照する必要があります。</td>
    </tr>
    <tr>
      <td><code>viewPath</code> <em>（オプション）</em></td>
      <td>タイプの表示の相対 URL。<code>viewBasePath</code> に対する相対 URL であり、スラッシュで始まらないようにする必要があります。 拡張子 <code>.html</code> を持つ HTML ファイルを参照する必要があります。 クエリ文字列とフラグメント識別子（ハッシュ）を使用できます。タイプのライブラリモジュールがユーザーの設定を使用しない場合、このプロパティを除外すると、Platform では、代わりに、設定が不要であることを示すプレースホルダーが表示されます。</td>
    </tr>
    <tr>
      <td><code>schema</code></td>
      <td>ユーザーが保存できる有効な settings オブジェクトの形式を記述する <a href="http://json-schema.org/">JSON スキーマ</a> のオブジェクト。 通常、設定はユーザーがデータ収集ユーザーインターフェイスを使用して設定および保存します。 このような場合、拡張機能の表示では、ユーザーが指定した設定を検証するために必要な手順を実行できます。 一方で、ユーザーインターフェイスを使用せずに、タグ API を直接使用するユーザーも存在します。このスキーマの目的は、ユーザーインターフェイスが使用されているかどうかに関係なく、ユーザーが保存する settings オブジェクトが、実行時に settings オブジェクト対して実行されるライブラリモジュールと互換性のある形式であることを、Platform が適切に検証できるようにすることです。<br><br>スキーマオブジェクトの例を次に示します。<br>
<pre class="JSON language-JSON hljs">
{
  "$schema":"http://json-schema.org/draft-04/schema#",
  "type":"object",
  "properties":{
    "delay":{
      "type":"数値",
      "minimum":3
    }
  },
  "required":[
    "delay"
  ]
  "additionalProperties":false
}
</pre>
      手動でスキーマをテストするには、<a href="http://www.jsonschemavalidator.net/">JSON Schema validator</a> などのツールを使用することをお勧めします。</td>
    </tr>
    <tr>
      <td><code>transforms</code> <em>（オプション）</em></td>
      <td>オブジェクトの配列。各オブジェクトは、ランタイムライブラリに発行されるときに、対応するすべての settings オブジェクトで実行する必要がある変換を表します。 このプロパティが必要となる理由とその使い方の詳細については、<a href="#transforms">変換</a>の節を参照してください。</td>
    </tr>
  </tbody>
</table>

### 変換 {#transforms}

特定のユースケースでは、拡張機能を使用するには、表示から保存された settings オブジェクトを Platform で変換してから、タグランタイムライブラリに発行する必要があります。 `extension.json` 内でタイプ定義を定義する際に、`transforms` プロパティを設定することで、これらの 1 つ以上の変換を実行するように要求できます。 `transforms` プロパティはオブジェクトの配列です。各オブジェクトは、実行する必要のある変換を表します。

すべての変換には `type` と `propertyPath` が必要です。 `type` は、`function`、`remove`、`file` のいずれかである必要があり、Platform が settings オブジェクトに適用する必要がある変換を示します。`propertyPath` はピリオドで区切られた文字列で、変更が必要なプロパティが settings オブジェクト内のどこにあるかをタグに伝えます。次に、設定オブジェクトと `propertyPath` の例を示します。

```js
{
  foo: {
    bar: "A string",
    baz: [
      "A",
      "B",
      "C"
    ]
  }
}
```

* `foo.bar` の `propertyPath` を設定すると、`"A string"` 値が変換されます。
* `foo.baz[]` の `propertyPath` を設定すると、`baz` 配列の各値が変換されます。
* `foo.baz` の `propertyPath` を設定すると、`baz` 配列が変換されます。

プロパティパスは、配列とオブジェクト表記の組み合わせを使用して、 設定オブジェクトの任意のレベルで変換を適用できます。

>[!WARNING]
>
>`propertyPath` 属性での配列表記の使用（例：`foo.baz[]`）は、拡張機能サンドボックス*ツールではまだサポートされていません。

以下の節では、利用可能な変換とその使用方法について説明します。

#### 関数変換

関数変換を使用すると、Platform ユーザーが記述したコードを、発行されたタグランタイムライブラリ内のライブラリモジュールによって実行できます。

「カスタムスクリプト」アクションタイプを指定するとします。 「カスタムスクリプト」アクションビューには、ユーザーがコードを入力できるテキスト領域があります。ユーザーがテキスト領域に次のコードを入力したとします。

`console.log('Welcome, ' + username +'. This is ZomboCom.');`

ユーザーがルールを保存すると、表示で保存される設定オブジェクトは次のようになります。

```javascript
{
  foo: {
    bar: "console.log('Welcome, ' + username +'. This is ZomboCom.');"
  }
}
```

タグランタイムライブラリ内でアクションを使用するルールが起動した場合、ユーザーのコードを実行し、ユーザー名を渡します。

設定オブジェクトがアクションタイプのビューから保存された時点で、ユーザーのコードは単なる文字列になります。 この動作には、JSON との間でコードを適切にシリアル化できるため利点もありますが、通常、コードは実行可能な関数ではなく、文字列としてタグのランタイムライブラリに発行されるため、欠点もあります。[`eval`](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/eval) または [Function コンストラクター](https://developer.mozilla.org/ja-JP/docs/Web/JavaScript/Reference/Global_Objects/Function)を使用して、アクションタイプのライブラリモジュール内のコードを実行しようとしても、[コンテンツのセキュリティポリシー](https://developer.mozilla.org/ja-JP/docs/Web/Security/CSP)によって実行がブロックされる可能性があるので、この操作は推奨されません。

この状況の回避策として、タグランタイムライブラリでユーザーのコードが発行されたときに、関数変換を使用して、そのコードを実行可能な関数に含めるよう Platform に指示します。 この例の問題を解決するには、`extension.json` でタイプの定義の変換を次のように定義します。

```json
{
  "transforms": [
    {
      "type": "function",
      "propertyPath": "foo.bar",
      "parameters": ["username"]
    }
  ]
}
```

* `type` は、設定オブジェクトに適用する必要のある変換のタイプを定義します。
* `propertyPath` は、設定オブジェクト内で変更する必要があるプロパティをどこで見つけるかを Platform に指示する、ピリオドで区切られた文字列です。
* `parameters` は、ラッピング関数のシグネチャに含める必要があるパラメーター名の配列です。

設定オブジェクトがタグランタイムライブラリで発行されると、次のように変換されます。

```javascript
{
  foo: {
    bar: function(username) {
      console.log('Welcome, ' + username +'. This is ZomboCom.');
    }
  }
}
```

その後、ライブラリモジュールは、ユーザーのコードを含む関数を呼び出し、`username` 引数に渡すことができます。

#### ファイル変換

ファイル変換を使用すると、Platform ユーザーが記述したコードを、タグランタイムライブラリとは別のファイルに発行できます。 このファイルは、タグランタイムライブラリと共にホストされ、必要に応じて、実行時に拡張機能で読み込むことができます。

「カスタムスクリプト」アクションタイプを指定するとします。 アクションタイプのビューには、ユーザーがコードを入力できるテキスト領域が表示される場合があります。 ユーザーがテキスト領域に次のコードを入力したとします。

`console.log('This is ZomboCom.');`

ユーザーがルールを保存すると、表示で保存される設定オブジェクトは次のようになります。

```js
{
  foo: {
    bar: "console.log('This is ZomboCom.');"
  }
}
```

ユーザーのコードを、タグランタイムライブラリ内ではなく別のファイルに配置する必要があります。 タグランタイムライブラリ内でアクションを使用するルールが起動する場合、ドキュメントの本文にスクリプト要素を追加して、ユーザーのコードを読み込みます。この例の問題を解決するには、`extension.json` のアクションタイプの定義の変換を次のように定義します。

```json
{
  "transforms": [
    {
      "type": "file",
      "propertyPath": "foo.bar"
    }
  ]
}
```

* `type` は、設定オブジェクトに適用する必要のある変換のタイプを定義します。
* `propertyPath` は、設定オブジェクト内で変更する必要があるプロパティをどこで見つけるかを Platform に指示する、ピリオドで区切られた文字列です。

設定オブジェクトがタグランタイムライブラリで発行されると、次のように変換されます。

```javascript
{
  foo: {
    bar: "//launch.cdn.com/path/abc.js"
  }
}
```

この場合、`foo.bar` の値は URL に変換されています。 正確な URL は、ライブラリの構築時に決定されます。 ファイルには常に拡張子 `.js` が付き、JavaScript 対応の MIME タイプを使用して配信されます。 将来、他の MIME タイプのサポートが追加される可能性があります。

#### 変換の削除

デフォルトでは、設定オブジェクトのすべてのプロパティは、タグランタイムライブラリに発行されます。 特定のプロパティが拡張機能の表示のみに使用される場合、特に機密情報（ 例：シークレットトークン）、変換の削除を使用して、情報がタグランタイムライブラリに発行されないようにする必要があります。

新しいアクションタイプを指定するとします。 アクションタイプのビューでは、特定の API への接続を許可する秘密キーをユーザーが入力できる場合があります。 ユーザーが次のテキストを入力したとします。

`ABCDEFG`

ユーザーがルールを保存すると、表示で保存される設定オブジェクトは次のようになります。

```js
{
  foo: {
    bar: "ABCDEFG"
  }
}
```

タグランタイムライブラリ内に `bar` プロパティを含めないようにします。 この例の問題を解決するには、`extension.json` のアクションタイプの定義の変換を次のように定義します。

```json
{
  "transforms": [
    {
      "type": "remove",
      "propertyPath": "foo.bar"
    }
  ]
}
```

* `type` は、設定オブジェクトに適用する必要のある変換のタイプを定義します。
* `propertyPath` は、設定オブジェクト内で変更する必要があるプロパティをどこで見つけるかを Platform に指示する、ピリオドで区切られた文字列です。

設定オブジェクトがタグランタイムライブラリで発行されると、次のように変換されます。

```js
{
  foo: {
  }
}
```

この場合、`foo.bar` の値は設定オブジェクトから削除されています。
