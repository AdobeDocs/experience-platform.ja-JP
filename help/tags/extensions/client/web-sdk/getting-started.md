---
title: Web SDK タグ拡張機能の概要
description: Web SDK タグ拡張機能を使用して、Adobe Experience Platform Edge Networkにイベントデータを送信します。
exl-id: 01ddbb19-40bb-4cb5-bfca-b272b88008b3
source-git-commit: 8dd658c46fc92ae6d450213ab79f2766f4211c53
workflow-type: tm+mt
source-wordcount: '755'
ht-degree: 5%

---

# Web SDK タグ拡張機能の概要

Adobe Experience Platform **tags** （旧称 Launch）を使用すると、web サイトからEdge NetworkおよびダウンストリームのAdobe ソリューションにイベントデータを送信できます。

これらの手順を実行する前に、次の [ プロパティ権限 ](/help/tags/ui/administration/user-permissions.md) にアクセスできることを確認してください。

* [!UICONTROL Develop]
* [!UICONTROL Manage extensions]

さらに、次のカテゴリにすべての [ 権限 ](/help/access-control/home.md#permissions) があることを確認します。

* データモデリング
* ID

## XDM スキーマの作成 {#schema}

[ エクスペリエンスデータモデル（XDM） ](/help/xdm/home.md) は、スキーマの形式でデータの共通の構造と定義を提供するオープンソース仕様です。 データをEdge Networkに送信する場合は、スキーマを設定することを強くお勧めします。

1. Adobe IDの資格情報を使用して [experience.adobe.com](https://experience.adobe.com) にログインします。
1. **[!UICONTROL Data Collection]**／**[!UICONTROL Schemas]**&#x200B;に移動します。
1. **[!UICONTROL Create schema]** を選択します。
1. 「**[!UICONTROL Experience Event]**」を選択し、「**[!UICONTROL Next]**」を選択します。
1. スキーマに必要な名前を付けて、「**[!UICONTROL Finish]**」を選択します。
1. （オプション）収集する追加データについて、さらにフィールドまたは [ フィールドグループ ](/help/xdm/ui/resources/field-groups.md) を追加できます。

![ スキーマキャンバス ](assets/getting-started/schema-structure.png)

>[!NOTE]\
>保存したスキーマでは、変更 *追加* のみが許可されます。 詳しくは、[ スキーマの進化 ](/help/xdm/schema/composition.md#evolution) を参照してください。

## データストリームの作成 {#datastream}

[ データストリーム ](/help/datastreams/overview.md) とは、送信するデータの処理方法をEdge Networkに指示する設定です。 特定の製品にデータを送信するようにデータストリームを設定すると、データストリームは、関連するデータを、特定の製品が理解できる方法で、各製品に自動的に渡します。

1. **[!UICONTROL Data Collection]**／**[!UICONTROL Datastreams]**&#x200B;に移動します。
1. **[!UICONTROL New datastream]** を選択します。
1. データストリームに必要な名前を付け、**[!UICONTROL Mapping schema]** の下で最近作成したスキーマを選択します。
1. **[!UICONTROL Save]** を選択します。

![ データストリームリスト ](assets/getting-started/datastreams.png)

## タグプロパティの作成

スキーマとデータストリームを作成したら、タグプロパティを作成して設定できます。

1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. **[!UICONTROL New property]** を選択します。
1. タグプロパティに必要な名前とドメインを指定し、「**[!UICONTROL Save]**」を選択します。

## タグ拡張機能のインストール

Web SDK タグ拡張機能は、指定されたタグプロパティにインストールされます。

1. **[!UICONTROL Data Collection]**/**[!UICONTROL Tags]**/**[!UICONTROL Extensions]** に移動します。
1. 「**[!UICONTROL Catalog]**」タブを選択します。
1. 検索を使用して **[!UICONTROL Adobe Experience Platform Web SDK]** 拡張機能を見つけます。
1. 拡張機能カードを選択し、右側の「**[!UICONTROL Install]**」を選択します。

![SDKのインストール ](assets/getting-started/install-sdk.png)

## タグ拡張機能の設定

Web SDK タグ拡張機能をインストールすると、自動的に [Configuration](configure/config-overview.md) ページが表示されます。

1. [ データストリーム セクション ](configure/datastreams.md) で、各環境に目的のデータストリームを選択します。

その他の設定は、すべて自己入力かオプションで指定します。 必要な設定を行い、「**[!UICONTROL Save]**」を選択します。

## 可変データ要素の作成

Adobeでは、[ 変数 ](data-element-types.md#variable) データ要素を使用して、Adobeに送信するペイロードを保存することをお勧めします。 XDM オブジェクトも使用可能なデータ要素ですが、より古く、該当するユースケースに限られています。

1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Data elements]**/**[!UICONTROL Create new data element]** を選択します。
1. データ要素に、左側で次のプロパティを指定します。
   * **[!UICONTROL Name]**：任意の名前
   * **[!UICONTROL Extension]**：[!UICONTROL Adobe Experience Platform Web SDK]
   * **[!UICONTROL Data element type]**：[!UICONTROL Variable]
1. 右側で次のプロパティを設定します。
   **変数タイプ**:XDM
   **[!UICONTROL Sandbox]**：スキーマを作成したサンドボックス
   **[!UICONTROL Schema]**：目的のスキーマ
1. **[!UICONTROL Save]** を選択します。

## ルールの作成

ルールは、何かをトリガーにするタイミングや、変数を設定するタイミングを決定します。 ライブラリが読み込まれるたびに実行されるルールを作成すると、すべてのページに値を含める変数を簡単に入力できます。

1. **[!UICONTROL Data Collection]**／**[!UICONTROL Tags]**&#x200B;に移動します。
1. 目的のタグプロパティを選択します。
1. **[!UICONTROL Rules]**/**[!UICONTROL Add rule]** を選択します。
1. ルールに目的の名前を付けます。
1. `+` の横にある「**[!UICONTROL Events]**」アイコンを選択します。
1. イベントに次の設定を行います。
   * **[!UICONTROL Extension]**：[!UICONTROL Core]
   * **[!UICONTROL Event type]**：[!UICONTROL Library loaded (page top)]
1. **[!UICONTROL Keep changes]** を選択します。

上記の手順では、ライブラリが読み込まれたらトリガーを実行するという、ルールの条件部分を定義します。 次の手順では、基準を満たした場合のアクションを設定します。

1. `+` の横にある「**[!UICONTROL Actions]**」アイコンを選択します。
1. アクションの左側で、次の設定を行います。
   * **[!UICONTROL Extension]**：[!UICONTROL Adobe Experience Platform Web SDK]
   * **[!UICONTROL Action type]**：[!UICONTROL Send event]
1. 以下のフィールドを右側に設定します。
   * **[!UICONTROL XDM]**:XDM 変数データ要素
1. **[!UICONTROL Keep changes]** を選択します。

![アクション設定](assets/getting-started/action-config.png)

## 公開

タグプロパティには、Edge Networkにデータを送信するために必要なすべてのコンポーネントが含まれています。

1. **[!UICONTROL Data Collection]**／**[!UICONTROL Publishing flow]**&#x200B;に移動します。
1. **[!UICONTROL Add library]** を選択します。
1. ライブラリに目的の名前を付けます。 バージョン管理ソフトウェアで作業する場合、この名前はコミット名と似ていると考えてください。
1. 環境ドロップダウンメニューを **[!UICONTROL Development]** に設定します。
1. **[!UICONTROL Add all changed resources]** を選択します。
1. **[!UICONTROL Save & build to Development]** を選択します。

これで、変更が開発環境にデプロイされました。

1. **[!UICONTROL Data Collection]**／**[!UICONTROL Environments]**&#x200B;に移動します。
1. 開発環境の横にある「インストール」アイコンを選択します。
1. サイトのテスト web ページ内に埋め込みコードをインストールします。

タグが開発環境で動作することを検証したら、[!UICONTROL Publishing flow] インターフェイスを使用してライブラリをステージング環境に公開し、最終的に実稼動環境に公開できます。

1. 拡張機能とルールを **ライブラリ** に追加し、**環境** にビルドして、埋め込みコードをサイトにインストールします。
2. **Adobe Experience Platform Debugger** で検証します。

これで、イベントをキャプチャしてEdge Networkに送信するリーン方式の設定ができました。 スキーマにフィールドを追加したり、データストリームに製品を追加したり、タグプロパティにデータ要素を追加したりして、実装をさらに構築できるようになりました。
