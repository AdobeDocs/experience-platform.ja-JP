---
title: メタコンバージョン API 拡張機能の概要
description: Adobe Experience Platformでのイベント転送のメタコンバージョン API 拡張機能について説明します。
source-git-commit: a47e35a1b8c7ce2b0fa4ffe30fcdc7d22fc0f4c5
workflow-type: tm+mt
source-wordcount: '819'
ht-degree: 2%

---

# [!DNL Meta Conversions API] 拡張機能の概要

この [[!DNL Meta Conversions API]](https://developers.facebook.com/docs/marketing-api/conversions-api/) では、サーバーサイドのマーケティングデータを [!DNL Meta] テクノロジーを使用して広告のターゲティングを最適化し、アクションあたりのコストを削減し、結果を測定します。 イベントは [[!DNL Meta Pixel]](https://developers.facebook.com/docs/meta-pixel/) ID とは、クライアント側のイベントと同様の方法で処理されます。

の使用 [!DNL Meta Conversions API] 拡張機能の場合は、 [イベント転送](../../../ui/event-forwarding/overview.md) にデータを送信するルール [!DNL Meta] をAdobe Experience Platform Edge Network から取得します。 このドキュメントでは、拡張機能のインストール方法と、イベント転送での拡張機能の使用方法について説明します [ルール](../../../ui/managing-resources/rules.md).

>[!NOTE]
>
>にイベントを送信しようとしている場合 [!DNL Meta] サーバー側ではなくクライアント側から、 [[!DNL Meta Pixiel] タグ拡張](../../client/meta/overview.md) 代わりに、

## 前提条件

拡張機能を使用するには、イベント転送にアクセスし、有効な [!DNL Meta] ～を利用できる口座 [!DNL Ad Manager] および [!DNL Event Manager]. 特に、既存の [[!DNL Meta Pixel]](https://www.facebook.com/business/help/952192354843755?id=1205376682832142) ( または [新しい [!DNL Pixel]](https://www.facebook.com/business/help/952192354843755) 代わりに )、拡張機能をアカウントに設定できます。

## 拡張機能のインストール

をインストールするには、以下を実行します。 [!DNL Meta Conversions API] 拡張機能に移動し、データ収集 UI またはExperience PlatformUI に移動して、「 」を選択します。 **[!UICONTROL イベント転送]** をクリックします。 ここから、拡張機能を追加するプロパティを選択するか、代わりに新しいプロパティを作成します。

目的のプロパティを選択または作成したら、「 」を選択します。 **[!UICONTROL 拡張機能]** 左側のナビゲーションで、 **[!UICONTROL カタログ]** タブをクリックします。 を検索します。 [!UICONTROL メタ変換 API] カードを選択し、 **[!UICONTROL インストール]**.

![この [!UICONTROL インストール] ボタンを選択しています [!UICONTROL メタ変換 API] 拡張機能を使用して、データ収集 UI に追加できます。](../../../images/extensions/server/meta/install.png)

表示される設定ビューで、 [!DNL Pixel] 拡張機能をアカウントにリンクするために以前にコピーした ID。 ID を直接入力に貼り付けることも、代わりにデータ要素を使用することもできます。

また、 [!DNL Conversions API] 特に。 詳しくは、 [!DNL Conversions API] ドキュメント [アクセストークンの生成](https://developers.facebook.com/docs/marketing-api/conversions-api/get-started#access-token) を参照してください。

終了したら、「 」を選択します。 **[!UICONTROL 保存]**

![この [!DNL Pixel] 拡張機能の設定表示でデータ要素として指定された ID。](../../../images/extensions/server/meta/configure.png)

拡張機能がインストールされ、タグルールでその機能を使用できるようになりました。

## イベント転送ルールの設定 {#rule}

新しいイベント転送ルールの作成を開始し、必要に応じて条件を設定します。 ルールのアクションを選択する場合は、「 **[!UICONTROL メタコンバージョン API 拡張機能]** 拡張機能に対して、「 」を選択します。 **[!UICONTROL コンバージョン API イベントの送信]** （アクションタイプ）。

![この [!UICONTROL ページビューを送信] データ収集 UI のルールに対して選択されているアクションタイプ。](../../../images/extensions/server/meta/select-action.png)

送信先のイベントデータを設定できるコントロールが表示されます。 [!DNL Meta] 経由 [!DNL Conversions API]. これらのオプションは、指定された入力に直接入力することも、代わりに既存のデータ要素を選択して値を表すこともできます。 設定オプションは、以下に示すように、4 つの主なセクションに分かれています。

| Config セクション | 説明 |
| --- | --- |
| [!UICONTROL サーバーイベントパラメーター] | イベントの発生時刻やイベントをトリガーしたソースアクションを含む、イベントに関する一般情報。 詳しくは、 [[!DNL Conversions API] ドキュメント](https://developers.facebook.com/docs/marketing-api/conversions-api/parameters/server-event) を参照してください。<br><br>また、次のオプションもあります。 **[!UICONTROL 限定的なデータ使用を有効にする]** 顧客のオプトアウトに準拠するため。 詳しくは、 [!DNL Conversions API] ドキュメント [データ処理オプション](https://developers.facebook.com/docs/marketing-apis/data-processing-options/) を参照してください。 |
| [!UICONTROL 顧客情報パラメーター] | イベントを顧客に関連付けるために使用されるユーザー ID データ。 これらの値の一部は、API に送信する前にハッシュ化する必要があります。 詳しくは、 [[!DNL Conversions API] ドキュメント](https://developers.facebook.com/docs/marketing-api/conversions-api/parameters/customer-information-parameters) を参照してください。 |
| [!UICONTROL カスタムデータ] | 広告配信の最適化に使用する追加データ。JSON オブジェクトの形式で提供されます。 詳しくは、 [[!DNL Conversions API] ドキュメント](https://developers.facebook.com/docs/marketing-api/conversions-api/parameters/custom-data) を参照してください。<br><br>購入イベントを送信する場合は、このセクションを使用して必要な属性を指定する必要があります `currency` および `value`. |
| [!UICONTROL テストイベント] | このオプションは、設定がサーバーイベントの受信を引き起こしているかどうかを検証するために使用します。 [!DNL Meta] 期待どおり この機能を使用するには、 **[!UICONTROL テストイベントとして送信]** チェックボックスに移動し、以下の入力で任意のテストイベントコードを指定します。 イベント転送ルールをデプロイすると、拡張機能とアクションを正しく設定した場合は、内にアクティビティが表示されます **[!DNL Test Events]** 表示 [!DNL Meta Events Manager]. |

{style=&quot;table-layout:auto&quot;}

終了したら、「 」を選択します。 **[!UICONTROL 変更を保持]** をクリックして、ルール設定にアクションを追加します。

![[!UICONTROL 変更を保持] を選択してアクションの設定に使用します。](../../../images/extensions/server/meta/keep-changes.png)

ルールに問題がない場合は、「 」を選択します。 **[!UICONTROL ライブラリに保存]**. 最後に、新しいイベント転送を公開します [ビルド](../../../ui/publishing/builds.md) ライブラリに対する変更を有効にします。

## 次の手順

このガイドでは、サーバー側のイベントデータをに送信する方法について説明しました。 [!DNL Meta] の使用 [!DNL Meta Conversions API] 拡張子。 タグとイベントの転送について詳しくは、 [タグの概要](../../../home.md).
