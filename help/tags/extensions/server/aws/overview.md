---
title: AWS拡張機能の概要
description: Adobe Experience Platformでのイベント転送用のAWS拡張機能について説明します。
exl-id: 826a96aa-2d64-4a8b-88cf-34a0b6c26df5
last-substantial-update: 2022-11-23T00:00:00Z
source-git-commit: c2832821ea6f9f630e480c6412ca07af788efd66
workflow-type: tm+mt
source-wordcount: '810'
ht-degree: 7%

---

# [!DNL AWS] 拡張機能の概要

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語が変更されました。用語の変更点の一覧については、次の[ドキュメント](../../../term-updates.md)を参照してください。

[[!DNL Amazon Web Services]  （[!DNL AWS]） &#x200B;](https://aws.amazon.com/jp/) は、分散コンピューティング、データベースストレージ、コンテンツ配信、顧客関係管理（CRM）やエンタープライズリソースプランニング（ERP）向けの SaaS （software-as-a-service）統合サービスなど、様々なサービスを提供するクラウドコンピューティングプラットフォームです。

[!DNL AWS] [&#x200B; イベント転送 &#x200B;](../../../ui/event-forwarding/overview.md) 拡張機能は、[[!DNL Amazon Kinesis Data Streams]](https://docs.aws.amazon.com/streams/latest/dev/introduction.html) を活用して、イベントをAdobe Experience Platform Edge Networkから [!DNL AWS] に送信して、さらに処理できるようにします。 このガイドでは、拡張機能をインストールし、イベント転送ルールでその機能を使用する方法について説明します。

## 前提条件

この拡張機能を使用するには、既存の [!DNL Kinesis] データストリームを持つ [!DNL AWS] アカウントが必要です。 既存のデータストリームがない場合は、[&#x200B; 管理コンソールを使用した新しいデータストリームの作成 &#x200B;](https://docs.aws.amazon.com/streams/latest/dev/how-do-i-create-a-stream.html) に関する [!DNL AWS] のドキュメントを参照し  [!DNL AWS]  ください。

## 拡張機能のインストール {#install}

[!DNL AWS] 拡張機能をインストールするには、データ収集 UI またはExperience PlatformUI に移動し、左側のナビゲーションから **[!UICONTROL イベント転送]** を選択します。 ここから、拡張機能を追加するプロパティを選択するか、代わりに新しいプロパティを作成します。

目的のプロパティを選択または作成したら、左側のナビゲーションで **[!UICONTROL 拡張機能]** を選択し、「**[!UICONTROL カタログ]**」タブを選択します。 [!UICONTROL AWS] カードを検索し、「**[!UICONTROL インストール]**」を選択します。

![&#x200B; データ収集 UI の [!UICONTROL AWS] 拡張機能に対して選択されている「[!UICONTROL &#x200B; インストール &#x200B;]」ボタン。](../../../images/extensions/server/aws/install.png)

次の画面で、[!DNL AWS] アカウントの接続資格情報を指定する必要があります。 特に、[!DNL AWS] アクセスキー ID と秘密アクセスキーを指定する必要があります。 これらの値がわからない場合は、[!DNL AWS] のドキュメント [&#x200B; アクセスキー ID と秘密アクセスキーを取得する方法 &#x200B;](https://docs.aws.amazon.com/powershell/latest/userguide/pstools-appendix-sign-up.html) を参照してください。

![&#x200B; 拡張機能の設定ビューに追加されたアクセスキー ID と秘密アクセスキー。](../../../images/extensions/server/aws/credentials.png)

>[!IMPORTANT]
>
>アクセス資格情報の生成に使用する [!DNL AWS] アカウントにアクセスポリシーを添付する必要があります。 このポリシーは、[!DNL Kinesis] のデータストリームにデータを送信するためのアクセス権を付与するように設定する必要があります。 ポリシーの定義方法については、[&#x200B; のポリシーの例  [!DNL Kinesis Data Streams]](https://docs.aws.amazon.com/streams/latest/dev/controlling-access.html#kinesis-using-iam-examples) に関する [!DNL AWS] ドキュメントの **例 2** を参照してください。

終了したら、「**[!UICONTROL 保存]** を選択すると、拡張機能がインストールされます。

## イベント転送ルールの設定 {#rule}

拡張機能をインストールした後、新しいイベント転送 [&#x200B; ルール &#x200B;](../../../ui/managing-resources/rules.md) を作成し、必要に応じてその条件を設定します。 ルールのアクションを設定する際に、**[!UICONTROL AWS]** 拡張機能を選択してから、アクションタイプとして **[!UICONTROL Kinesis データストリームにデータを送信]** を選択します。

![&#x200B; データ収集 UI でルールに対して選択されている「[!UICONTROL Kinesis データストリームにデータを送信 &#x200B;]」アクションタイプ &#x200B;](../../../images/extensions/server/aws/select-action-type.png)

右側のパネルが更新され、データの送信方法に関する設定オプションが表示されます。 特に、[!DNL Event Hub] 設定を表す様々なプロパティに [&#x200B; データ要素 &#x200B;](../../../ui/managing-resources/data-elements.md) を割り当てる必要があります。

![UI に表示される [!UICONTROL Kinesis データストリームにデータを送信 &#x200B;] アクションタイプの設定オプション &#x200B;](../../../images/extensions/server/aws/data-stream-details.png)

**[!UICONTROL Kinesis データストリームの詳細]**

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL &#x200B; ストリーム名 &#x200B;] | このイベント転送ルールがデータレコードを送信するストリームの名前。 |
| [!UICONTROL AWS リージョン] | [!DNL Kinesis] データストリームが作成される [!DNL AWS] 領域。 |
| [!UICONTROL &#x200B; パーティションキー &#x200B;] | データ ストリームにデータを送信するときに拡張機能が使用する [&#x200B; パーティション キー &#x200B;](https://docs.aws.amazon.com/streams/latest/dev/key-concepts.html#partition-key)。<br><br>[!DNL Kinesis Data Streams] は、ストリームに属するデータレコードを複数のシャードに分離します。 各データレコードと共に送信されるパーティションキーを使用して、特定のデータレコードが属するシャードを判断します。<br><br> 顧客ごとに異なるので、顧客を配布するための良いパーティションキーは、顧客番号かもしれません。 パーティション キーが不適切な場合、近くの同じエリアに住んでいる可能性があるため、郵便番号が異なる可能性があります。 一般に、パーティションキーは異なる可能性のある値の範囲が最も大きいものを選択します。 パーティションキーの管理のベストプラクティスについては、[&#x200B; データストリームのスケーリング &#x200B;](https://aws.amazon.com/blogs/big-data/under-the-hood-scaling-your-kinesis-data-streams/) に関する [!DNL AWS] の記事を参照してください  [!DNL Kinesis]  |

{style="table-layout:auto"}

**[!UICONTROL データ]**

| 入力 | 説明 |
| --- | --- |
| [!UICONTROL &#x200B; ペイロード &#x200B;] | このフィールドには、[!DNL Kinesis] データストリームに転送されるデータが JSON 形式で格納されます。<br><br> 「**[!UICONTROL Raw]**」オプションで、JSON オブジェクトを指定されたテキストフィールドに直接貼り付けるか、データ要素アイコン（![&#x200B; データセットアイコン &#x200B;](/help/images/icons/database.png)）を選択して、ペイロードを表す既存のデータ要素のリストから選択できます。<br><br> また、「**[!UICONTROL JSON キーと値のペア エディター]**」オプションを使用し、UI エディターを使用して各キーと値のペアを手動で追加することもできます。 各値は、生の入力で表すことも、代わりにデータ要素を選択することもできます。 |

{style="table-layout:auto"}

完了したら、「**[!UICONTROL 変更を保持]**」を選択して、アクションをルール設定に追加します。 ルールの設定が完了したら、「**[!UICONTROL ライブラリに保存]**」を選択します。

最後に、新しいイベント転送 [&#x200B; ビルド &#x200B;](../../../ui/publishing/builds.md) を公開して、ライブラリに対する変更を有効にします。

## 次の手順

このガイドでは、[!DNL AWS] イベント転送拡張機能を使用して [!DNL Kinesis Data Streams] にデータを送信する方法について説明しました。 Experience Platformのイベント転送機能について詳しくは、[&#x200B; イベント転送の概要 &#x200B;](../../../ui/event-forwarding/overview.md) を参照してください。
