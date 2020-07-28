---
keywords: Experience Platform;home;popular topics
solution: Experience Platform
title: CCPA に関する FAQ
topic: troubleshooting
translation-type: tm+mt
source-git-commit: 5b32c1955fac4f137ba44e8189376c81cdbbfc40
workflow-type: tm+mt
source-wordcount: '787'
ht-degree: 79%

---


# CCPA に関する FAQ

This document provides answers to frequently asked questions about the [!DNL California Consumer Protection Act] (CCPA) and its implementation in Adobe Experience Cloud.

## CCPA とは

The [!DNL California Consumer Privacy Act] (CCPA) is California’s new privacy law that provides its residents with new rights regarding their personal information, and imposes data protection responsibilities on certain entities who conduct business in California.

>[!NOTE]
>
> CCPA は技術的には 2020 年 1 月に有効になっていますが、依然として立法者によって微調整されています。さらに、重要な実装やその他のガイダンスの詳細は、カリフォルニアの規制当局によってまだ記述されていないルールで将来公開されます。

CCPA は、個人情報にアクセスして削除する個人の権利など、EU の一般データ保護規則（GDPR）に基づいて提供されるいくつかの概念を共有していますが、CCPA が GDPR と異なる点はいくつかあります。例えば、CCPA は、個人情報を第三者に「販売」すると見なされる特定のデータ共有アクティビティの事前の同意を必要とする代わりに、オプトアウト権を消費者に提供します。

## CCPA の個人情報の定義は何ですか。

個人情報は、「消費者又は世帯を、識別し、関連し、叙述し、関連付けることができ、又は直接的に若しくは間接的に合理的にリンクさせることのできる情報」です。

## Adobe Experience Cloud で使用される、どの種類の個人情報または識別情報に、これらの新しい要件が適用されますか。

The following identifiers are commonly used in [!DNL Experience Cloud] applications and could be subject to CCPA requirements:

- 名前
- 住所
- 一意の個人識別子
- オンライン識別子
- IP アドレス
- 電子メールアドレス
- アカウント名

個人情報には、インターネットやその他の電子ネットワークアクティビティ情報も含まれます。これには、以下が含まれます。

- 閲覧履歴
- 検索履歴
- Web サイト、アプリケーションまたは広告との消費者のインタラクションに関する情報

Even though CCPA covers a wide set of personal information, Adobe’s standard contract terms dictate that sensitive personal information (such as SSN, driver’s license information, financial account information, and biometric data) is generally prohibited from import and use in [!DNL Experience Cloud] applications.

## How do CCPA&#39;s different roles and responsibilities apply to [!DNL Experience Cloud]?

CCPA の定義に従って、次の役割がアドビとその顧客に適用されます。

- アドビの顧客（カリフォルニア在住者の個人情報の収集と使用を要求する当事者）は、**ビジネス**&#x200B;と見なされます。
- アドビは、サービスを提供する役割を果たしており、**サービスプロバイダー**&#x200B;と見なされます。

この関係とアドビの契約言語を考えると、アドビへの開示は、ビジネスが通知やオファーにオプトアウトを提供する必要がある「販売」とは見なされません。

ただし、アドビのサービスを使用して、特定のデータ共有を有効にし、サードパーティに転送することができます。これらのサードパーティの転送は「販売」と見なされ、法的に開示やオプトアウトを義務付けられます。  顧客は、適切な要件を評価する特定の使用事例を評価するために、弁護士と協力する必要があります。

## 顧客の個人情報のアクセスや削除をおこなう要求に対して何日で応答する必要がありますか。

ビジネスが個人情報を収集し、特定のカリフォルニア州の消費者の ID を認証または検証できると仮定すると、CCPA では、（一部の例外を除き）45 日以内に消費者要求を満たすことを許可します。

## CCPA の下でのアドビの役割とは何ですか。

サービスプロバイダーとして、アドビは、ビジネスの代わりに個人情報を収集し、処理し、契約上、契約に基づき、契約で設定された特定の目的のみにその情報を使用する義務があります。

この関係とアドビの契約言語を考えると、アドビへの開示は、サービスプロバイダーに関する法律の規定の範囲内に含まれ、無事ネスが通知やオファーにオプトアウトを提供する必要がある「販売」とは見なされません。

アドビのサービスを使用して、特定のデータ共有を有効にし、サードパーティに転送することができます。これらのサードパーティの転送は「販売」と見なされ、法的に開示やオプトアウトを義務付けられます。顧客は、適切な要件を評価する特定の使用事例を評価するために、弁護士と協力する必要があります。

## 要件の対象となる特定の種類のデータを維持している場合、CCPA の下で消費者のプライバシー要件をどのようにサポートできますか。

Once you have taken the necessary steps to authenticate CA consumers, Adobe Experience Platform [!DNL Privacy Service] allows you to submit consumer privacy requests to compatible [!DNL Experience Cloud] applications. 詳しくは、「[Privacy Service の概要](../home.md)」を参照してください。For information on how your particular [!DNL Experience Cloud] applications can honor privacy requests, please refer to the guide on [Privacy Service and Experience Cloud applications](../experience-cloud-apps.md).

>[!NOTE]
>
> カリフォルニア州の規制当局からの詳細なガイダンスは、消費者のプライバシーリクエストに適したデータの種類に関して、今後も発表され続けます。

## アドビは、CCPA の要件に対処するのに役立つその他のツールを提供しますか。

Adobe Experience Cloud アプリケーションは、企業のプライバシーニーズに役立つデータ管理およびガバナンス機能を提供します。これらのツールの中には、データ使用のラベル付け、役割ベースのアクセス制御、IP 難読化、ハッシュ機能があります。

アドビは、ISO 27001 認定制度や TrustArc GDPR 検証など、プライバシーとセキュリティの実践に関するさまざまな認定を受けています。