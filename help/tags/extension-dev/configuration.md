---
title: 拡張機能の設定
description: Adobe Experience Platform のデータ収集 UI で、ユーザーからグローバル設定を収集するようにタグ拡張を設定する方法について説明します。
exl-id: 2bf33617-1398-499f-8325-3849dbdb1f97
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 100%

---

# 拡張機能の設定

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../term-updates.md)を参照してください。

拡張機能の設定に応じて、ユーザーからグローバル設定が収集されます。例えば、ユーザーが「ビーコンを送信」アクションを使用してビーコンを送信できる拡張機能について考えてみます。このビーコンには常にアカウント ID を含める必要があります。「ビーコンを送信」アクションを設定するたびにアカウント ID を尋ねられるという煩わしい体験をユーザーにさせたくはありません。代わりに、拡張機能の設定表示から、アカウント ID を 1 回だけ要求します。ビーコンが送信されるたびに、「ビーコンを送信」アクションのライブラリモジュールで拡張機能設定からアカウント ID を取得し、ビーコンに追加できます。

ユーザーが Adobe Experience Platform のタグプロパティに拡張機能をインストールすると、拡張機能によって提供される拡張機能の設定が表示されます。拡張機能をインストールするには、拡張機能の設定を完了する必要があります。拡張機能の設定表示を作成する方法については、[表示](./web/views.md)に関するドキュメントを参照してください。

拡張機能の設定ビューから設定を保存した後、タグのランタイムライブラリで設定が発行されます。その後、[`turbine.getExtensionSettings()`](./turbine.md#get-extension-settings) を呼び出すことで、拡張機能ライブラリモジュールからこれらの設定にアクセスできます。

拡張機能の設定はオプションの機能で、活用しないことも選択できます。
