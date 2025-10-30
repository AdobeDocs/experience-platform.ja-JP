---
title: Id グラフリンクルール設定ガイド
description: ID グラフリンクルールを使用して設定できる様々な実装タイプについて説明します。
exl-id: fd0afb0b-a368-45b9-bcdc-f2f3b7508cee
source-git-commit: be2ad7a02d4bdf5a26a0847c8ee7a9a93746c2ad
workflow-type: tm+mt
source-wordcount: '1951'
ht-degree: 8%

---

# [!DNL Identity Graph Linking Rules] 設定ガイド {#configurations-guide}

>[!CONTEXTUALHELP]
>id="platform_identities_algorithmconfiguration"
>title="アルゴリズムの設定"
>abstract="取り込んだ ID に合わせて、一意の名前空間と、名前空間の優先度を設定します。"

[!DNL Identity Graph Linking Rules] を使用して設定できる様々な実装タイプについては、このドキュメントを参照してください。

顧客グラフシナリオは、3 つの異なるカテゴリにグループ化できます。

* **基本**: [&#x200B; 基本実装 &#x200B;](#basic-implementations) には、ほとんどの場合が単純な実装を含むグラフが含まれています。 これらの実装は、単一のクロスデバイス名前空間（CRMID など）を中心に展開される傾向があります。 基本的な実装はかなり簡単ですが、多くの場合、（共有デバイス **シナリオが原因で、グラフが折りたたまれ** ことがあります。
* **中間**:[&#x200B; 中間実装 &#x200B;](#intermediate-implementations) には、**複数のクロスデバイス名前空間**、**一意でない ID**、**複数の一意の名前空間** など、複数の変数が含まれます。
* **詳細**: [&#x200B; 高度な実装 &#x200B;](#advanced-implementations) には、複雑な多層グラフシナリオが含まれます。 高度な実装では、適切なリンクを確実に削除してグラフの折りたたみを防ぐために、正しい名前空間の優先順位を確立することが不可欠です。

## 基本を学ぶ

次のドキュメントに進む前に、ID サービスと [!DNL Identity Graph Linking Rules] ークフローのいくつかの重要な概念を理解していることを確認してください。

* [ID サービスの概要](../home.md)
* [[!DNL Identity Graph Linking Rules] の概要](../identity-graph-linking-rules/namespace-priority.md)
* [名前空間の優先度](namespace-priority.md)
* [一意の名前空間](overview.md#unique-namespace)
* [グラフシミュレーション](graph-simulation.md)

## 基本的な実装 {#basic-implementations}

>[!NOTE]
>
>以下の実装を完了するには、ID 記号（大文字と小文字を区別）が `CRMID` のカスタム名前空間を作成する必要があります。

[!DNL Identity Graph Linking Rules] の基本的な実装については、この節をお読みください。

### ユースケース：1 つのクロスデバイス名前空間を使用するシンプルな実装

通常、Adobeのお客様は、Web、モバイル、アプリケーションを含むすべてのプロパティで使用される 1 つのクロスデバイス名前空間を持っています。 小売、通信、金融サービスの顧客がこのタイプの実装を使用するので、このシステムは業界と地理的に無関係です。

通常、エンドユーザーはクロスデバイス名前空間（多くの場合、CRMID）で表されるので、CRMID は一意の名前空間として分類する必要があります。 コンピューターと [!DNL iPhone] を所有し、デバイスを共有していないエンドユーザーは、次のような ID グラフを持つことができます。

例えば、**ACME** という e コマース会社のデータアーキテクトの場合を考えてみます。 ジョンとジェーンは君の顧客だ。 カリフォルニア州サンノゼに共同生活を送るエンドユーザーです。 デスクトップコンピューターを共有し、このコンピューターを使用して web サイトを参照します。 同様に、John と Jane も [!DNL iPad] を共有し、この [!DNL iPad] を使用して web サイトを含むインターネットを参照することがあります。

**テキストモード**

```json
CRMID: John, ECID: 123
CRMID: John, ECID: 999, IDFA: a-b-c
```

**アルゴリズム設定（ID 設定）**

グラフをシミュレートする前に、グラフシミュレーションインターフェイスで次の設定を行います。

| 表示名 | ID 記号 | ID タイプ | グラフごとに一意 | 名前空間の優先度 |
| --- | --- | --- | --- | --- |
| CRMID | CRMID | CROSS_DEVICE | ✔️ | 1 |
| ECID | ECID | COOKIE | | 2 |
| IDFA | IDFA | デバイス | | 3 |

**シミュレーショングラフ**

このグラフでは、John （エンドユーザー）は CRMID で表されています。 `{ECID: 123}` は、John が e コマースプラットフォームにアクセスするためにパーソナルコンピューターで使用した web ブラウザーを表します。 `{ECID: 999}` は、彼が [!DNL iPhone] で使用したブラウザーを表し、`{IDFA: a-b-c}` は彼の [!DNL iPhone] を表します。

![1 つのクロスデバイス名前空間を持つシンプルな実装…](../images/configs/basic/simple-implementation.png)

**演習**

グラフシミュレーションで次の設定をシミュレートします。 独自のイベントを作成するか、テキストモードを使用してコピーして貼り付けることができます。

>[!BEGINTABS]

>[!TAB  共有デバイス（PC） ]

**共有デバイス（PC）**

**テキストモード**

```json
CRMID: John, ECID: 111
CRMID: Jane, ECID: 111
```

**シミュレーショングラフ**

このグラフでは、John と Jane は、それぞれ独自の CRMID で表されています。

* `{CRMID: John}`
* `{CRMID: Jane}`

両方のユーザーが e コマースプラットフォームへのアクセスに使用するデスクトップコンピューター上のブラウザーは、`{ECID: 111}` で表されます。 このグラフシナリオでは、Jane は最後に認証されたエンドユーザーなので、`{ECID: 111}` と `{CRMID: John}` の間のリンクは削除されます。

![&#x200B; 共有デバイス（PC）のシミュレーショングラフ。](../images/configs/basic/shared-device-pc.png)

>[!TAB  共有デバイス （モバイル） ]

**共有デバイス （モバイル）**

**テキストモード**

```json
CRMID: John, ECID: 111, IDFA: a-b-c
CRMID: Jane, ECID: 111, IDFA: a-b-c
```

**シミュレーショングラフ**

このグラフでは、John と Jane の両方がそれぞれの CRMID で表されています。 ユーザーが使用するブラウザーは `{ECID: 111}` で表され、ユーザーが共有する [!DNL iPad] は `{IDFA: a-b-c}` で表されます。 このグラフシナリオでは、Jane は最後に認証されたエンドユーザーなので、`{ECID: 111}` と `{IDFA: a-b-c}` から `{CRMID: John}` へのリンクは削除されます。

![&#x200B; 共有デバイス（モバイル）のシミュレーショングラフ。](../images/configs/basic/shared-device-mobile.png)

>[!ENDTABS]

## 中間実装 {#intermediate-implementations}

>[!TIP]
>
>**一意でない ID** は、一意でない名前空間に関連付けられた ID です。

[!DNL Identity Graph Linking Rules] の中間実装については、この節を参照してください。

### ユースケース：データに一意でない ID が含まれる

>[!NOTE]
>
>以下の実装を完了するには、の ID 記号（大文字と小文字を区別）を使用して次のカスタム名前空間を作成する必要があります。
>
>* `CRMID`
>* `CChash` （これは、ハッシュ化されたクレジットカード番号を表すカスタム名前空間です。）

例えば、クレジットカードを発行する商業銀行で働くデータアーキテクトの場合を考えてみます。 マーケティングチームが、過去のクレジットカードのトランザクション履歴をプロファイルに含める必要があることを示しました。 この ID グラフは次のようになります。

**テキストモード**

```json
CRMID: John, CChash: 1111-2222 
CRMID: John, CChash: 3333-4444 
CRMID: John, ECID: 123 
CRMID: John, ECID: 999, IDFA: a-b-c
```

**アルゴリズム設定（ID 設定）**

グラフをシミュレートする前に、グラフシミュレーションインターフェイスで次の設定を行います。

| 表示名 | ID 記号 | ID タイプ | グラフごとに一意 | 名前空間の優先度 |
| --- | --- | --- | --- | --- |
| CRMID | CRMID | CROSS_DEVICE | ✔️ | 1 |
| CChash | CChash | CROSS_DEVICE | | 2 |
| ECID | ECID | COOKIE | | 3 |
| IDFA | IDFA | デバイス | | 4 |

**シミュレーショングラフ**

![&#x200B; シミュレーショングラフの画像 &#x200B;](../images/configs/basic/simple-implementation-non-unique.png)

これらのクレジットカード番号や、その他の一意でない名前空間が、常に 1 人のエンドユーザーに関連付けられるとは保証されません。 2 人のエンドユーザーが同じクレジットカードに登録すると、一意でないプレースホルダー値が誤って取り込まれる場合があります。 簡単に言えば、一意でない名前空間がグラフの折りたたみを引き起こさない保証はありません。

この問題を解決するために、ID サービスは最も古いリンクを削除し、最新のリンクを保持します。 これにより、グラフに CRMID が 1 つだけあるので、グラフが折りたたまれるのを防ぐことができます。

**演習**

グラフシミュレーションで次の設定をシミュレートします。 独自のイベントを作成するか、テキストモードを使用してコピーして貼り付けることができます。

>[!BEGINTABS]

>[!TAB  共有デバイス ]

**テキストモード**

```json
CRMID: John, CChash: 1111-2222
CRMID: Jane, CChash: 3333-4444
CRMID: John, ECID: 123
CRMID: Jane, ECID:123
```

**シミュレーショングラフ**

![CChash を使用した中間共有デバイスグラフ &#x200B;](../images/configs/intermediate/intermediate-shared-device.png)

>[!TAB  同じクレジットカードを持つ 2 人のエンドユーザー ]

2 人の異なるエンドユーザーが、同じクレジットカードを使用して e コマース web サイトに新規登録します。 マーケティングチームは、クレジットカードが 1 つのプロファイルにのみ関連付けられるようにすることで、グラフが折りたたまれないようにします。

**テキストモード**

```json
CRMID: John, CChash: 1111-2222
CRMID: Jane, CChash: 1111-2222
CRMID: John, ECID: 123
CRMID: Jane, ECID:456
```

**シミュレーショングラフ**

![2 人のエンドユーザーが同じクレジットカードで新規登録するグラフ。](../images/configs/intermediate/graph-with-same-credit-card.png)

>[!TAB  クレジットカード番号が無効です ]

データがクリーンではないので、無効なクレジットカード番号がExperience Platformに取り込まれます。

**テキストモード**

```json
CRMID: John, CChash: undefined
CRMID: Jane, CChash: undefined
CRMID: Jack, CChash: undefined
CRMID: Jill, CChash: undefined
```

**シミュレーショングラフ**

![&#x200B; ハッシュの問題により無効なクレジットカードが発生するグラフ。](../images/configs/intermediate/graph-with-invalid-credit-card.png)

>[!ENDTABS]

### ユースケース：データには、ハッシュ化された CRMID とハッシュ化されていない CRMID の両方が含まれます

>[!NOTE]
>
>以下の実装を完了するには、の ID 記号（大文字と小文字を区別）を使用してカスタム名前空間を作成する必要があります。
>
>* `CRMID`
>* `CRMIDhash`

ハッシュ化されていない（オフラインの） CRMID とハッシュ化された（オンラインの） CRMID の両方を取り込んでいます。 ハッシュ化されていない CRMID とハッシュ化された CRMID の両方の間に直接の関係があることが期待されます。 認証済みアカウントを使用してエンドユーザーが閲覧すると、ハッシュ化された CRMID がデバイス ID と共に送信されます（ID サービスで ECID として表されます）。

**アルゴリズム設定（ID 設定）**

グラフをシミュレートする前に、グラフシミュレーションインターフェイスで次の設定を行います。

| 表示名 | ID 記号 | ID タイプ | グラフごとに一意 | 名前空間の優先度 |
| --- | --- | --- | --- | --- | 
| CRMID | CRMID | CROSS_DEVICE | ✔️ | 1 |
| CRMIDhash | CRMIDhash | CROSS_DEVICE | ✔️ | 2 |
| ECID | ECID | COOKIE | | 3 |


**演習**

グラフシミュレーションで次の設定をシミュレートします。 独自のイベントを作成するか、テキストモードを使用してコピーして貼り付けることができます。

>[!BEGINTABS]

>[!TAB  共有デバイス ]

ジョンとジェーンはデバイスを共有している。

**テキストモード**

```json
CRMID: John, CRMIDhash: John
CRMID: Jane, CRMIDhash: Jane
CRMIDhash: John, ECID: 111 
CRMIDhash: Jane, ECID: 111
```

![&#x200B; ハッシュ化された CRMID を含む共有デバイスグラフ &#x200B;](../images/configs/intermediate/shared-device-hashed-crmid.png)

>[!TAB  不正なデータ ]

ハッシュプロセスのエラーにより、一意でないハッシュ化された CRMID が生成され、ID サービスに送信されます。

**テキストモード**

```json
CRMID: John, CRMIDhash: aaaa
CRMID: Jane, CRMIDhash: aaaa
```

![&#x200B; ハッシュプロセスでエラーが発生し、一意でないハッシュ化された CRMID が発生した共有デバイスグラフ。](../images/configs/intermediate/hashing-error.png)

>[!ENDTABS]
<!-- 
### Use case: You are using Real-Time CDP and Adobe Commerce

You have two types of end-users:

* **Members**: An end-user who is assigned a CRMID and has an email account registered to your system.
* **Guests**: An end-user who is not a member. They do not have an assigned CRMID and their email accounts are not registered to your system.

In this scenario, your customers are sending data from Adobe Commerce to Real-Time CDP.

**Exercise**

Simulate the following configurations in the graph simulation tool. You can either create your own events, or copy and paste using text mode.

>[!BEGINTABS]

>[!TAB Shared device between two members]

In this scenario, two members share the same device to browse an e-commerce website.

**Text mode**

```json
CRMID: John, Email: john@g
CRMID: Jane, Email: jane@g
CRMID: John, ECID: 111
CRMID: Jane, ECID: 111
```

![A graph that displays two authenticated members who share a device.](../images/configs/intermediate/shared-device-two-members.png)

>[!TAB Shared device between two guests]

In this scenario, two guests share the same device to browse an e-commerce website.

**Text mode**

```json
Email: john@g, ECID: 111
Email: jane@g, ECID: 111
```

![A graph that displays two guests who share a device.](../images/configs/intermediate/shared-device-two-guests.png)

>[!TAB Shared device between a member and a guest]

In this scenario, a member and a guest share the same device to browse an e-commerce website.

**Text mode**

```json
CRMID: John, Email: john@g
CRMID: John, ECID: 111
Email: jane@g, ECID: 111
```

![A graph that displays a member and a guest who share a device.](../images/configs/intermediate/shared-device-member-and-guest.png)

>[!ENDTABS] -->

### ユースケース：データには、3 つの一意の名前空間が含まれます

>[!NOTE]
>
>以下の実装を完了するには、ID 記号（大文字と小文字を区別）が `CRMID` のカスタム名前空間を作成する必要があります。

顧客は、次のように単一人物エンティティを定義します。

* CRMID が割り当てられているエンドユーザー。
* プロファイルをハッシュ化されたメールをサポートする宛先（例：[!DNL Facebook]）にアクティブ化できるように、ハッシュ化されたメールアドレスに関連付けられているエンドユーザー。
* サポート担当者がメールアドレスを使用してReal-Time CDP上でプロファイルを検索できるように、メールアドレスに関連付けられたエンドユーザー。

| 表示名 | ID 記号 | ID タイプ | グラフごとに一意 | 名前空間の優先度 |
| --- | --- | --- | --- | --- |
| CRMID | CRMID | CROSS_DEVICE | ✔️ | 1 |
| 電子メール | 電子メール | 電子メール | ✔️ | 2 |
| Email_LC_SHA256 | Email_LC_SHA256 | メール | ✔️ | 3 |
| ECID | ECID | COOKIE | | 4 |

グラフシミュレーションツールで次の設定をシミュレートします。 独自のイベントを作成するか、テキストモードを使用してコピーして貼り付けることができます。

>[!BEGINTABS]

>[!TAB  共有デバイス ]

このシナリオでは、John と Jane の両方が e コマース web サイトにログインします。

**テキストモード**

```json
CRMID: John, Email: john@g, Email_LC_SHA256: john_hash 
CRMID: Jane, Email: jane@g, Email_LC_SHA256: jane_hash 
CRMID: John, ECID: 111 
CRMID: Jane, ECID: 111
```

![&#x200B; 同じデバイスを使用して web サイトにログインした 2 人のエンドユーザーを表示するグラフ。](../images/configs/intermediate/two-end-users-log-ing.png)

>[!TAB  エンドユーザーがメールを変更した場合 ]

**テキストモード**

```json
CRMID: John, Email: john@g, Email_LC_SHA256: john_hash
CRMID: John, Email: john@y, Email_LC_SHA256: john_y_hash
```

![&#x200B; メールを変更したエンドユーザーを表示するグラフ。](../images/configs/intermediate/end-user-changes-email.png)

>[!ENDTABS]

## 高度な実装 {#advanced-implementations}

高度な実装には、複雑で複数のレイヤーを持つグラフシナリオが含まれます。 これらのタイプの実装には、グラフの折りたたみを防ぐために削除する必要がある正しいリンクを識別するための **名前空間優先度** の使用が含まれます。

**名前空間の優先度** は、名前空間を重要度別にランク付けするメタデータです。 グラフに 2 つの ID が含まれ、それぞれが異なる一意の名前空間を持つ場合、ID サービスは名前空間の優先度を使用して、削除するリンクを決定します。 詳しくは、[&#x200B; 名前空間の優先度に関するドキュメント &#x200B;](../identity-graph-linking-rules/namespace-priority.md) を参照してください。

名前空間の優先度は、複雑なグラフシナリオで重要な役割を果たします。 グラフには複数のレイヤーを含めることができます。1 人のエンドユーザーを複数のログイン ID に関連付けて、これらのログイン ID をハッシュ化することもできます。 さらに、別の ECID を別のログイン ID にリンクすることもできます。 適切なレイヤーの適切なリンクが削除されるようにするには、名前空間の優先度の設定を正しく設定する必要があります。

[!DNL Identity Graph Linking Rules] の高度な実装については、この節を参照してください。

### 使用例：複数の事業部門のサポートが必要な場合

>[!NOTE]
>
>以下の実装を完了するには、の ID 記号（大文字と小文字を区別）を使用してカスタム名前空間を作成する必要があります。
>
>* `CRMID`
>* `loginID`

エンドユーザーは、個人用アカウントとビジネスアカウントという 2 つの異なるアカウントを持っています。 各アカウントは、異なる ID で識別されます。 このシナリオでは、一般的なグラフは次のようになります。

**テキストモード**

```json
CRMID: John, loginID: JohnPersonal
CRMID: John, loginID: JohnBusiness
loginID: JohnPersonal, ECID: 111
loginID: JohnPersonal, ECID: 222
loginID: JohnBusiness, ECID: 222
```

**アルゴリズム設定（ID 設定）**

グラフをシミュレートする前に、グラフシミュレーションインターフェイスで次の設定を行います。

| 表示名 | ID 記号 | ID タイプ | グラフごとに一意 | 名前空間の優先度 |
| --- | --- | --- | --- | --- |
| CRMID | CRMID | CROSS_DEVICE | ✔️ | 1 |
| loginID | loginID | CROSS_DEVICE | | 2 |
| ECID | ECID | COOKIE | | 3 |

**シミュレーショングラフ**

![&#x200B; ビジネスと個人のメールを持つエンドユーザーの ID グラフ &#x200B;](../images/configs/advanced/advanced.png)

**演習**

グラフシミュレーションで次の設定をシミュレートします。 独自のイベントを作成するか、テキストモードを使用してコピーして貼り付けることができます。

>[!BEGINTABS]

>[!TAB  共有デバイス ]

**テキストモード**

```json
CRMID: John, loginID: JohnPersonal
CRMID: John, loginID: JohnBusiness
CRMID: Jane, loginID: JanePersonal
CRMID: Jane, loginID: JaneBusiness
loginID: JohnPersonal, ECID: 111
loginID: JanePersonal, ECID: 111
```

![&#x200B; 高度な共有デバイスのグラフ。](../images/configs/advanced/advanced-shared-device.png)

>[!TAB  無効なデータがReal-Time CDPに送信される ]

**テキストモード**

```json
CRMID: John, loginID: JohnPersonal
CRMID: John, loginID: error
CRMID: Jane, loginID: JanePersonal
CRMID: Jane, loginID: error
loginID: JohnPersonal, ECID: 111
loginID: JanePersonal, ECID: 222
```

![&#x200B; 無効なデータがReal-Time CDPに送信されるシナリオを表示するグラフ。](../images/configs/advanced/advanced-bad-data.png)

>[!ENDTABS]

### ユースケース：複数の名前空間を必要とする複雑な実装がある場合

>[!NOTE]
>
>以下の実装を完了するには、の ID 記号（大文字と小文字を区別）を使用してカスタム名前空間を作成する必要があります。
>
>* `CRMID`
>* `loyaltyID`
>* `thirdPartyID`
>* `orderID`

メディアおよびエンターテインメント企業の場合、エンドユーザーは次のアイテムを所有しています。

* CRMID
* ロイヤルティ ID

さらに、エンドユーザーは e コマース web サイトで購入でき、このデータはエンドユーザーのメールアドレスに関連付けられます。 ユーザーデータは、サードパーティのデータベースプロバイダーによってもエンリッチメントされ、バッチでExperience Platformに送信されます。

**テキストモード**

```json
CRMID: John, loyaltyID: John, Email: john@g
Email: john@g, orderID: aaa
CRMID: John, thirdPartyID: xyz
CRMID: John, ECID: 111
```

**アルゴリズム設定（ID 設定）**

グラフをシミュレートする前に、グラフシミュレーションインターフェイスで次の設定を行います。

| 表示名 | ID 記号 | ID タイプ | グラフごとに一意 | 名前空間の優先度 |
| --- | --- | --- | --- | --- |
| CRMID | CRMID | CROSS_DEVICE | ✔️ | 1 |
| loyaltyID | loyaltyID | CROSS_DEVICE | ✔️ | 2 |
| 電子メール | 電子メール | 電子メール | ✔️ | 3 |
| thirdPartyID | thirdPartyID | CROSS_DEVICE | | 4 |
| orderID | orderID | CROSS_DEVICE | | 5 |
| ECID | ECID | COOKIE | | 6 |

**演習**

グラフシミュレーションで次の設定をシミュレートします。 独自のイベントを作成するか、テキストモードを使用してコピーして貼り付けることができます。

>[!BEGINTABS]

>[!TAB  共有デバイス ]

**テキストモード**

```json
CRMID: John, loyaltyID: John, Email: john@g
CRMID: Jane, loyaltyID: Jane, Email: jane@g
Email: john@g, orderID: aaa 
CRMID: John, thirdPartyID: xyz 
CRMID: John, ECID: 111
CRMID: Jane, ECID: 111
```

![&#x200B; 共有デバイスの複雑なグラフの例 &#x200B;](../images/configs/advanced/complex-shared-device.png)

>[!TAB  エンドユーザーがメールアドレスを変更する ]

**テキストモード**

```json
CRMID: John, loyaltyID: John, Email: john@g
CRMID: John, loyaltyID: John, Email: john@y
```

![&#x200B; メールの変更が与えられた ID の動作を表示するグラフ。](../images/configs/advanced/complex-email-change.png)

>[!TAB thirdPartyID の関連付けが変更されました ]

**テキストモード**

```json
CRMID: John, loyaltyID: John, Email: john@g
CRMID: Jane, loyaltyID: Jane, Email: jane@g
CRMID: John, thirdPartyID: xyz
CRMID: Jane, thirdPartyID: xyz
```

![&#x200B; サードパーティ ID の関連付けに変更があった場合の ID 動作を表示するグラフ。](../images/configs/advanced/complex-third-party-change.png)

>[!TAB  一意でない orderID]

**テキストモード**

```json
CRMID: John, loyaltyID: John, Email: john@g
CRMID: Jane, loyaltyID: Jane, Email: jane@g
Email: john@g, orderID: aaa
Email: jane@g, orderID: aaa
```

![&#x200B; 一意でない注文 ID が指定された ID の動作を表示するグラフ。](../images/configs/advanced/complex-non-unique.png)

>[!TAB  エラーのある loyaltyID]

**テキストモード**

```json
CRMID: John, loyaltyID: aaa, Email: john@g
CRMID: Jane, loyaltyID: aaa, Email: jane@g
```

![&#x200B; 誤ったロイヤルティ ID が指定された ID 行動を表示するグラフ。](../images/configs/advanced/complex-error.png)

>[!ENDTABS]

## 次の手順

[!DNL Identity Graph Linking Rules] について詳しくは、次のドキュメントを参照してください。

* [[!DNL Identity Graph Linking Rules] の概要](./overview.md)
* [ID 最適化アルゴリズム](./identity-optimization-algorithm.md)
* [実装ガイド](./implementation-guide.md)
* [トラブルシューティングと FAQ](./troubleshooting.md)
* [名前空間の優先度](./namespace-priority.md)
* [グラフシミュレーション UI](./graph-simulation.md)
* [ID 設定 UI](./identity-settings-ui.md)