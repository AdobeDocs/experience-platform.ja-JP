---
title: 顧客の同意を管理する JavaScript タグのデプロイ
description: Adobe Experience Platform の様々なアドビソリューション用に顧客のオプトインおよびオプトアウトシグナルを管理する方法について説明します。
source-git-commit: 7e27735697882065566ebdeccc36998ec368e404
workflow-type: ht
source-wordcount: '651'
ht-degree: 100%

---

# 顧客の同意を管理する JavaScript タグのデプロイ

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

[EU 一般データ保護規則（GDPR）](https://gdpr-info.eu/art-7-gdpr/)と [ePrivacy](https://medium.com/mydata/consent-lost-gdpr-and-found-eprivacy-e85cf881ffb) 法により、企業には、ユーザーの同意を管理できる能力が求められます。アドビのお客様は、任意の訪問者に対してアドビソリューションを実行する前に、その訪問者にオプトインを求めることができます。訪問者は、オプトインとオプトアウトのステータスを管理できます。

Adobe Experience Cloud のお客様には、これらの要件の様々な実装が必要になります。エンタープライズレベルの同意マネージャを使用しているお客様や、自分たちで構築しているお客様もいます。

Adobe Experience Platform 拡張機能の開発者は、拡張機能とルールビルダーを使用して、オプトインおよびオプトアウトソリューションを定義します。

このドキュメントでは、同意が得られるまでアドビのタグが実行されないようにする方法について説明します。

## Advertising Cloud

Adobe Experience Platform は、[!DNL Advertising Cloud] を自動的に起動しません。 [!DNL Advertising Cloud] は、ルールアクションで具体的に指定する場合にのみ発生します。いつ実行するかを決定するには、ルール条件を使用します。例えば、cookie を使用してオプトインステータスを決定するには、データ要素を設定してそ のcookie を読み取り、ルールの条件として使用して、「Track Conversion」アクションを実行するタイミングを決定します。

同意マネージャ（OneTrust など）との統合により、顧客の同意 cookie を設定および追跡した後、ルールビルダーで使用できます。

## Analytics

[!DNL Analytics] 拡張機能の設定で、以下が選択されて&#x200B;*いない*&#x200B;ことを確認します。

* ダウンロードリンクを追跡
* 離脱リンクを追跡

これらの設定が選択されていない場合、Platform は自動的には [!DNL Adobe Analytics] を起動しません。[!DNL Analytics] ルールは、ルールアクションで具体的に指定された場合にのみ実行します。いつ実行するかを決定するには、ルール条件を使用します。例えば、cookie を使用してオプトインステータスを決定するには、データ要素を設定してそ のcookie を読み取り、ルールの条件として使用して、「Send Beacon」アクションを実行するタイミングを決定します。

これとは別に、[アドビのオプトインオブジェクト](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja)を使用して、同意管理プラットフォームと連携させてこのタグの実行を制御することを検討できます。

同意マネージャ（OneTrust など）との統合により、顧客の同意 cookie を設定および追跡した後、ルールビルダーで使用できます。

## Audience Manager

現在、DIL を顧客ページに配置すると、自動的に実行されます。[アドビのオプトインオブジェクト](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja)を使用して、同意管理プラットフォームと連携させてこのタグの実行を制御することを検討してください。

[!DNL Adobe] では、[!DNL Analytics] 内でサーバー側転送を使用することをお勧めします。

## Experience Cloud ID

現在、[!DNL Experience Cloud ID] を顧客ページに配置すると、自動的に実行されます。

[アドビのオプトインオブジェクト](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja)を使用して、同意管理プラットフォームと連携させてこのタグの実行を制御することを検討してください。

## Target

Adobe Experience Platform は、[!DNL Target] を自動的に起動しません。 [!DNL Target] ルールは、ルールアクションで具体的に指定された場合にのみ実行します。いつ実行するかを決定するには、ルール条件を使用します。例えば、cookie を使用してオプトインステータスを決定するには、データ要素を設定してそ のcookie を読み取り、ルールの条件として使用して、「Load [!DNL Target]」アクションを実行するタイミングを決定します。

これとは別に、[アドビのオプトインオブジェクト](https://experienceleague.adobe.com/docs/id-service/using/implementation/opt-in-service/optin-overview.html?lang=ja)を使用して、同意管理プラットフォームと連携させてこのタグの実行を制御することを検討できます。

同意マネージャ（OneTrust など）との統合により、顧客の同意 cookie を設定および追跡した後、ルールビルダーで使用できます。
