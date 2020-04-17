---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: プライバシー要求のIDデータ
topic: overview
translation-type: tm+mt
source-git-commit: a1161630c8edae107b784f32ee20af225f9f8c46

---


# プライバシー要求のIDデータ

Adobe Experience Platformプライバシーサービスで個人データの顧客の要求（アクセス、削除、オプトアウトの要求を含む）を処理するには、Adobe Experience Cloud対応のアプリケーションに保存された個人データに特定の顧客をリンクする一意の識別子を提供する必要があります。 次に、プライバシーサービスはこれらの識別子を使用して、顧客のIDの下に保存されたすべてのデータをExperience Cloud内で収集し、顧客の要求に従って処理します。

このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシーリクエストに適したID情報を効果的に取得する方法に関する一般的なガイダンスを提供します。

## IDと名前空間

顧客が複数の異なるチャネルを通じてブランドとやり取りできる場合、多数のインタラクションから記録される異なる識別子を調整するのは困難な場合があります。 その結果、Experience Cloudアプリケーションの特定のユーザーに属するデータを特定するのが困難になる可能性があります。

例えば、プライバシーサービスで顧客データリクエストを処理する場合、IDは、アドビが制御するドメインに設定したcookie値、サードパーティドメインに設定してアドビと共有したcookie値、またはIMS組織内で明示的に定義したカスタム識別子を表します。

したがって、プライバシーサービスに送信される各IDには、ID値を接触チャネルのシステムに関連付けることによってコンテキストを提供する **** 名前空間が付属する必要があります。 名前空間は、電子メールアドレス（「電子メール」）などの汎用概念を表すことや、IDを特定のアプリケーション(Adobe Advertising Cloud ID(「AdCloud」)やAdobeターゲットID(「TNTID」)など)に関連付けることができます。

Adobe Experience Platform Identity Serviceは、グローバルに定義されたユーザー定義のID名前空間を管理します。 名前空間について詳しくは、ID名前空間の概要を参照し [てください](../identity-service/namespaces.md)。 プライバシーサービスで一般的に使用される標準名前空間と名前空間修飾子のリストについては、開発者ガ [イドの付録の節](api/appendix.md) を参照してください。

## ECIDとオプトインサービス

Adobe Experience Cloud Identity Serviceは、Experience Cloudの共通の識別フレームワークとして機能し、各サイト訪問者に一意の永続的なIDを割り当てます。 Experience Cloud ID(ECID)は、ファーストパーティCookieを使用して顧客のアクティビティを追跡し、複数のアプリケーションでデバイスを一意に識別し、同じサイトの訪問者とそのデータを異なるExperience Cloudアプリケーションで識別できます。 See the [Experience Cloud Identity Service overview](https://docs.adobe.com/content/help/ja-JP/id-service/using/intro/overview.html) for more information.

Experience Cloud IDサービスの拡張機能であるオプトインサービスを使用すると、訪問者に対してプロトコルを設定し、訪問者がのデバイスまたはブラウザーにcookieを設定できるかどうかを判断できるようにできます。 ご使用のアプリケーションに対するサービスの設定方法など、オプトインサービスに関する詳細は、オプトインサービスのドキュメントを [参照してください](https://docs.adobe.com/content/help/ja-JP/id-service/using/implementation/opt-in-service/optin-overview.html)。

サイト訪問者にECIDが割り当てられると、次の節で説明するように、AdobeプライバシーJavaScriptライブラリを利用して、プライバシーリクエストで使用するIDを取得できます。

## プライバシーJSライブラリ

アドビのプライバシーJavaScriptライブラリには、ブラウザーに保存されている顧客IDを取得および削除できる機能がいくつか用意されています。 ライブラリは、ECIDを含む複数のアドビアプリケーションからID情報を取得するように設定できます。 コールバックまたはプロミスを使用することで、IDの取得が成功した場合にプログラムで処理し、それらをプライバシーサービスAPIに送信できます。

一般的な使用例のコードサンプルを含む、プライバシーJSライブラリについて詳しくは、プライバシーJSライブラリの概要 [を参照してください](js-library.md)。

## 次の手順

このドキュメントでは、プライバシーリクエストで使用する顧客IDデータの取得に関する中心的な概念の概要を簡単に説明しました。 これらの概念とサービスの詳細については、各セクションに記載されているドキュメントのリンクを確認することをお勧めします。 取得したIDをプライバシーサービスに送信して、アクセス、削除、またはオプトアウトオブセールのリクエストを作成する手順については、プライバシーサービス開発者ガイドを参 [照してください](api/getting-started.md)。