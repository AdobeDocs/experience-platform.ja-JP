---
title: 'クライアントサイドの情報 '
description: Webまたはモバイルアプリケーションのクライアント側でタグ操作を管理する方法を説明します。
source-git-commit: 39d9468e5d512c75c9d540fa5d2bcba4967e2881
workflow-type: tm+mt
source-wordcount: '197'
ht-degree: 49%

---

# クライアントサイドの情報

>[!NOTE]
>
>Adobe Experience Platform Launch は、Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

この節では、Adobe Experience Platformでクライアント側のタグ操作を管理する際に役立つ情報について説明します。

* [**タグ「_satellite」オブジェクト参照**](satellite-object.md)

   このリファレンスでは、`_satellite` オブジェクトと、このオブジェクトを使用して実行できる操作について説明します。

* [**Experience Cloud JavaScript の非同期デプロイメント**](asynchronous-deployment.md)

   製品で必要となるJavaScriptライブラリのパフォーマンスと、ノンブロッキングデプロイメントは、Adobe Experience Cloudユーザーにとってますます重要になっています。 [[!DNL Google PageSpeed]](https://developers.google.com/speed/pagespeed/insights/) などのツールは、自分たちのサイトでの [!DNL Adobe] ライブラリのデプロイ方法を変更するユーザーにお勧めです。この記事では、非同期的にAdobeJavaScriptライブラリを使用する方法を説明します。

* [**コンテンツセキュリティポリシー**](content-security-policy.md)（CSP）は、クロスサイトスクリプティング（XSS）などの特定タイプのブラウザーベースの攻撃を検出し、軽減するためのツールです。この記事では、タグ実装に対するCSPの影響と、それに関して何ができるかについて説明します。
