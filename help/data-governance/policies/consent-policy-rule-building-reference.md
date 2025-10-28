---
title: 同意ポリシールール構築リファレンス
description: XDM データタイプ、サポートされている演算子、高度なロジックを使用して、Adobe Experience Platform UI で詳細な同意ポリシールールを作成する方法を説明します。
source-git-commit: 678220b14cefd4dd31ef7a12355d796575a77a50
workflow-type: tm+mt
source-wordcount: '1576'
ht-degree: 0%

---

# 同意ポリシールール構築リファレンス

Adobe Experience Platformの同意ポリシービルダーの **[!UICONTROL Then]** 句に、正確で法的に有効なルールを設定するには、高度なルールロジックに関するこの参照を使用します。

![&#x200B; ユーザーがルール条件を定義する [!UICONTROL Then] 句セクションを強調表示した同意ポリシービルダーインターフェイス &#x200B;](../images/policies/multiple-rules.png)

顧客の同意環境設定を正確に適用するために、ポリシールールを同意データの構造とタイプに適用する方法を説明します。

このドキュメントでは、XDM スキーマのコンテナフィールドに移動し、プリミティブなフィールドを選択することで、同意に基づいてプロファイルをフィルタリングする方法を説明します。 次に、適切な演算子を使用して、プロファイルが一致する必要のある正確な値を定義します。

## 前提条件

このリファレンスを使用する前に、同意ポリシーの設定が完了し、Adobe Experience Platformのデータアーキテクチャとガバナンスフレームワークの基本概念を理解していることを確認してください。

次の前提条件を満たしていることを確認してください。

* **ポリシー設定の完了**:Adobe Experience Platform UI で同意ポリシーを作成または作成し始めました。 詳しい手順については、[&#x200B; データ使用ポリシーユーザーガイド &#x200B;](user-guide.md#consent-policy) を参照してください。

* **データ構造に精通している**：このリファレンスでは、次の中心概念に関する実務知識が必要です。
   * **XDM と結合スキーマ**：エクスペリエンスデータモデル構造によるデータ関係の定義方法と、結合スキーマによる統合顧客プロファイルの表現方法について説明します。 詳しくは、[XDM システムの概要 &#x200B;](../../xdm/home.md) を参照してください。
   * **データガバナンスフレームワーク**:Adobe Experience Platformでのデータ使用ポリシーとガバナンスルールの適用方法を説明します。 詳しくは、[&#x200B; データガバナンスの概要 &#x200B;](../home.md) を参照してください。
   * **顧客の同意処理**：同意データを収集、保存、顧客体験ワークフロー内で適用する方法を理解します。 [&#x200B; 同意処理の概要 &#x200B;](../../landing/governance-privacy-security/consent/adobe/overview.md) を参照してください。

## 中心概念：プリミティブフィールドとコンテナフィールド

この節では、同意ポリシールールが XDM スキーマで様々なフィールドタイプを使用する方法について説明します。 コンテナ フィールドとプリミティブ フィールドの違いを理解すると、ポリシー条件を定義する際に正しいフィールドおよび演算子を選択するのに役立ちます。

### サポートされるフィールドタイプとルールロジック

同意ポリシーでは、複数のフィールドタイプをサポートし、それぞれにルール条件を作成するための特定の演算子があります。 フィールドタイプは、**コンテナタイプ** と **プリミティブタイプ** の 2 つのカテゴリにグループ化されます。

### コンテナタイプ （スキーマナビゲーション）

コンテナタイプは同意データを整理しますが、ポリシー条件で直接使用することはできません。 これらは、実際の値を保持するプリミティブフィールドに到達するためのナビゲーションパスとして機能します。

| コンテナタイプ | 説明 |
|----------------|-------------|
| **オブジェクト** | 異なるタイプの複数のフィールドを保持する、固定スキーマを持つコンテナ。 |
| **配列** | 同じタイプの複数の値を保持するコンテナ。 |
| **マップ** | オブジェクトや他のフィールド タイプを保持できるダイナミック キーを持つコンテナ。 |

>[!IMPORTANT]
>
>同意ポリシーの条件では、コンテナフィールドを直接選択できません。 コンテナに移動して、ルール作成用の **プリミティブフィールド** （文字列、数値、ブール値など）を選択する必要があります。 コンテナ演算子は、スキーマのナビゲーションにのみ使用され、ポリシー条件の設定には使用されません。

### プリミティブタイプ（ルール条件）

プリミティブフィールドは、実際の同意データ値（`true` や `"weekly"` など）を保持し、ポリシー条件の定義に使用できる唯一のフィールドタイプです。

次の表に、サポートされている各プリミティブ型と使用可能な演算子を示します。

| プリミティブ型 | サポートされる演算子 | 説明 |
|----------------|---------------------|-------------|
| **文字列** | `is equal to`、`is not equal to`、`exists`、`does not exist` | テキストベースの同意属性。 |
| **数値** | `is equal to`, `is not equal to`, `is greater than`, `is less than`, `exists`, `does not exist` | 数値の同意属性。 |
| **ブール値** | `is equal to`, `is not equal to` | True または false の同意値。 |
| **日付** | `is equal to`、`is not equal to`、`exists`、`does not exist` | 日付ベースの同意属性。 |


## 複雑なデータ構造の操作

この節では、同意スキーマ内でネストされたコンテナを移動して、プリミティブなフィールドに到達する方法を説明します。 共通のスキーマパターンを紹介し、より深い構造によってより詳細な同意ロジックが可能になる仕組みを説明します。

### ネストされた複雑なスキーマ構造の処理

複雑な同意スキーマには、多くの場合、柔軟で拡張性の高いデータ管理をサポートする、ネストされたコンテナ構造が含まれています。 ポリシールールはプリミティブなフィールドのみを参照できるので、同意ポリシー条件で使用できるフィールドに到達するには、コンテナ階層をナビゲートする必要があります。 ネストを深くすることで、より詳細で具体的なルールのターゲット設定が可能になります。

一般的なネストされたコンテナパターンを次に示します。

* **マップのマップ** – 他のマップを含む動的キー。
* **オブジェクトのマップ** – 固定スキーマのオブジェクトを含む動的キー。
* **マップの配列** – 動的キーを持つマップを含む配列。
* **オブジェクトの配列** – 固定スキーマを持つオブジェクトを含む配列。
* **map または array プロパティを持つオブジェクト** - map または array フィールドを含むオブジェクト。

### フィールド構造の例

次の構造は、このガイド全体でのルール例の視覚的な参照として機能します。

```
consent.marketing (Object)
├── email (Boolean)
├── sms (Boolean)
├── preferences (Map with dynamic keys)
│   ├── "email_preferences" (Object)
│   │   ├── frequency (String)
│   │   └── channels (Array of Strings)
│   ├── "sms_preferences" (Object)
│   │   ├── frequency (String)
│   │   └── opt_in_time (Date)
│   └── "push_preferences" (Object)
│       ├── frequency (String)
│       └── categories (Array of Strings)
└── lastUpdated (Date)
```

## フィールドタイプ別の高度なルール作成

フィールドタイプに基づいて同意ポリシールールを作成する方法について詳しくは、この節を参照してください。 ブール値、マップ、オブジェクトおよび配列のルールロジックを設定して、正確な同意条件を取得する方法を説明します。

### ルール作成のコンポーネントと手順

効果的な同意ポリシールールを作成するには、スキーマ構造をナビゲートし、各フィールドタイプに正しい演算子を適用する方法を理解する必要があります。 各ルールは、同じ基本的なアプローチに従います。プリミティブなフィールドに移動し、適切な演算子を選択して、満たす必要がある条件を定義します。

ルールを作成するには、次の手順に従います。

1. **フィールドを選択** - コンテナフィールド内を移動して、プリミティブなフィールドに到達します。
2. **演算子を選択** - フィールドタイプでサポートされている演算子を選択します。
   ![&#x200B; ユーザーがコンテナを展開してプリミティブフィールドに到達する様子を示す、階層スキーマナビゲーションパネル。](../images/policies/consent-policy-map-field.png)
3. **値を設定** – 一致する値または条件を定義します。
4. **マップ キーを一致** – 特定のキーをターゲットにするか、マップ内のすべてのキーを一致させるかを選択します。
5. **条件の追加** – 必要に応じて、AND または OR ロジックを使用して複数のルールを組み合わせます。

### ブール値フィールドの使用（暗黙の同意ロジック）

ブール値フィールドは、true または false の同意値を格納し、最も一般的な同意属性を表します。 `is not equal to` 演算子を使用すると、明示的にオプトアウトされていないプロファイルを含めて、暗黙の同意シナリオをサポートできます。

**ブール演算子と結果**

| 演算子 | 値 | 結果 |
|----------|-------|--------|
| `is equal to` | `true` | 明示的な同意（`true`）を持つプロファイルを含みます。 |
| `is equal to` | `false` | 明示的なオプトアウト （`false`）を持つプロファイルを含みます。 |
| `is not equal to` | `true` | 明示的な同意なしにプロファイルを含めます（`false` または欠落）。 |
| `is not equal to` | `false` | 明示的にオプトアウトしていない（`true` または不足している）プロファイルが含まれます。 |

**例：暗黙のメール同意**

```
Field: consent.marketing.email (boolean)
Operator: is not equal to
Value: false
Result: Includes profiles who have not explicitly opted out of email marketing (includes both true and missing/null values).
```

### マップフィールドの操作（動的環境設定）

マップフィールドは、固定スキーマを持つオブジェクトとは異なり、動的キーを持つキーと値のペアを格納します。 マップは、スキーマを更新せずに新しいカテゴリを追加できる環境設定センターでよく使用されます。 特定のキーをターゲットにするか、すべてのキーに対してワイルドカード一致を使用できます。

**特定のキーのマッチング**

特定の環境設定カテゴリをターゲットにするには、この方法を使用します。

```
Field: consent.preferences["email_preferences"].frequency (string) - navigated to from the map container
Operator: is equal to
Value: "weekly"
Result: Includes profiles who set the email frequency to weekly (for the "email_preferences" key)
```

**任意のキーの一致**

「**[!UICONTROL find any matching item]**」チェックボックスオプションを使用して、マップ内のすべての動的キーを照合します。

![&#x200B; すべての動的キーの値を一致させるために使用される、マップフィールドの「一致する項目を検索」チェックボックスを表示するポリシールールビルダー。](../images/policies/find-any-matching-item.png)

```
Field: consent.preferences.*.frequency (string)
Operator: is equal to
Value: "weekly"
Result: Includes profiles who set frequency to weekly in ANY preference category (for example, email_preferences, sms_preferences, or push_preferences)
```

### オブジェクトフィールドの操作（固定ナビゲーション）

オブジェクトフィールドは、スキーマが固定されたコンテナとして機能します。 これらはナビゲーションにのみ使用され、ポリシー条件で直接参照することはできません。

**ナビゲーションの例**

```
consent.marketing (object) → consent.marketing.email (boolean)
```

**ユースケースの例：**

```
Field: consent.marketing.email (Boolean) - navigated to from the object
Operator: is equal to
Value: true
Result: Include profiles who have explicitly consented to marketing emails
```


### 配列フィールドの操作（複数値）

配列フィールドには、同じタイプの複数の値が含まれており、プリミティブとオブジェクトのどちらを格納するかによって、異なる処理が必要です。 ナビゲーションと演算子のオプションは、配列のタイプによって異なります。

**プリミティブの配列の例**

`contains` 演算子を使用して、配列内の特定の値に基づいてプロファイルを識別します。

```
Field: consent.communication_channels (array of strings)
Operator: contains
Value: "email"
Result: Include profiles who have consented to email communication
```

**オブジェクトの配列の例**

配列に移動して、ネストされたオブジェクト内のプリミティブフィールドにアクセスします。

```
Field: consent.preferences["email_preferences"].categories[].type - navigated to from the array
Operator: is equal to
Value: "promotional"
Result: Include profiles where any email category is "promotional"
```

## ルールと複雑なロジックの組み合わせ

ここでは、AND または OR ロジックを使用して複数のルール条件を組み合わせる方法について説明します。 論理演算子が連携して、高度な複数条件の同意ポリシーを定義する方法を学びます。

### 複数の条件の組み合わせ（AND または OR ロジック）

AND または OR ロジックを使用して複数のルール条件を組み合わせ、特定のプロファイルセグメントをターゲットにするより高度な同意ポリシーを作成できます。\
**AND ロジック** では、すべての条件が true である必要があり、より狭いオーディエンス一致が生成されます。\
**OR ロジック** を使用すると、あらゆる条件を true にでき、オーディエンスのリーチを拡大できます。

同意ポリシーインターフェイスで、ルール条件の間に表示されるロジックセレクターを使用して、AND および OR ロジックを切り替えます。

### 一般的な複雑なルールの例

次の例では、基本的な同意ステータスと環境設定の頻度を組み合わせて、ターゲットセグメントを作成しています。

```
Field: consent.marketing.email
Operator: is equal to true
AND
Field: consent.preferences.frequency
Operator: is not equal to "daily"
Result: Include profiles who consent to email marketing but not to a daily frequency
```

### オブジェクトの配列に対する高度なロジック

オブジェクトの配列内で条件を組み合わせる場合、条件間に AND ロジックまたは OR ロジックを使用するかによって動作が異なります。

**例：AND 条件を持つオブジェクトの配列**

すべての条件を *same* 配列要素に適用する必要がある場合は、AND ロジックを使用します。

```
Field: consent.preferences["email_preferences"].categories[].enabled (boolean)
Operator: is equal to
Value: true
AND
Field: consent.preferences["email_preferences"].categories[].type (string)
Operator: is equal to
Value: "promotional"
Result: Includes profiles where the same category entry has both enabled=true and type="promotional".
Note: AND conditions apply to the same array entry. Using OR logic would include profiles if any array entry matches any of the conditions.
```

>[!TIP]
>
>**AND ロジックのベストプラクティス**
>
>AND ベースの配列条件を作成する場合は、次の主な動作に注意してください。
>
>* すべての条件を **同じ配列要素** に適用する必要がある場合は、AND ロジックを使用します。
>* AND は、制限のあるターゲティングを作成します（一致するプロファイルが少なくなります）。
>* AND ロジックが複数の配列エントリで一致することは想定しないでください。各エントリ内で適用されます。
>* エントリ間で柔軟に一致させる必要がある場合は、AND ロジックを使用しないでください。

>[!IMPORTANT]
>
>AND ロジックを使用すると、各配列エントリは、指定されたすべての条件を一度に満たす必要があります。 この動作は、有効化されているカテゴリとプロモーションされているカテゴリの両方など、結合された属性を照合する必要がある場合に最適です。

>[!NOTE]
>
>AND ロジックは、同じ配列エントリ **オブジェクトの配列の場合のみ** に適用されます。\
>プリミティブの配列の場合、AND ロジックは、配列全体のフィールドレベルで評価されます。

**例：OR 条件を持つオブジェクトの配列**

OR ロジックを使用して、配列エントリ全体で任意の条件を true にできるようにすることで、包括的なオーディエンス一致を作成します。

```
Field: consent.preferences["email_preferences"].categories[].enabled (boolean)
Operator: is equal to
Value: true
OR
Field: consent.preferences["email_preferences"].categories[].type (string)
Operator: is equal to
Value: "newsletter"
Result: Includes profiles where any category entry has enabled=true or any entry has type="newsletter".
Note: OR logic allows matching across different array entries. One entry can meet the first condition while another meets the second.
```

### 次の手順

同意ポリシールールを作成および調整した後、次のリソースを使用して、設定を最終決定し、ポリシーの適用を検証し、基になるデータモデルを確認します。

* **ポリシー作成ワークフロー**:[&#x200B; 同意ポリシー UI ガイド &#x200B;](user-guide.md#consent-policy.md) でポリシービルダー UI で定義したルールを実装します
* **同意ポリシーの評価と適用**：有効なポリシーがオーディエンスのアクティベーションとプロファイルデータの使用に与える影響を確認します。 詳しくは、[&#x200B; 自動ポリシー適用ガイド &#x200B;](../enforcement/auto-enforcement.md) を参照してください
* **XDM 同意データタイプ**：ポリシールールで使用される同意属性について、特定のスキーマ構造とフィールド定義を参照します。 [XDM 同意および環境設定データタイプ &#x200B;](../../xdm/data-types/consents.md) ガイドを参照してください。
