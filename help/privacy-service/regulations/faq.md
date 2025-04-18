---
keywords: Experience Platform；ホーム；人気のトピック；GDPR;gdpr;CCPA;ccpa;PDPA;pdpa;LGPD;lgpd;faq;FAQ；規制；規制；規制；規制；プライバシー；プライバシー；
solution: Experience Platform
title: プライバシー規制 FAQ
description: このドキュメントでは、サポートされる法的プライバシー規制とAdobe Experience Cloudでの実装に関するよくある質問に対する回答を示します。
exl-id: ec553e53-664b-4e18-abb1-4e4063fdd2c9
source-git-commit: d643b2aeadd4080fa89d6a7b0f84a9f6882d7b89
workflow-type: tm+mt
source-wordcount: '1606'
ht-degree: 32%

---

# プライバシー規制 FAQ

このドキュメントでは、サポートされる法的プライバシー規制とAdobe Experience Cloudでの実装に関するよくある質問に対する回答を示します。

>[!NOTE]
>
>このドキュメントで使用される様々な用語の定義については、[ プライバシー規制の用語 ](terminology.md) ガイドを参照してください。

## 一般的な質問

以下の質問は、Experience Cloudがサポートするすべてのプライバシー規制に関連しています。

### サポートされるプライバシー規制は誰に影響を与えますか？

[Experience Cloudがサポートするプライバシー規制 ](./overview.md) は、組織の地理的な場所に関係なく、規制の各管轄区域内の市民の個人データを保存および処理するすべての組織に適用されます。

### 個人データとは何ですか？

個人データとは、自然人またはデータ主体に関する（個人を直接的または間接的に特定するために使用できる）情報です。個人データは、名前、写真、電子メールアドレス、銀行口座の詳細、ソーシャルネットワーキング web サイト上の投稿、医療情報、コンピューターの IP アドレスなどである場合があります。

次の識別子は、Experience Cloudアプリケーションで一般的に使用され、プライバシー規制の要件の対象となる可能性があります。

* 名前
* 住所
* 一意の個人識別子
* オンライン識別子
* IP アドレス
* 電子メールアドレス
* アカウント名

個人情報には、インターネットやその他の電子ネットワークアクティビティ情報も含まれます。これには、以下が含まれます。

* 閲覧履歴
* 検索履歴
* Web サイト、アプリケーションまたは広告との消費者のインタラクションに関する情報

プライバシー規制は幅広い個人情報を対象としていますが、Adobeの標準的な契約条件では、機密性の高い個人情報（SSN、運転免許証、金融口座情報、生体認証データなど）は通常、Experience Cloudアプリケーションでのインポートおよび使用は禁止されています。

### データ管理者とデータ処理者の違いは何ですか？

**データ管理者**&#x200B;は、個人データを処理する目的、条件、手段を決定する主体であり、**データ処理者**&#x200B;は、データ管理者に代わって個人データを処理する主体です。

**データ管理者** とは、個人データの収集、使用、開示に関する決定を下す権限と責任を持つ個人または組織です。 **データ処理者** とは、個人データの収集、使用、開示およびデータ管理者の指示に関連して活動する個人または組織です。

### データ主体の明示的な同意とあいまいでない同意の違いは何ですか？

**明示的な同意** とは、データ主体の希望を、口頭または書面による形で、明確な情報に基づいて示す、同意の標準を指します。 簡単に言えば、データ主体は、同意が明示的と見なされるために、「同意します」または「同意します」と文字通り明示的に言う必要があります。 さらに、同意を取り消すことは、それを与えることと同じくらい簡単でなければなりません。

**明白な（暗黙的な）同意** とは、データ主体によって明示的に与えられてはいないが、本質的に明白な同意を指します。 例えば、会社の web サイトのサインアッププロセス中に、メールアドレスを指定することで、データ主体が特別オファーでのメール受信に同意する通知が送信されます。 データ主体が通知を読んだ場合、メールを入力する肯定的なアクションは、明確な同意と見なされるに十分です。

GDPR などの多くの規制では、機密性の高い個人データを処理するには明示的な同意が必要で、「オプトイン」以外では十分ではありません。 ただし、機密データでない場合は、明確な（暗黙的な）同意を得ることができます。

### 特定の年齢未満のデータ主体は同意できますか？

多くのプライバシー規制では、データ主体が特定の年齢に達しない場合、個人データの収集に対する同意を法的に提供できないと規定されています。 一部の規制では、データ主体に対する親の責任を負う人の同意を認めていますが、すべての同意を認めているわけではありません。 次の表に、各規制に対してデータ主体が自分で同意を得るための最低年齢を示し、詳細に関する注意事項を示します。

| 規則 | 同意年齢 | メモ |
| --- | --- | --- |
| CCPA （カリフォルニア州） | 16 | <ul><li>保護者の同意は、13 歳以上のデータ主体に対してのみ提供できます。</li><li>13 歳未満の対象者からの個人データの収集は固く禁止されています。</li></ul> |
| GDPR （欧州連合） | 16 | <ul><li>EU の一部の加盟国は、この目的のために年齢を下げる法律を提供することがありますが、13 以上です。</li><li>年齢制限以下のすべてのデータ主体に対して、保護者の同意を提供する必要があります。</li></ul> |
| LGPD （ブラジル） | 13 | <ul><li>年齢制限以下のすべてのデータ主体に対して、保護者の同意を提供する必要があります。</li><li>個人データの処理が彼らの最善の利益のために受け取られている限り、13～18 歳の自然人によって同意が与えられる可能性があります。</li></ul> |
| PDPA （タイ） | 10 | <ul><li>年齢制限以下のすべてのデータ主体に対して、保護者の同意を提供する必要があります。</li></ul> |

<!-- | New Zealand [!DNL Privacy Act] | 16 | <ul><li>Parental consent must be provided for all data subjects below the age limit in cases where consent is required.</li></ul> | -->

### 顧客の個人情報のアクセスや削除をおこなう要求に対して何日で応答する必要がありますか。

ビジネスが個人情報を収集し、特定の消費者の ID を認証または検証できると仮定すると、プライバシー規制は、消費者のリクエストが満たされる特定の時間枠を許可します。 次の表に、各規制に適用される期間を示します。一部の例外に関する注意事項も含まれています。

>[!NOTE]
>
>「日数」で応答する期間は、消費者のリクエストを完了するために各規制法で義務付けられているタイムラインを反映しています。

| 規則 | 応答期間 | メモ |
| --- | --- | --- |
| CCPA （カリフォルニア州） | 45 日 | |
| GDPR （欧州連合） | 30 日 | |
| LGPD （ブラジル） | 15 日 | |
| PDPA （タイ） | 30 日 | 企業がコンプライアンス期間内にデータ主体のリクエストに応答できない場合、データ主体に書面で応答するリクエストを実行できなかった日付から 30 日以内に、企業は追加の日数を請求されます。 |

<!-- | New Zealand [!DNL Privacy Act] | 20 working days | | -->

### データ保護担当者を任命する必要がありますか？

組織のデータ操作が GDPR、LGPD または PDPA の法的管轄区域に該当する場合、次の場合にデータ保護責任者（DPO）を任命する必要があります。

* 組織は公開機関です
* 組織は、大規模な体系的な監視に従事しています
* 組織は、機密性の高い個人データの大規模な処理に従事します。

>[!IMPORTANT]
>
>CCPA は他の規制と異なり、これを要件として規定しています。 ただし、プライバシーコンプライアンスを維持するには、企業は適格な個人のモニタリングデータ収集アクティビティと消費者データの保存を持ち、顧客の問い合わせに対応する必要があります。

### プライバシー規制の対象となるデータを維持する場合、消費者のプライバシーリクエストをどのようにサポートできますか？

適切な法的管轄区域に該当するコンシューマーを認証するために必要な手順を実行したら、Adobe Experience Platform Privacy Serviceを使用して、互換性のあるExperience Cloudアプリケーションにコンシューマープライバシーリクエストを送信できます。 詳しくは、[[!DNL Privacy Service]  概要 ](../home.md) を参照してください。 お使いの Experience Cloud アプリケーションがプライバシーリクエストをどのように受け入れるかについて詳しくは、[Privacy Service および Experience Cloud アプリケーション](../experience-cloud-apps.md)のガイドを参照してください。

>[!NOTE]
>
>カリフォルニア州規制当局からの今後のガイダンスでは、消費者のプライバシーリクエストの対象となるデータのタイプについて、引き続きガイダンスを提供します。

## CCPA に関する質問

次の質問は、特に CCPA に関連するものです。

### CCPA の様々な役割や責任は Experience Cloud にどのように適用されますか。

CCPA の定義に従って、次の役割がアドビとその顧客に適用されます。

* アドビの顧客（カリフォルニア在住者の個人情報の収集と使用を要求する当事者）は、**ビジネス**&#x200B;と見なされます。
* アドビは、サービスを提供する役割を果たしており、**サービスプロバイダー**&#x200B;と見なされます。

サービスプロバイダーとして、アドビは、ビジネスの代わりに個人情報を収集し、処理し、契約上、契約に基づき、契約で設定された特定の目的のみにその情報を使用する義務があります。

この関係とAdobeの契約条項を考慮すると、Adobeへの開示は、企業が通知を提供し、同意を求める必要がある「セール」とは見なされない可能性が高くなります。

ただし、アドビのサービスを使用して、特定のデータ共有を有効にし、サードパーティに転送することができます。これらの第三者移転は「販売」とみなされ、法的に開示と同意が必要となる可能性があります。 顧客は、適切な要件を評価する特定の使用事例を評価するために、弁護士と協力する必要があります。

### アドビは、CCPA の要件に対処するのに役立つその他のツールを提供しますか。

Adobe Experience Cloud アプリケーションは、企業のプライバシーニーズに役立つデータ管理およびガバナンス機能を提供します。これらのツールの中には、データ使用のラベル付け、役割ベースのアクセス制御、IP 難読化、ハッシュ機能があります。

アドビは、ISO 27001 認定制度や TrustArc GDPR 検証など、プライバシーとセキュリティの実践に関するさまざまな認定を受けています。

## GDPR に関する質問

次の質問は、特に GDPR に関連しています。

### 規制と指令の違いは何ですか？

**規制**&#x200B;は、拘束力のある法的措置であり、EU 全域にわたって適用する必要があります。**指令**&#x200B;は、EU 加盟国すべてが達成する必要がある目標を掲げた法的措置ですが、その達成方法は個々の加盟国が決定します。

GDPR は規制であり、ディレクティブである以前の法律（データ保護指令）とは異なることに注意することが重要です。

### GDPR は、データ漏洩に関するポリシーにどのような影響を与えますか？

データ漏洩に関する規制案は、侵害された企業の通知ポリシーに主に関連しています。個人をリスクにさらす可能性のあるデータ漏洩は、データ保護機関に 72 時間以内に通告され、影響を受ける個人にも遅滞なく通知される必要があります。

## PDPA の質問

次の質問は、PDPA に特化したものです。

### 機密性の高い個人データとは何ですか？

PDPA は、人種的または民族的起源、政治的意見、宗教的または哲学的信念、犯罪記録、労働組合のメンバーシップ、遺伝的データ、生体認証データ、健康記録、性的指向または好みに関する個人データを含む、機密性の高い個人データの収集と保存に対する厳しい要件を提供します。
