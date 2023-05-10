---
title: Splunk 拡張機能の概要
description: Adobe Experience Platform でのイベント転送用の Splunk 拡張機能について説明します。
exl-id: 653b5897-493b-44f2-aeea-be492da2b108
source-git-commit: bfbad3c11df64526627e4ce2d766b527df678bca
workflow-type: tm+mt
source-wordcount: '1051'
ht-degree: 100%

---

# Splunk 拡張機能の概要

[Splunk](https://www.splunk.com/ja_jp) は、データに関する実用的なインサイトの検索、分析、ビジュアライゼーションを提供する観察プラットフォームです。 Splunk [イベント転送](../../../ui/event-forwarding/overview.md)拡張機能は、[Splunk HTT Event Collector REST API](https://docs.splunk.com/Documentation/Splunk/8.2.5/Data/HECRESTendpoints) を活用して、Adobe Experience Platform Edge Network から [Splunk HTTP イベントコレクター](https://docs.splunk.com/Documentation/Splunk/8.2.5/Data/UsetheHTTPEventCollector)にイベントを送信します。

Splunk は、Splunk Event Collector API と通信するための認証メカニズムとして bearer トークンを使用します。

## ユースケース {#use-cases}

マーケティングチームは、次のユーズケースで拡張機能を使用できます。

| ユースケース | 説明 |
| --- | --- |
| 顧客行動分析 | 組織は、web サイトから顧客インタラクションイベントデータを取り込み、関連するイベントを Splunk に転送できます。その後、マーケティングチームと分析チームは、Splunk プラットフォーム内で後続の分析を実行して、主要ユーザーのインタラクションと行動を把握できます。 Splunk プラットフォームを使用して、グラフ、ダッシュボード、その他のビジュアライゼーションを生成し、ビジネス関係者に情報を提供できます。 |
| 大規模なデータセットでの拡張性の高い検索 | 組織は、web サイトからイベントデータとしてトランザクション入力や会話入力を取り込み、イベントを Splunk に転送できます。 その後、分析チームは、Splunk の拡張性の高いインデックス付け機能を活用して、大規模なデータセットをフィルタリングおよび処理し、ビジネスインサイトを導き出し、十分な情報に基づいた意思決定を行うことができます。 |

{style="table-layout:auto"}

## 前提条件 {#prerequisites}

この拡張機能を使用するには、Splunk アカウントが必要です。 Splunk アカウントは、[Splunk ホームページ](https://www.splunk.com/ja_jp/page/sign_up)で登録できます。

>[!NOTE]
>
> Splunk 拡張機能は、Splunk Cloud と Splunk Enterprise の両方のインスタンスをサポートします。このガイドは、[Splunk Cloud](https://www.splunk.com/ja_jp/products/splunk-cloud-platform.html) を使用した実装を参照してドキュメント化したものです。[Splunk Enterprise](https://www.splunk.com/ja_jp/products/splunk-enterprise.html) の設定プロセスは似ていますが、Splunk Enterprise 管理者からの具体的なガイダンスが必要です。

また、拡張機能を設定するには、次の技術的な値が必要です。

* [イベントコレクタートークン](https://docs.splunk.com/Documentation/Splunk/8.2.5/Data/UsetheHTTPEventCollector#Create_an_Event_Collector_token_on_Splunk_Cloud_Platform)。トークンは通常、`12345678-1234-1234-1234-1234567890AB` のような UUIDv4 形式です。
* 組織の Splunk プラットフォームインスタンスのアドレスとポート。通常、プラットフォームインスタンスのアドレスとポートの形式は、`mysplunkserver.example.com:443` です。
   >[!IMPORTANT]
   >
   > イベント転送内で参照される Splunk エンドポイントは、ポート `443` のみを使用する必要があります 。非標準ポートは、現在、イベント転送の実装ではサポートされていません。

## Splunk 拡張機能のインストール {#install}

UI に Splunk イベントコレクター拡張機能をインストールするには、**イベント転送**&#x200B;に移動し、拡張機能を追加するプロパティを選択するか、代わりに新しいプロパティを作成します。

目的のプロパティを選択または作成したら、**拡張機能**／**カタログ**&#x200B;に移動します。「[!DNL Splunk]」を検索し、Splunk 拡張機能の **[!DNL Install]** を選択します。

![UI で選択されている Splunk 拡張機能のインストールボタン](../../../images/extensions/server/splunk/install.png)

## Splunk 拡張機能の設定 {#configure_extension}

>[!IMPORTANT]
>
>実装のニーズに応じて、拡張機能を設定する前に、スキーマ、データ要素、データセットの作成が必要になる場合があります。ユースケースに合わせて設定する必要のあるエンティティを判断するには、開始する前にすべての設定手順を確認してください。

左側のナビゲーションの「**拡張機能**」をクリックします。**インストール済み**&#x200B;で、Splunk 拡張機能の「**設定**」を選択します。

![UI で選択されている Splunk 拡張機能の設定ボタン](../../../images/extensions/server/splunk/configure.png)

**[!UICONTROL HTTP イベントコレクター URL]** 用に、Splunk プラットフォームインスタンスのアドレスとポートを入力します。 **[!UICONTROL アクセストークン]**&#x200B;の下に、[!DNL Event Collector Token] の値を入力します。終了したら「**[!UICONTROL 保存]**」を選択します。

![UI に入力された設定オプション](../../../images/extensions/server/splunk/input.png)

## イベント転送ルールの設定 {#config_rule}

新しいイベント転送ルールの[ルール](../../../ui/managing-resources/rules.md)の作成を開始し、 必要に応じて条件を設定します。 ルールのアクションを選択する場合、[!UICONTROL Splunk] 拡張機能を選択してから、[!UICONTROL イベントを作成]アクションタイプを選択します。 追加のコントロールが表示され、Splunk イベントをさらに設定できます。

![アクション設定の定義](../../../images/extensions/server/splunk/action-configurations.png)

次の手順では、Splunk イベントプロパティを、以前に作成したデータ要素にマッピングします。設定可能な入力イベントデータに基づいて、サポートされているオプションのマッピングを以下に示します。 詳しくは、[Splunk のドキュメント](https://docs.splunk.com/Documentation/Splunk/8.2.5/Data/FormateventsforHTTPEventCollector#Event_metadata)を参照してください。

| フィールド名 | 説明 |
| --- | --- |
| [!UICONTROL イベント&#x200B;]<br><br>**（必須）** | イベントデータの提供方法を指定します。 イベントデータは、HTTP リクエストの JSON オブジェクト内の `event` キーに割り当てることも、生のテキストにすることもできます。この `event` キーは、JSON イベントパケット内のメタデータキーと同じレベルにあります。 `event` キーと値の中括弧内では、データを必要なあらゆる形式（文字列、数値、他の JSON オブジェクトなど）にすることができます。 |
| [!UICONTROL ホスト] | データを送信するクライアントのホスト名。 |
| [!UICONTROL ソースタイプ] | イベントデータに割り当てるソースタイプ。 |
| [!UICONTROL ソース] | イベントデータに割り当てるソース値。 例えば、開発中のアプリからデータを送信する場合は、このキーをアプリの名前に設定します。 |
| [!UICONTROL インデックス] | イベントデータのインデックスの名前。 トークンにインデックスパラメーターが設定されている場合、ここで指定するインデックスは、許可されたインデックスのリスト内にある必要があります。 |
| [!UICONTROL 時間] | イベント時間。 デフォルトの時間形式は UNIX 時間（`<sec>.<ms>` 形式）で、ローカルタイムゾーンによって異なります。例： `1433188255.500` は、エポックから 1433188255 秒と 500 ミリ秒後、すなわち 2015年6月1日（月）の午後 7 時:50: 55 分（GMT）を示します。 |
| [!UICONTROL フィールド] | インデックス時間に定義する明示的なカスタムフィールドを含む、生の JSON オブジェクトまたはキーと値のペアのセットを指定します。 この `fields` キーは、生データには適用されません。<br><br>`fields` プロパティを含むリクエストは、`/collector/event` エンドポイントに送信する必要があります。送信しない場合、インデックスが作成されません。詳しくは、[インデックス付きフィールド抽出](https://docs.splunk.com/Documentation/Splunk/8.2.5/Data/IFXandHEC)に関する Splunk のドキュメントを参照してください。 |

### Splunk 内のデータの検証 {#validate}

イベント転送ルールを作成して実行した後、Splunk API に送信されたイベントが Splunk UI で期待どおりに表示されるかどうかを検証します。 イベント収集と Experience Platform 統合が成功すると、Splunk コンソール内に次のようなイベントが表示されます。

![検証中に Splunk UI に表示されるイベントデータ](../../../images/extensions/server/splunk/splunk-data.png)

## 次の手順

このドキュメントでは、UI で Splunk イベント転送拡張機能をインストールおよび設定する方法について説明しました。 Splunk でのイベントデータの収集について詳しくは、次の公式ドキュメントを参照してください。

* [Splunk web での HTTP イベントコレクターの設定と使用](https://docs.splunk.com/Documentation/Splunk/8.2.5/Data/UsetheHTTPEventCollector)
* [トークンによる認証の設定](https://docs.splunk.com/Documentation/Splunk/8.2.5/Security/Setupauthenticationwithtokens#Prerequisites_for_activating_tokens)
* [HTTP イベントコレクターのトラブルシューティング](https://docs.splunk.com/Documentation/Splunk/8.2.5/Data/TroubleshootHTTPEventCollector)（[考えられるエラーコード](https://docs.splunk.com/Documentation/Splunk/8.2.5/Data/TroubleshootHTTPEventCollector#Possible_error_codes)の概要も表示）
