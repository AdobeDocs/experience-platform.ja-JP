---
title: Experience Cloud 組織の設定
description: Adobe Experience Platform の拡張機能の開発を開始するために Adobe Experience Cloud 組織を登録する方法について説明します。
exl-id: ee36319d-5de8-462e-879b-311445cf334c
source-git-commit: a8b0282004dd57096dfc63a9adb82ad70d37495d
workflow-type: tm+mt
source-wordcount: '608'
ht-degree: 100%

---

# Experience Cloud 組織の設定

>[!NOTE]
>
>Adobe Experience Platform Launch は、Adobe Experience Platform のデータ収集テクノロジースイートとしてリブランドされています。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更点の一覧については、次の[ドキュメント](../../term-updates.md)を参照してください。

Adobe Experience Platform でタグ拡張機能を開発するには、Adobe Experience Cloud 組織を登録する必要があります。

Experience Cloud の顧客には、契約が締結される際に組織が割り当てられます。既存のお客様で、既に組織が登録されている場合は、このドキュメントをスキップし、[拡張機能開発のためのユーザーアクセスの許可](./access.md)に関するガイドに進むことができます。Experience Cloud の顧客でない場合は、次に示す Adobe パートナープログラムの 1 つに参加することで組織を作成できます。

## Exchange パートナープログラム

このプログラムは、アドビを補完し、アドビテクノロジーと統合して顧客に価値を提供できるテクノロジーを持つパートナーを対象としています。このプログラムでは、パートナーが統合を促進するためのリストを作成できる[マーケットプレイス](https://www.adobeexchange.com/experiencecloud.html)が管理されています。登録ガイドは[こちら](https://partners.adobe.com/exchangeprogram/experiencecloud/reg-guide.html)で確認できます。また、登録は[こちら](https://partners.adobe.com/exchangeprogram/experiencecloud/prereg.html)から開始できます。

Marketplace に表示される Adobe Experience Platform の公開タグ拡張機能を作成するには、このプログラムのメンバーである必要があります。

>[!WARNING]
>
>前述のリンクから Exchange パートナープログラムに登録すると、6 か月後、Innovate パートナーとして参加した場合に請求される料金についての文章が表示される場合があります。この料金は、拡張機能を作成する目的でのみ Exchange プログラムに参加している 拡張機能の開発者には適用されません。いかなる料金も請求されません。

## ソリューションパートナープログラム（SPP）

このプログラムは、アドビの顧客が投資を最大限に活用し、アドビソリューションを再販するパートナーを支援するコンサルティング会社向けです。ソリューションパートナープログラムへの参加方法に関するステップバイステップガイドは、[Adobe Spark の Web サイト](https://spark.adobe.com/page/7PKZzIJJjkcDd/)に移動するか、[ソリューションパートナープログラムサイト](https://solutionpartners.adobe.com/home.html)を参照してください。

>[!NOTE]
>
>Exchange の契約には、Adobe Experience Platform の拡張機能開発に関する利用条件が含まれているので、ソリューションパートナーは Exchange パートナープログラムに[登録](https://partners.adobe.com/exchangeprogram/experiencecloud/prereg.html)する必要があります。
>
>Exchange への登録を完了する前に、Exchange 管理者（<ExchangeHelpEC@adobe.com>）に電子メールを送信し、 の拡張機能を宣伝するために登録しようとしていることを説明してください。これをおこなわないと、アプリケーションが却下され、代わりに SPP を参照する可能性があります。
>
>現在、会社の電子メールは一度に 1 つのパートナープログラムでのみ使用できるので、各プログラムの登録連絡先には、それぞれ別の電子メールを使用する必要があります。

会社が Exchange パートナープログラムに参加したら、[Exchange パートナーサイト](https://partners.adobe.com/exchangeprogram/experiencecloud)にサインインし、次の手順に従うことで、アドビソリューションへのアクセスを要求できます。既に Experience Cloud アカウントを持ち、ソリューションにアクセスできる一方で、Adobe Experience Platform のデータ収集 UI にアクセスできない場合は、[グループとユーザーの設定手順](../../ui/administration/user-permissions.md)を参照してください。

## 個人の開発者

独立した開発者、またはリストに記載されたパートナープログラムの 1 つに参加できない場合で、Adobe Experience Platform のタグ拡張機能を構築したい場合は、launch-ext-dev@adobe.com に電子メールを送信してアクセスをリクエストしてください。Adobe Experience Platform のタグでのエクスペリエンスに関する背景と、予定している拡張機能プロジェクトのロードマップを提供してください。
