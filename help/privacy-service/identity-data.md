---
keywords: Experience Platform;ホーム;人気のトピック;ECID;ecid
solution: Experience Platform
title: プライバシーリクエスト用のID データ
description: このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。
exl-id: 43b0292a-ea4d-4858-b584-ba71029724f6
source-git-commit: 36871289743f384207bb149df6e5e1af14d4d371
workflow-type: tm+mt
source-wordcount: '629'
ht-degree: 35%

---

# プライバシーリクエストの ID データ

Adobe Experience Platform [!DNL Privacy Service]が個人データに対するお客様のリクエスト（アクセス、削除、オプトアウトのリクエストを含む）を処理するには、Adobe Experience Cloud対応アプリケーションに保存されている個人データに特定のお客様をリンクする一意のIDを指定する必要があります。 [!DNL Privacy Service]は、これらの識別子を使用して、[!DNL Experience Cloud]内の顧客のIDに格納されているすべてのデータを収集し、顧客の要求に従って処理します。

このドキュメントでは、データ操作を設定し、アドビのテクノロジーを活用して、顧客のプライバシー要求に適した ID 情報を効果的に取得する方法について、一般的なガイダンスを提供しています。

## ID と名前空間

顧客が複数の異なるチャネルを通じてブランドとやり取りできる場合、多数のインタラクションから記録される様々な識別子を紐付けるのは困難なことがあります。これにより、どのデータが[!DNL Experience Cloud] アプリケーションの特定のユーザーに属しているかを判断するのが困難になる可能性があります。

例えば、[!DNL Privacy Service]で顧客データリクエストを処理する場合、IDは、Adobeが管理するドメインに設定されたcookie値、サードパーティドメインのcookie値、Adobeと共有されたcookie値、または組織内で明示的に定義されたカスタム IDを表すことができます。

したがって、[!DNL Privacy Service]に送信される各IDには、ID値を発信元システムに関連付けることでコンテキストを提供する名前空間が付属している必要があります。 名前空間は、電子メールアドレス（「電子メール」）などの一般的な概念を表したり、IDをAdobe Advertising IDやAdobe Target IDなどの特定のアプリケーションに関連付けたりできます。

Adobe Experience Platform ID サービスは、グローバルに定義された、またユーザー定義の ID 名前空間を管理します。名前空間について詳しくは、[ID 名前空間の概要](../identity-service/features/namespaces.md)を参照してください。[!DNL Privacy Service]で一般的に使用される標準の名前空間と名前空間修飾子の一覧については、API ガイドの[付録セクション &#x200B;](api/appendix.md)を参照してください。

## ECID とオプトインサービス

Adobe Experience Cloud [!DNL Identity Service]は、[!DNL Experience Cloud]の共通ID フレームワークとして機能し、各サイト訪問者に一意の永続的なIDを割り当てます。 [!DNL Experience Cloud] ID （ECID）は、ファーストパーティ Cookieを使用して顧客のアクティビティを追跡し、複数のアプリケーションをまたいでデバイスを一意に識別でき、異なる[!DNL Experience Cloud] アプリケーションで同じサイト訪問者とそのデータを識別できます。 詳しくは、[Experience Cloud ID サービスの概要](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html?lang=ja)を参照してください。

オプトインサービスは[!DNL Experience Cloud Identity Service]の拡張機能で、訪問者が訪問者のデバイスまたはブラウザーにCookieを設定できるかどうかを訪問者が判断できるように、アプリケーションにプロトコルを設定できます。 アプリケーションでのサービスの設定方法など、オプトインサービスについて詳しくは、[オプトインサービスに関するドキュメント](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja)を参照してください。

サイト訪問者にECIDが割り当てられたら、次の節で説明するように、Adobe [!DNL Privacy JavaScript Library]を使用して、プライバシーリクエストで使用するためにこれらのIDを取得できます。

## [!DNL Privacy JS Library]

[!DNL Adobe Privacy JavaScript Library]には、ブラウザーに保存されている顧客IDを取得および削除できる機能がいくつか用意されています。 複数のアドビアプリケーションから ECID を含む ID 情報を取得するように、ライブラリを設定できます。コールバックまたはプロミスを使用することで、正常に取得されたIDをプログラムで処理し、[!DNL Privacy Service] APIに送信できます。

いくつかの一般的なユースケースのコードサンプルを含む[!DNL Privacy JS Library]の詳細については、[&#x200B; プライバシーJS ライブラリの概要](js-library.md)を参照してください。

## 次の手順

このドキュメントでは、プライバシーリクエストで使用する顧客 ID データの取得に関する主要概念を簡単に説明しました。これらの概念とサービスについて詳しくは、各節に記載されているドキュメントへのリンクを確認することをお勧めします。取得したIDを[!DNL Privacy Service]に送信して、アクセス要求、削除要求、または販売停止要求を作成する手順については、[Privacy Service API ガイド &#x200B;](api/overview.md)を参照してください。
