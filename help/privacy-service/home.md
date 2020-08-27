---
keywords: Experience Platform;home;popular topics;GDPR;gdpr;ccpa:CCPA
solution: Experience Platform
title: Adobe Experience Platform Privacy Service
topic: overview
translation-type: tm+mt
source-git-commit: 1b398e479137a12bcfc3208d37472aae3d6721e1
workflow-type: tm+mt
source-wordcount: '1502'
ht-degree: 14%

---


# Adobe Experience Platform [!DNL Privacy Service] overview

より良い顧客体験を提供するためには、顧客の個人データを収集し、保存する必要があります。このデータを使用する場合、顧客のプライバシーを理解し、尊重することが重要です。新しい法規制や組織の規制により、ユーザーは要求に応じて、データストアから個人データにアクセスしたり、個人データを削除したりする権利が与えられます。

Adobe Experience Platform [!DNL Privacy Service] was developed in response to a fundamental shift in how businesses are required to manage the personal data of their customers. The central purpose of [!DNL Privacy Service] is to automate compliance with data privacy regulations which, when violated, can result in major fines and disrupt data operations for your business.

[!DNL Privacy Service] には、顧客データリクエストを管理するのに役立つRESTful APIおよびユーザーインターフェイスが用意されています。 With [!DNL Privacy Service], you can submit requests to access and delete personal customer data from Adobe Experience Cloud applications, facilitating automated compliance with legal and organizational privacy regulations.

## Getting started with [!DNL Privacy Service] {#getting-started}

利用するために [!DNL Privacy Service]は、組織のプライバシー要件、顧客から収集するIDデータの種類、CRMシステムとサービスを連携させる最良の方法に関して、いくつかの重要な決定を下す必要があります。

これらの決定事項は、次の質問にまとめることができます。

1. **顧客から収集する情報**
   * を最大限に活用するに [!DNL Privacy Service]は、顧客から収集するデータのタイプと、どのデータがプライバシー規制の対象となるかについて、詳細に理解する必要があります。 詳しくは、プライバシー要件の [決定に関する節を参照してください](#requirements) 。
1. **データに正しいラベルを付けているか。**
   * サービスがプライバシージョブ中にアクセスまたは削除するフィールドを決定するには、データに適切なラベルを付ける必要があります。 詳しくは、「データの [ラベル付け](#label) 」の項を参照してください。
1. **送信先のIDがわかり[!DNL Privacy Service]ますか。**
   * プライバシーリクエストを送信する際は、特定のAdobeアプリケーションに固有の個々の顧客IDを提供する必要があります。 詳しくは、IDデータの [提供とプライバシーリクエストの作成に関する節を参照してください](#identity)[](#requests) 。
1. **プライバシー業務の追跡方法**
   * プライバシーリクエストを行った後、そのステータスと結果を追跡するためのオプションがいくつか用意されています。 詳しくは、「プライバシージョブ [の監視](#monitor) 」の節を参照してください。

以下の節では、これらの重要な前提条件の手順に関する一般的なガイダンスを提供し、さらに詳細について詳しいドキュメントへのリンクも [!DNL Privacy Service] 提供します。

### 組織のプライバシー要件の決定 {#requirements}

貴社のビジネスの性質とその運営する管轄に応じて、データ操作は法的なプライバシー規制の対象となる場合があります。 このような規制により、お客様は、お客様から収集されたデータへのアクセスを要求する権利と、保存されたデータの削除を要求する権利を得ることができます。 個人データを要求するお客様は、ドキュメント全体で「プライバシー要請」と呼ばれます。

次の表に、リクエストを管理する法的プライバシー規制の概要を示します。詳細については、ドキュメントへのリンクを含めて [!DNL Privacy Service] ください。

| 規制 | 説明 |
| --- | --- |
| CCPA（カリフォルニア） | The [!DNL California Consumer Privacy Act] (CCPA) enhances privacy rights and consumer protection for residents of California, United States. CCPA は、自分の個人データにアクセスして削除する権利、自分の個人データが販売または開示されているかどうか（および誰に）を知る権利、第三者へのデータの販売をオプトアウトする権利など、カリフォルニア在住者に新しいデータのプライバシー権を提供します。<br/><br/>その他のドキュメントへのリンク： <ul><li>[法的概要](https://oag.ca.gov/privacy/ccpa)</li><li>[CCPA FAQ](ccpa/faq.md)</li></ul> |
| GDPR(ヨーロッパ和集合) | The [!DNL General Data Protection Regulation] (GDPR) introduced several new data privacy rights for members of the European Union, including the **Right to Access** and the **Right to be Forgotten**. つまり、企業が収集した個人データに対して EU 市民は、いつでもデータのアクセスや削除を要求できます。<br/><br/>その他のドキュメントへのリンク： <ul><li>[法的概要](https://gdpr-info.eu/)</li><li>[GDPR に関するよくある質問](gdpr/faq.md)</li><li>[GDPR 関連の用語](gdpr/terminology.md)</li></ul> |
| PDPA_THA（タイ） | タイの個人データ保護法(PDPA)は、タイのデータ所有者を、個人データの不正な収集、使用、開示から守るために導入された。 欧州和集合のGDPRに刺激され、この規制は、タイ国民に対し、保存された個人データへのアクセスを要求し、あるいは削除する権利を与える。<br/><br/>その他のドキュメントへのリンク： <ul><li>[法的概要](https://www.dataprotectionreport.com/2020/02/thailand-personal-data-protection-law/)</li><li>[PDFA_THA FAQ](pdpa-tha/faq.md)</li><li>[PDPA_THAの用語](pdpa-tha/terminology.md)</li></ul> |

データ操作が上記の規制のいずれかに該当する場合は、お客様に提供する特定のプライバシー権限や、プライバシー要求を守るためのコンプライアンス・ウィンドウなど、重要な情報について、ドキュメントを確認します。 この情報は、CRMシステムへの統合方法を決定する際、およびプライバシーリクエストを行うため [!DNL Privacy Service] に顧客がWebサイトとどのようにやり取りするかを考慮する必要があります。

法的規制に加えて、お客様の組織に適用される組織や業界標準も、これらの決定を行う際に考慮する必要があります。

### Label data for privacy requests {#label}

使用している [!DNL Experience Cloud] アプリケーションに応じて、プライバシーの要請に応じてアクセスまたは削除する必要がある特定のデータフィールドにラベルを付ける必要があります。 データのラベル付けのプロセスは、アプリケーションによって異なります。 サポートされている各Adobeアプリケーションのデータラベル付けの方法については、 [Experience Cloudアプリケーションのドキュメントを参照してください](./experience-cloud-apps.md)。

### 送信先のIDデータの種類の決定 [!DNL Privacy Service] {#identity}

顧客からのプライバシー要求 [!DNL Privacy Service] を処理するには、その顧客の一意のID値をリクエスト自体に少なくとも1つ指定する必要があります。 一意のID値とは、個々の人物と、その人物が保存する個人データを識別するために使用できる [!DNL Experience Cloud] データの一部です。 [!DNL Privacy Service] この識別情報を使用して、要求（アクセス、削除またはオプトアウト）の性質に従って顧客の個人データを検索し、処理します。

CRMシステムが利用する [!DNL Experience Cloud] アプリケーションによって、各顧客に指定する必要のあるIDのタイプと数は異なります。 一部のアプリケーションは、独自の内部顧客ID値(Adobe TargetIDなど)を使用し、他のソリューションはAdobe [!DNL Experience Cloud Identity Service] (ECID)のグローバルIDに依存しており、すべての [!DNL Experience Cloud] アプリケーションで顧客のアクティビティを追跡します。 また、電子メールアドレスや電話番号などの一般的な個人情報も有効なIDデータとして機能します。

プライバシー要求の [IDデータに関するドキュメント](./identity-data.md) （Idデータに関する情報）には、で受け入れられるID情報のタイプに関する詳細が記載されてい [!DNL Privacy Service]ます。 また、ドキュメントは、Adobeテクノロジーを利用して、Webサイトとのやり取りに応じて顧客から適切なID情報を効果的に取得し、そのデータをAPIリクエストに送信する方法に関するガイダンスも提供し [!DNL Privacy Service] ます。

### プライバシーリクエストの開始 {#requests}

ビジネスのプライバシーニーズを決定し、送信するIDの値を決定したら、開始がプライバシーリクエストを作成でき [!DNL Privacy Service]ます。 [!DNL Privacy Service] APIまたはUIを通じてプライバシーリクエストを送信できます。

>[!IMPORTANT]
>
>以下の節では、APIまたはUIで一般的なプライバシーリクエストを行う方法を説明するドキュメントへのリンクを提供します。 ただし、使用している [!DNL Experience Cloud] アプリケーションによっては、リクエストペイロードで送信する必要があるフィールドが、これらのガイドに示す例と異なる場合があります。
>
>APIまたはUIガイドに従う場合は、 [](./experience-cloud-apps.md)[!DNL Experience Cloud] Privacy ServiceおよびExperience Cloudアプリケーションに関するドキュメントを参照して、特定のアプリケーションに対するプライバシー要求の形式を設定する方法に関する詳細なドキュメントを参照してください。

#### API の使用

[ [[!DNLPrivacy ServiceAPI]](https://www.adobe.io/apis/experienceplatform/home/api-reference.html#!acpdr/swagger-specs/privacy-service.yaml) は、RESTful API呼び出しを使用してプライバシージョブを作成および管理するためのエンドポイントをいくつか備えており、アプリ [!DNL Experience Cloud] ケーションのプライバシー規制への準拠をプログラム的にアプローチできます。 API の使用方法に関する詳細な手順については、「[Privacy Service API 開発者ガイド](api/getting-started.md)」を参照してください。

#### UI の使用

>[!NOTE]
>
>現在、 [!DNL Privacy Service] UIはアクセス要求と削除要求のみをサポートしています。 代わりに、すべてのオプトアウトリクエストはAPIを使用して行う必要があります。

The [!DNL Privacy Service] UI allows you to create and monitor privacy jobs using a graphical interface. UI には、すべてのアクティブなリクエストのステータスを視覚的に表す 「**[!UICONTROL ステータスレポート]**」Widget が含まれており、組み込みの「**[!UICONTROL リクエストビルダー]**」を使用するか、JSON ファイルをアップロードして新しいリクエストを作成できます。UI の使用について詳しくは、「[Privacy Service のユーザガイド](ui/overview.md)」を参照してください。

### プライバシージョブの監視 {#monitor}

プライバシージョブを作成した後は、そのステータスと結果を監視するためのいくつかのオプションが用意されています。

| 監視方法 | 説明 |
| --- | --- |
| [!DNL Privacy Service] UI | UIには監視ダッシュボードが用意されており、すべてのアクティブな要求のステータスを視覚的に表示できます。 [!DNL Privacy Service] See the [Privacy Service user guide](ui/overview.md) for more information. |
| [!DNL Privacy Service] API | APIが提供する参照エンドポイントを使用して、プライバシージョブのステータスをプログラムで監視でき [!DNL Privacy Service] ます。 APIの使用方法に関する詳細な手順については、 [Privacy Service開発者ガイド](./api/getting-started.md) （英語）を参照してください。 |
| [!DNL Privacy Events] | [!DNL Privacy Events] 効率的なジョブリクエストの自動化を促進するために、設定済みのWebフックに送信されるAdobeI/Oイベントを活用します。 They reduce or eliminate the need to poll the [!DNL Privacy Service] API in order to check if a job is complete or if a certain milestone within a workflow has been reached. 詳しくは、「プライバシーイベントの [購読](./privacy-events.md) 」のチュートリアルを参照してください。 |

## 次の手順

このドキュメントでは、の概要 [!DNL Privacy Service] と、サービスの機能を使用して開始するために必要な主な手順を示しました。 での作業の様々な面についての詳細は、概要全体にリンクされたドキュメントを参照してくだ [!DNL Privacy Service]さい。
