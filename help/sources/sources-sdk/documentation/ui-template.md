---
keywords: Experience Platform;ホーム;人気の高いトピック;ソース;コネクタ;ソースコネクタ;ソース sdk;SDK;SDK
title: UI 用のセルフサービスドキュメントテンプレート
description: Adobe Experience Platform UI を使用して YOURSOURCE ソース接続を作成する方法を説明します。
exl-id: 6471c0a2-22e8-4133-a76f-ee3c5c669ef8
source-git-commit: 1ed82798125f32fe392f2a06a12280ac61f225c6
workflow-type: tm+mt
source-wordcount: '727'
ht-degree: 22%

---

# の作成 *YOURSOURCE* UI のソース接続

*このテンプレートの操作中に、斜体のすべての段落を置き換えるか削除します（この段落から始まります）。*

*まず、ページ上部のメタデータ（タイトルと説明）を更新します。 このページの UICONTROL のすべてのインスタンスを無視してください。 これは、機械翻訳プロセスがページをサポートする複数の言語に正しく翻訳するのに役立つタグです。 お客様がドキュメントを送信した後、ドキュメントにタグを追加します。*

このチュートリアルでは、 *YOURSOURCE* Platform ユーザーインターフェイスを使用したソースコネクタ

## 概要

*顧客に提供する価値を含め、会社の概要を短く示します。 詳しくは、製品ドキュメントのホームページへのリンクを含めてください。*

>[!IMPORTANT]
>
>このソースコネクタとドキュメントページは、 *YourSource* チーム。 お問い合わせや更新のご依頼は、直接お問い合わせください。 *更新用にアクセスできるリンクまたは電子メールアドレスを挿入します*.

## 前提条件

*Adobe Experience Platformユーザーインターフェイスでソースの設定を開始する前に顧客が認識しておく必要がある事項に関する情報を、この節に追加します。 次のような場合が考えられます。*

* *許可リストに追加する必要がある*
* *電子メールハッシュの要件*
* *お客様側のアカウントの詳細*
* *プラットフォームに接続するための認証資格情報の取得方法*

### 必要な資格情報の収集

接続するには *YOURSOURCE* を Platform に対して、次の接続プロパティの値を指定する必要があります。

| 資格情報 | 説明 | 例 |
| --- | --- | --- |
| *資格情報 1* | *ここでソースの認証資格情報に簡単な説明を追加してください* | *ここにソースの認証資格情報の例を追加してください* |
| *資格情報 2* | *ここでソースの認証資格情報に簡単な説明を追加してください* | *ここにソースの認証資格情報の例を追加してください* |
| *資格 3* | *ここでソースの認証資格情報に簡単な説明を追加してください* | *ここにソースの認証資格情報の例を追加してください* |

これらの資格情報について詳しくは、 *YOURSOURCE* 認証に関するドキュメント。 *ここにプラットフォームの認証に関するドキュメントへのリンクを追加してください*.

## 接続する *YOURSOURCE* アカウント

Platform UI の左側のナビゲーションバーで「**[!UICONTROL ソース]**」を選択し、[!UICONTROL ソース]ワークスペースにアクセスします。[!UICONTROL カタログ]画面には、アカウントを作成できる様々なソースが表示されます。

画面の左側にあるカタログから適切なカテゴリを選択することができます。または、使用する特定のソースを検索オプションを使用して探すこともできます。

の下 *ソースのカテゴリ* カテゴリ、選択 *YOURSOURCE*&#x200B;を選択し、 **[!UICONTROL データを追加]**.

>[!TIP]
>
>以下に使用するスクリーンショットは例です。 ドキュメントを作成する際は、画像を実際のソースのスクリーンショットに置き換えてください。 同じマークアップパターンと色、同じファイル名を使用できます。 スクリーンショットに Platform UI 画面全体が取り込まれていることを確認してください。 スクリーンショットのアップロード方法について詳しくは、 [ドキュメントのレビュー用の送信](./github.md).

![カタログ](../assets/ui/catalog.png)

The **[!UICONTROL YOURSOURCE アカウントに接続]** ページが表示されます。 このページでは、新しい資格情報または既存の資格情報を使用できます。

### 既存のアカウント

既存のアカウントを使用するには、 *YOURSOURCE* 新しいデータフローを作成するアカウントを選択し、 **[!UICONTROL 次へ]** をクリックして続行します。

![既存](../assets/ui/existing.png)

### 新しいアカウント

新しいアカウントを作成する場合は、「**[!UICONTROL 新規アカウント]**」を選択し、続けて名前、説明（オプション）、 の資格情報を指定します。終了したら「**[!UICONTROL ソースに接続]**」を選択し、新しい接続が確立されるまでしばらく待ちます。

![新規](../assets/ui/new.png)

## 次の手順

*データフローの作成の残りの手順のワークフローはモジュール化されます。 ソースに関する特定のコールアウトが必要な場合は、以下の追加のリソースの節を参照してください。*

このチュートリアルに従うことで、 *YOURSOURCE* アカウント。 次のチュートリアルに進み、[データを Platform に取り込むためのデータフローの設定](https://experienceleague.adobe.com/docs/experience-platform/sources/ui-tutorials/dataflow/crm.html)を行いましょう。

## その他のリソース

*このセクションはオプションで、製品ドキュメントへの詳細なリンクや、その他の手順、スクリーンショット、ニュアンスを提供して、顧客が成功するために重要と考えるものへのリンクを提供できます。 このセクションを使用して、ソースのワークフロー全体に関する情報やヒントを追加できます。特に、エンドユーザーが遭遇する可能性のある特定の「了解事項」がある場合に役立ちます。*
