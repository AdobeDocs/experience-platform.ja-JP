---
title: 顧客管理キー用のAWS KMS の設定
description: Adobe Experience Platformの顧客管理キーと共に使用するAmazon Web Services Key Management Service （KMS）を設定する方法について説明します。
exl-id: 0cf0deab-dc30-412f-b511-dee5504c3953
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '1571'
ht-degree: 0%

---

# 顧客管理キー用のAWS KMS の設定

>[!AVAILABILITY]
>
>このドキュメントは、Amazon Web Services（AWS）上で動作するExperience Platformの実装に適用されます。 AWS上で動作するExperience Platformは、現在、限られた数のお客様が利用できます。 サポートされるExperience Platform インフラストラクチャについて詳しくは、[Experience Platform multi-cloud overview](https://experienceleague.adobe.com/en/docs/experience-platform/landing/multi-cloud) を参照してください。
>
>AWSの [ 顧客管理キー ](../overview.md) （CMK）は、Privacy and Security Shield でサポートされていますが、Healthcare Shield では使用できません。 Azure 上の CMK は、Privacy and Security Shield および Healthcare Shield の両方でサポートされています。

このガイドを使用すると、Adobe Experience Platformの暗号化キーを作成、管理、および制御して、Amazon Web Services（AWS） Key Management Service （KMS）でデータを保護できます。 この統合により、コンプライアンスの合理化、自動化によるオペレーションの合理化が実現し、独自の主要管理インフラストラクチャを維持する必要がなくなります。

Customer Journey Analytics固有の手順については、[Customer Journey Analytics CMK ドキュメントを参照してください ](https://experienceleague.adobe.com/en/docs/analytics-platform/using/cja-privacy/cmk)

>[!IMPORTANT]
>
>Adobe Experience Platformは、システム管理キーを使用して、デフォルトで保存中のデータを暗号化します。 顧客管理キー（CMK）を有効にすると、データのセキュリティを完全に制御できます。 ただし、この変更は元に戻せません。CMK が有効になると、システム管理キーに戻すことはできません。 キーを安全に管理して、データへの中断のないアクセスを確保し、アクセス不能の可能性を防ぐ責任があります。

AWS KMS を使用すると、Adobe Experience Platformの統合暗号化キー管理によりデータのセキュリティを強化できます。 このガイドに従って暗号化キーを作成および管理し、データを確実に保護します。

## 前提条件 {#prerequisites}

このドキュメントを進める前に、次の主な概念と機能を十分に理解しておく必要があります。

- **AWS Key Management Service （KMS）**：暗号化キーの作成、管理、ローテーションの方法など、AWS KMS の基本事項を説明します。 詳しくは、[ 公式の KMS ドキュメント ](https://docs.aws.amazon.com/kms/) を参照してください。
- **AWSの IAM （Identity and Access Management）ポリシー**: IAM は、AWSのサービスやリソースへのアクセスを安全に管理できるサービスです。 IAM を使用して、次の操作を行います。
   - 特定のリソースにアクセスできるユーザー、グループ、役割を定義します。
   - ユーザーが実行を許可または拒否するアクションを指定します。
   - IAM ポリシーを使用して権限を割り当てることで、きめ細かなアクセス制御を実装します。
詳しくは、[AWS KMS の IAM ポリシーの公式ドキュメント ](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html) を参照してください。
- **Experience Platformのデータセキュリティ**:Experience Platformでデータセキュリティを確保し、AWS KMS などの外部サービスと統合して暗号化する方法について説明します。 Experience Platformは、転送用の HTTPS TLS v1.2、保存時のクラウドプロバイダー暗号化、分離ストレージ、カスタマイズ可能な認証および暗号化オプションにより、データを保護します。 データを保護する方法について詳しくは、[governance, privacy, and security overview](../overview.md) または [Experience Platformでのデータ暗号化 ](../../encryption.md) に関するドキュメントを参照してください。
- **AWS Management Console**：すべてのAWS サービスに 1 つの web ベース アプリケーションからアクセスして管理できる中央ハブです。 検索バーを使用して、ツールの検索、通知の確認、アカウントと請求の管理、設定のカスタマイズをすばやく行います。 詳しくは、[AWS管理コンソールの公式ドキュメント ](https://docs.aws.amazon.com/awsconsolehelpdocs/latest/gsg/what-is.html) を参照してください。

## 基本を学ぶ {#get-started}

このガイドでは、既にAmazon Web Services アカウントにアクセスし、Management Console にアクセスできる必要があります。 開始するには、次の手順に従います。

### サポートされている地域を選択 {#select-supported-region}

AWS KMS は、特定の地域でご利用いただけます。 KMS がサポートされている地域で操作していることを確認してください。 サポートされているリージョンの一覧は、[AWS KMS エンドポイントとクォータの一覧 ](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/) で確認できます。

AWS KMS 暗号化キーがAdobe Experience Platform インスタンスと同じリージョンにあることを確認して、データレジデンシー要件へのコンプライアンスを維持し、パフォーマンスを最適化し、リージョン間の追加コストを回避します。 領域の位置がずれると、データにアクセスできなくなったり、統合に失敗したりする可能性があります。

### 権限の検証 {#verify-permissions}

KMS 内で暗号化キーを作成、管理、使用するために必要なAWS IAM （Identity and Access Management）権限があることを確認します。 権限を確認するには：

1. [IAM ポリシーシミュレーター ](https://policysim.aws.amazon.com/) にアクセスします。
2. ユーザーアカウントまたは役割を選択します。
3. `kms:CreateKey` や `kms:Encrypt` などの KMS アクションをシミュレートします。

シミュレーションでエラーが返された場合や、権限が不明な場合は、AWS管理者に問い合わせてください。

### AWS アカウントの設定の確認

AWS アカウントでAWS KMS サービスの使用が有効になっていることを確認します。 ほとんどのアカウントでは、デフォルトで KMS アクセスが有効になっていますが、[AWS Management Console](https://aws.amazon.com/console/) にアクセスして、アカウントの設定を確認できます。 詳しくは、[AWS Key Management Service デベロッパーガイド ](https://docs.aws.amazon.com/ja_jp/kms/latest/developerguide/overview.html) を参照してください。

### AWS KMS に移動して、主要な設定を開始します

暗号化キーの設定と管理を開始するには、AWS アカウントにログインし、AWS Key Management Service （KMS）に移動します。 AWS Management Console で、サービスメニューから **Key Management Service （KMS）** を選択します。

![ キー管理サービスがハイライト表示されたAWS Management Console の「検索」ドロップダウンメニュー。](../../../images/governance-privacy-security/key-management-service/navigate-to-kms.png)

## 新しいキーの作成 {#create-a-key}

>[!IMPORTANT]
>
>暗号化キーの安全な保存、アクセス、可用性を確保します。 キーを管理し、Experience Platformの操作が中断されるのを防ぐ責任があります。

[!DNL Key Management Service (KMS)] ワークスペースで、「**[!DNL Create a key]**」を選択します。

![ 「キーの作成」がハイライト表示されたキーマネジメントサービスワークスペース ](../../../images/governance-privacy-security/key-management-service/create-a-key.png)

## キー設定を指定 {#configure-key}

[!DNL Configure Key] ワークフローが表示されます。 デフォルトでは、キーの種類は **[!DNL Symmetric]** に設定され、キーの使用法は **[!DNL Encrypt and Decrypt]** に設定されます。 続行する前に、これらのオプションが選択されていることを確認します。

![ 「対称」および「暗号化と復号化」の基本オプションがハイライト表示されたキーワークフローの設定手順 1。](../../../images/governance-privacy-security/key-management-service/configure-key-basic-options.png)

**[!DNL Advanced options]** ドロップダウンメニューを展開します。 AWSで主要な資料を作成および管理できる **[!DNL KMS]** オプションを使用することをお勧めします。 デフォルトでは、「**[!DNL KMS]**」オプションが選択されています。

>[!NOTE]
>
>既存のキーがある場合は、外部キーマテリアルを読み込むか、AWS [!DNL CloudHSM] キーストアを使用できます。 これらのオプションについては、このドキュメントの範囲では説明しません。

次に、キーの領域範囲を指定する [!DNL Regionality] 設定を選択します。 「**[!DNL Single-Region key]**」、「**[!DNL Next]**」の順に選択して、手順 2 に進みます。

>[!IMPORTANT]
>
>AWSでは、KMS キーの地域に関する制限が適用されます。 つまり、キーはAdobe アカウントと同じリージョンにある必要があります。 Adobeは、お使いのアカウントの地域内にある KMS キーにのみアクセスできます。 選択した地域が、Adobe シングルテナントアカウントの地域と一致していることを確認してください。

![AWS地域、KMS、単一地域のキーの詳細オプションがハイライト表示されたキーワークフローの手順 1。](../../../images/governance-privacy-security/key-management-service/configure-key-advanced-options.png)

## キーにラベルを付けてタグ付け {#add-labels-and-tags-to-key}

2 番目に、ワ [!DNL Add labels] クフローのステージが表示されます。 ここでは、AWS KMS コンソールで暗号化キーを管理および見つけるのに役立つ [!DNL Alias] および [!DNL Tags] フィールドを設定します。

キー入力フィールドに、キーのわかりやすいラベルを **[!DNL Alias]** 力します。 エイリアスは、AWS KMS コンソールの検索バーを使用してキーをすばやく見つけるために、ユーザーにわかりやすい識別子として機能します。 混乱を防ぐには、「Adobe-Experience-Platform-Key」や「Customer-Encryption-Key」など、キーの目的を反映した意味のある名前を選択します。 また、鍵のエイリアスが目的を説明するのに不十分な場合は、鍵の説明を含めることもできます。

最後に、「[!DNL Tags]」セクションでキーと値のペアを追加して、キーにメタデータを割り当てます。 この手順はオプションですが、管理を容易にするために、AWS リソースの分類およびフィルタリングにタグを追加する必要があります。 例えば、組織で複数のAdobe関連リソースを使用している場合は、「Adobe」または「Experience-Platform」でタグ付けできます。 この追加の手順により、AWS Management Console で、関連するすべてのリソースを簡単に検索して管理できます。 「**[!DNL Add tag]**」を選択してプロセスを開始します。

<!-- I do not have an AWS account with which to document the Add tag process as yet. -->

設定に問題がなければ、「**[!DNL Next]**」を選択してワークフローを続行します。

![ エイリアス、説明、タグ、次へ ](../../../images/governance-privacy-security/key-management-service/add-labels.png) がハイライト表示されたキーワークフローの手順 2。

## 主な管理権限の定義 {#define-key-admins}

キー作成ワークフローの手順 3 が表示されます。 安全で制御されたアクセスを確保するために、鍵を管理できる IAM ユーザーとロールを選択できます。 この段階では、[!DNL Key administrators] と [!DNL Key deletion] の 2 つのオプションがあります。 **[!DNL Key administrators]** セクションで、このキーに対する管理者権限を付与するユーザーまたはロールの名前の横にある 1 つ以上のチェックボックスを選択します。

>[!NOTE]
>
>ワークフローのこの段階では管理者を作成できません。

「**[!DNL Key deletion]**」セクションで、キー管理者にこのキーを削除する権限を許可するチェックボックスを有効にします。 チェックボックスをオフにすると、管理者ユーザーはその操作を実行できません。

「**[!DNL Next]**」を選択して、ワークフローを続行します。

![ チェックボックスと「次へ」がハイライト表示された、ワークフローの主要な管理権限を定義ステージ。](../../../images/governance-privacy-security/key-management-service/define-key-admins.png)

## 主要なユーザーへのアクセス権の付与 {#assign-key-users}

ワークフローの手順 4 で、次の操作を [!DNL Define key usage permissions] 行できます。 **[!DNL Key users]** リストから、このキーを使用するアクセス許可を与えるすべての IAM ユーザーとロールのチェックボックスを選択します。

このビューから、を [!DNL Add another AWS account] くこともできますが、他のAWS アカウントを追加することは強くお勧めしません。 別のアカウントを追加すると、リスクが生じ、暗号化および復号化操作の権限管理が複雑になる可能性があります。 Adobeは、1 つのAWS アカウントに関連付けられたキーを保持することで、AWS KMS との安全な統合を確保し、リスクを最小限に抑え、信頼性の高い運用を確保します。

「**[!DNL Next]**」を選択して、ワークフローを続行します。

![ チェックボックスと「次へ」がハイライト表示された、ワークフローの主な使用権限の定義段階。](../../../images/governance-privacy-security/key-management-service/define-key-users.png)

## 主要な設定を確認 {#review}

主要設定のレビューステージが表示されます。 [!DNL Key configuration] の節と [!DNL Alias and description] の節で主な詳細を確認します。

>[!NOTE]
>
>キー地域がAWS アカウントと同じであることを確認します。

![ 主要な設定セクション、エイリアスセクション、説明セクションがハイライト表示されたワークフローのレビューステージ。](../../../images/governance-privacy-security/key-management-service/review-key-configuration-details.png)

「**[!DNL Confirm]**」を選択してプロセスを完了します。 使用可能なすべてのキーが一覧表示されている KMS 顧客管理キーワークスペースに戻ります。

## 次の手順

AWS KMS を設定したら、[!UICONTROL Platform Encryption Configuration] UI またはAdobe Experience Platform API を使用した統合の設定に進みます。 顧客管理キー機能を 1 回限りの方法で設定する場合は、[UI 設定ガイド ](./ui-set-up.md) を参照してください。
