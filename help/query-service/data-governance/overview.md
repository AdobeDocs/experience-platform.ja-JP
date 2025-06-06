---
title: クエリサービスでのデータガバナンス
description: ここでは、Experience Platform クエリサービスのデータガバナンスの主な要素について説明します。
exl-id: 37543d43-bd8c-4bf9-88e5-39de5efe3164
source-git-commit: f129c215ebc5dc169b9a7ef9b3faa3463ab413f3
workflow-type: tm+mt
source-wordcount: '3142'
ht-degree: 1%

---

# クエリサービスでのデータガバナンス

Adobe Experience Platformでは、複数のエンタープライズシステムのデータを統合し、必要に応じて、クエリサービスを通じてデータをクリーンアップ、シェイプ、操作、エンリッチメントできます。 これにより、マーケターは、顧客をより適切に識別、理解、惹きつけることができます。 適切なデータガバナンスの確保は、特定のデータが組織のポリシーや法規制に基づく使用制限の対象となる可能性があるため、個人情報の取り扱いにおける重要な側面です。 取り込んだデータとその関連操作が、定義されたデータ使用ポリシーに準拠していることを確認することが重要です。

クエリサービス内のデータガバナンスを使用すると、顧客データを管理し、データの使用に適用される規制、制限、ポリシーへのコンプライアンスを確保できます。 これは、ビジネスで定義された規制に従って使用ポリシーが適用されていることを確認する際に、重要な役割を果たします。

日常的にデータ処理を行う組織は、すべてのユーザーに対してプライバシーを意識した環境を作成するために、これらのガイドラインの概要を示し、実践し、適用することをお勧めします。

次のカテゴリは、クエリサービスを使用する際にデータコンプライアンス規制を遵守するうえで役に立ちます。

1. セキュリティ
1. 監査
1. データ使用状況
1. プライバシー
1. データハイジーン

このドキュメントでは、ガバナンスの様々な領域のそれぞれを調べ、クエリサービスを使用する際にデータコンプライアンスを促進する方法を示します。 Experience Platformを使用して顧客データを管理し、コンプライアンスを確保する方法について詳しくは、[ ガバナンス、プライバシー、セキュリティの概要 ](../../landing/governance-privacy-security/overview.md) を参照してください。

## セキュリティ {#security}

データのセキュリティとは、不正アクセスからデータを保護し、ライフサイクル全体を通じて安全なアクセスを確保するプロセスです。 ロールベースのアクセス制御や属性ベースのアクセス制御などの機能により、ロールと権限を適用することで、Experience Platformでの安全なアクセスが維持されます。 認証情報、SSL、データ暗号化も、Experience Platform全体でデータを確実に保護するために使用されます。

クエリサービスに関するセキュリティは、次のカテゴリに分類されます。

* [ アクセス制御 ](#access-control)：アクセスは、データセットや列レベルの権限などの役割と権限によって制御されます。
* [ 接続性 ](#connectivity) によるデータの保護：有効期限のある資格情報または有効期限のない資格情報を使用して限定的な接続を実現することで、Experience Platformおよび外部クライアントを通じてデータを保護します。
* [ 暗号化および顧客管理キー（CMK） ](#encryption-and-customer-managed-keys) によるデータの保護：データが保存されている場合は、暗号化によってアクセスが制御されます。

### アクセス制御 {#access-control}

Adobe Experience Platformのアクセス制御では、[Adobe Admin Console](https://adminconsole.adobe.com/) を使用して、役割ベースの権限を使用したクエリサービス機能へのアクセスを管理できます。 同様に、スキーマやデータフィールドのラベル管理を通じて、特定のデータ属性へのアクセスを制御できます。

この節では、クエリサービス機能を最大限に活用するためにユーザーが持つ必要がある、必要なアクセス制御権限の概要を説明します。 製品プロファイルにアクセス権を割り当てる手順について詳しくは、[ 権限の管理 ](../../access-control/ui/permissions.md) および [ ユーザーの管理 ](../../access-control/ui/users.md) に関するドキュメントを参照してください。

#### 関連する権限

関連するアクセス制御権限は、範囲のレベルに応じて、以下の表で定義されています。

**クエリ実行権限**

クエリサービス内でクエリを実行するには、次の権限を持つ役割がユーザーに割り当てられている必要があります。

| 権限 | 説明 |
|---|---|
| [!UICONTROL クエリの管理] | この権限を使用すると、ユーザーはデータ調査とバッチクエリを実行できます。これらのクエリでは、既存のデータセットの読み取りや、データセットへのデータの書き込みを行うことができます。 これには、`CREATE TABLE AS SELECT` （`CTAS`）と `INSERT INTO AS SELECT` （`ITAS`）の両方のクエリが含まれます。 |

**データセット権限**

この節では、クエリサービスを通じてデータにクエリを実行する際に、データセットにアクセスするために必要なリソースベースのアクセスについて説明します。

権限インターフェイスを使用すると、次の権限を持つデータセットおよびスキーマのリソースベースのアクセス制御を定義できます。

| 権限 | 説明 |
|---|---|
| [!UICONTROL データセットの管理] | この権限を持つユーザーは、スキーマに読み取り専用アクセスを提供し、クエリサービスで使用するデータセットの読み取り、作成、編集、削除へのアクセスを可能にします。 |
| [!UICONTROL データセットの表示] | この権限を使用すると、クエリサービスで使用するデータセットおよびスキーマに対して読み取り専用アクセスが可能になります。 |

#### 列/フィールドのアクセス制御

属性ベースのアクセス制御機能を使用すると、クエリサービスユーザーは重要なユーザーデータへのアクセスを制限できます。 役割に割り当てられた権限に基づいて、アクセスを許可または制限できます。 個々の列へのユーザーアクセスは、ユーザーに割り当てられた役割に適用される関連データ使用ラベルと権限セットによって制御されます。

データ使用ラベル付きのスキーマフィールドグループとクラスをタグ付けすると、同じフィールドグループとクラスを持つすべてのスキーマに対して、データ使用に関する制限が適用されます。 この機能の包括的な情報については、[ 属性ベースのアクセス制御 ](../../access-control/abac/overview.md) の概要を参照してください。

この機能を使用すると、選択したユーザーグループに機密列へのアクセス権を付与できます。 列のアクセス制御では、特定のタイプのユーザーに対する読み取り機能と書き込み機能の両方を制限できます。

列のアクセス制御は、標準スキーマとアドホックスキーマの両方のスキーマレベルで適用できます。 XDM スキーマにデータ使用ラベルを適用して、1 つ以上の列へのアクセスを制限します。 事前定義済みのスキーマまたは CTAS 操作の一部として生成されたアドホックスキーマを使用してクエリサービスを通じて作成されたデータセットであっても、データのラベル付けは一貫して適用されます。

ラベルと役割を使用して適切なレベルのアクセスが適用されると、ユーザーがアクセスできないデータにアクセスしようとすると、次のシステム動作が発生します。

1. ユーザーがスキーマ内の列のいずれかに対するアクセスを拒否された場合、そのユーザーは、制限された列に対する読み取りまたは書き込みの権限も拒否されます。 これは、次の一般的なシナリオに適用されます。

   * **ケース 1**：制限された列のみに影響するクエリをユーザーが実行しようとすると、列が存在しないというエラーがシステムからスローされます。
   * **ケース 2**：制限された列を含む複数の列を持つクエリをユーザーが実行しようとすると、制限されていないすべての列に対してのみ出力が返されます。

1. ユーザーが計算フィールドにアクセスしようとすると、ユーザーはコンポジションで使用されているすべてのフィールドにアクセスできる必要があり、システムも計算フィールドへのアクセスを拒否します。

#### ビューのアクセス制御

クエリサービスは、[`CREATE VIEW`](../sql/syntax.md#create-view) 文に標準の ANSI SQL を使用する機能を提供します。 機密性の高いデータワークフローの場合は、ビューの作成時に適切な制御を適用する必要があります。

`CREATE VIEW` キーワードは、クエリのビューを定義しますが、ビューは物理的に実体化されていません。 代わりに、ビューがクエリで参照されるたびにクエリが実行されます。 ユーザーがデータセットからビューを作成すると、親データセットの役割および属性ベースのアクセス制御規則が階層的に適用されます **適用されません**。 その結果、ビューを作成する際は、各列に対して権限を明示的に設定する必要があります。

#### 高速データセットにフィールドベースのアクセス制限を作成 {#create-field-based-access-restrictions-on-accelerated-datasets}

[ 属性ベースのアクセス制御機能を使用すると ](../../access-control/abac/overview.md) [ 高速化ストア ](../data-distiller/sql-insights/send-accelerated-queries.md) のファクトおよびディメンションデータセットに対して、組織またはデータの使用範囲を定義できます。 これにより、管理者は特定のセグメントへのアクセスを管理し、ユーザーまたはユーザーグループに付与するアクセスをより適切に管理できます。

高速データセットにフィールドベースのアクセス制限を作成するには、クエリサービス CTAS クエリを使用して、高速データセットを作成し、既存の XDM スキーマまたはアドホックスキーマに基づいてこれらのデータセットを構造化します。 管理者はその後 [&#128279;](./ad-hoc-schema-labels.md#edit-governance-labels) スキーマまたは [ アドホックスキーマのデータ使用ラベルを追加 ](../../xdm/tutorials/labels.md#edit-the-labels-for-the-schema-or-field) 編集  できます。 [!UICONTROL &#x200B; スキーマ &#x200B;] UI の [!UICONTROL &#x200B; ラベル &#x200B;] ワークスペースからスキーマへのラベルの適用、作成および編集を行えます。

データ使用ラベルは、データセット UI を通じて [ データセットに直接適用または編集 ](../../data-governance/labels/user-guide.md#add-labels) したり、アクセス制御 [!UICONTROL &#x200B; ラベル &#x200B;] ワークスペースから作成したりすることもできます。 詳しくは、[ 新しいラベルの作成 ](../../access-control/abac/ui/labels.md) 方法に関するガイドを参照してください。

個々の列へのユーザーアクセスは、添付されたデータ使用ラベルおよびユーザーに割り当てられた役割に適用される権限セットによって制御できます。

### 接続性 {#connectivity}

クエリサービスには、Experience Platform UI からアクセスするか、外部の互換性のあるクライアントと接続することでアクセスできます。 使用可能なすべての前面へのアクセスは、一連の資格情報によって制御されます。

#### 外部クライアントを介した接続

サードパーティのクライアントを使用してクエリサービスにアクセスするには、認証用の資格情報が必要です。 これらの資格情報は、互換性のある外部クライアントのいずれかでクエリサービスにアクセスするために必須です。 外部クライアントに接続するには、[ 有効期限のある資格情報 ](#expiring-credentials) または [ 有効期限のない資格情報 ](#non-expiring-credentials) を使用します。

#### 有効期限が切れる資格情報による接続時間が制限されています {#expiring-credentials}

[ 資格情報の有効期限が切れる ](../ui/credentials.md) ため、ユーザーは外部クライアントと一時的な接続を確立できます。 この資格情報のセットは 24 時間のみ有効です。 これらのタイプの資格情報の有効期限は、クエリサービスダッシュボードの「資格情報」タブと共に確認できます。

![ 有効期限が切れる資格情報がハイライト表示されたクエリサービスワークスペースの「資格情報」タブ ](../images/data-governance/overview/expiring-credentials.png)

#### 有効期限のない資格情報 {#non-expiring-credentials}

[ 有効期限のない資格情報 ](../ui/credentials.md#non-expiring-credentials) を使用すると、外部クライアントとの永続的な接続を形成でき、手動のパスワードなしでクエリサービスに簡単に接続できます。

有効期限のない認証情報を生成するオプションを有効にするには、説明されている [ 前提条件のワークフロー ](../ui/credentials.md#prerequisites) に従う必要があります。 このプロセスの一環として、組織の管理者は、製品プロファイルの権限を設定し、有効期限のない資格情報を使用するアクセス権を持つアカウントを管理者が制御できるようにする必要があります。

有効期限のない認証情報で許可されたテクニカルユーザーアカウントは、役割の責任とニーズに基づいて読み取りおよび書き込みアクセスの範囲を定義することで、適切なデータガバナンスを確保できるように割り当てることができます。 クエリサービスへのアクセスを管理するには、[ アクセス制御を介した役割ベースの権限の使用 ](#access-control) に関する前の節を参照してください。

前提条件のワークフローが完了すると、承認済みユーザーは [ 必要な接続資格情報を生成 ](../ui/credentials.md#generate-credentials) できるようになります。

#### SSL データ暗号化

セキュリティを強化するために、クエリサービスでは、クライアント/サーバー通信を暗号化するための SSL 接続のネイティブサポートを提供します。 Experience Platformでは、データセキュリティのニーズに合わせ、暗号化と鍵交換の処理オーバーヘッドのバランスを取るために、様々な SSL オプションをサポートしています。

`verify-full` SSL パラメーター値を使用した接続方法など、詳しくは、クエリサービスへのサードパーティクライアント接続に使用できる [SSL オプション ](../clients/ssl-modes.md) に関するガイドを参照してください。

### 暗号化および顧客管理キー（CMK） {#encryption-and-customer-managed-keys}

暗号化とは、データをエンコードされた判読不可能なテキストに変換するアルゴリズム処理を使用して、復号化キーなしで情報を保護し、アクセスできないようにすることです。

クエリサービスのデータコンプライアンスにより、データが常に暗号化されるようにします。 転送中のデータは常に HTTPS に準拠しており、保存中のデータはシステムレベルのキーを使用して Azure Data Lake ストアで暗号化されます。 詳しくは、[Adobe Experience Platformでのデータの暗号化 ](../../landing/governance-privacy-security/encryption.md) に関するドキュメントを参照してください。 保存データの Azure Data Lake Storage での暗号化の仕組みについて詳しくは、[Azure の公式ドキュメント ](https://docs.microsoft.com/ja-jp/azure/data-lake-store/data-lake-store-encryption) を参照してください。

転送中のデータは常に HTTPS に準拠しており、同様に、データがデータレイクに保存されている場合、暗号化は顧客管理キー（CMK）で行われます。CMK は、データレイク管理で既にサポートされています。 現在サポートされているバージョンは TLS1.2 です。Adobe Experience Platformに保存されたデータ用に独自の暗号化キーを設定する方法については [&#128279;](../../landing/governance-privacy-security/customer-managed-keys/overview.md)CMK （顧客管理キー）ドキュメント）を参照してください。


## 監査 {#audit}

クエリサービスは、ユーザーのアクティビティを記録し、そのアクティビティを様々なログタイプに分類します。 ログは、実行した **誰** 何 **アクション** および **タイミング** に関する情報を提供します。 ログに記録される各アクションには、アクションのタイプ、日時、アクションを実行したユーザーのメール ID、アクションのタイプに関連する追加の属性を示すメタデータが含まれます。

ログカテゴリは、Experience Platform ユーザーが必要に応じてリクエストできます。 ここでは、クエリサービス用に取得される情報のタイプと、その情報にアクセスできる場所の詳細を説明します。

### クエリログ {#query-logs}

クエリログ UI を使用すると、クエリエディターまたはクエリサービス API を介して実行されたすべてのクエリの実行の詳細を監視および確認できます。 これにより、クエリサービスアクティビティが透過的になり、クエリサービスをまたいで実行されたクエリの **すべて** メタデータを確認できます。 探索的クエリ、バッチクエリ、スケジュール済みクエリのいずれであっても、すべてのタイプのクエリが含まれます。

クエリログには、Experience Platform UI の [!UICONTROL &#x200B; クエリ &#x200B;] ワークスペースの「[!UICONTROL &#x200B; ログ &#x200B;]」タブからアクセスできます。

![ 詳細パネルがハイライト表示された「クエリログ」タブ ](../images/data-governance/overview/queries-log.png)

### 監査ログ {#audit-logs}

監査ログには、クエリログよりも詳細な情報が含まれており、ユーザー、日付、クエリのタイプなどの属性に基づいてログをフィルタリングできます。 監査ログには、クエリログ UI で使用可能な詳細に加えて、個々のユーザーの詳細がセッションデータやサードパーティクライアントへの接続と共に保存されます。

ユーザーのアクションの正確な記録を提供することで、監査証跡は問題のトラブルシューティングに役立ち、企業のデータ管理ポリシーおよび規制要件に効果的に準拠するのに役立ちます。 監査ログは、すべてのExperience Platform アクティビティの記録を提供します。 監査ログを使用すると、クエリの実行、テンプレート、スケジュールされたクエリに関するユーザーのアクションを監査して、クエリサービスでユーザーが実行したアクションの透明性と可視性を高めることができます。

次の表に、監査ログで取り込まれるクエリカテゴリと、それらのクエリカテゴリが記録するアクションタイプを示します。

| カテゴリ | アクションタイプ |
|---|---|
| クエリ | 実行 |
| クエリテンプレート | 作成、削除、更新 |
| スケジュールされたクエリ | 作成、削除、更新 |

次に、クエリログ内の詳細を保持する 3 つの拡張サーバーログのリストを示します。 拡張ログは、監査ログのクエリカテゴリ内にあります。

1. **メタクエリログ**：クエリが実行されると、関連する様々なバックエンドサブクエリ（解析など）が実行されます。 このようなタイプのクエリは、「メタデータ」クエリと呼ばれます。 関連する詳細は、監査ログで確認できます。
1. **セッションログ**：ユーザーがクエリを実行するかどうかに関係なく、クエリサービスにログインすると、システムはユーザーのセッションエントリログを作成します。
1. **サードパーティのクライアント接続ログ**：接続監査ログは、ユーザーがクエリサービスをサードパーティのクライアントに正常に接続した場合に生成されます。

監査ログが組織がデータのコンプライアンスに取り組む上でどのように役立つかについて詳しくは、[ 監査ログの概要 ](../../landing/governance-privacy-security/audit-logs/overview.md) を参照してください。

## データ使用状況 {#data-usage}

Experience Platformのデータガバナンスフレームワークは、すべてのAdobe ソリューション、サービス、プラットフォームにわたって、責任を持ってデータを使用する統一された方法を提供します。 メタデータのキャプチャ、伝達、使用に対する体系的なアプローチを、Adobe Experience Cloud全体で調整します。 これは、データ管理者が必要なマーケティングアクションに従ってデータにラベルを付け、意図したマーケティングアクションからそのデータに課せられる制限に役立ちます。 データガバナンスを使用して、データセットとフィールドにデータ使用ラベルを適用する方法について詳しくは、[ データ使用ラベル ](../../data-governance/labels/overview.md) の概要を参照してください。

ベストプラクティスは、データのジャーニーのすべての段階で、データのコンプライアンスに取り組むことです。 このため、アドホックスキーマを使用する派生データセットには、データガバナンスフレームワークの一部として適切にラベルを付ける必要があります。 クエリサービスで作成される派生データセットには、標準スキーマを使用するデータセットとアドホックスキーマを使用するデータセットの 2 種類があります。

>[!NOTE]
>
>クエリサービスを使用して作成されたデータセットは、「派生データセット」と呼ばれます。

アドホックスキーマは、特定の目的で個々のユーザーによって作成されるので、XDM スキーマフィールドは、その特定のデータセット用に名前空間が設定され、異なるデータセット間での使用は意図されていません。 その結果、Experience Platform UI ではアドホックスキーマはデフォルトでは表示されません。 標準スキーマとアドホックスキーマの両方でデータ使用ラベルの適用に違いはありませんが、ラベル設定を目的としてクエリサービスで作成されたアドホックスキーマは、まずExperience Platform UI で表示できるようにする必要があります。 詳しくは、[Experience Platform UI 内のアドホックスキーマの検出 ](./ad-hoc-schema-labels.md#discover-ad-hoc-schemas) のガイドを参照してください。

スキーマにアクセスしたら、[ 個々のフィールドにラベルを適用 ](../../xdm/tutorials/labels.md) できます。 スキーマにラベルが付けられると、そのスキーマから派生したすべてのデータセットがこれらのラベルを継承します。 ここから、特定のラベルを持つデータが特定の宛先に対してアクティブ化されることを制限できるデータ使用ポリシーを設定できます。 詳しくは、[ データ使用ポリシー ](../../data-governance/policies/overview.md) の概要を参照してください。

## プライバシー {#privacy}

[Privacy Service](../../privacy-service/home.md) は、法的なプライバシー規制に従ってデータへのアクセスおよび削除を求める顧客のリクエストを管理するのに役立ちます。 これを行うには、データを既存の識別子で検索し、要求されたプライバシージョブに応じて、そのデータにアクセスまたは削除します。 サービスがプライバシージョブ中にアクセスまたは削除するフィールドを決定するには、データに適切なラベルを付ける必要があります。プライバシーリクエストの対象となるデータには、プライバシーリクエストが適用される個人と様々なデータを結び付けるために、顧客 ID 情報が含まれている必要があります。 クエリサービスは、プライバシージョブを満たすために、一意の識別子を使用して使用するデータを強化できます。

プライバシーリクエストは、データレイクまたはプロファイルデータストアに送信できます。 データレイクからレコードを削除しても、それらのレコードから作成されたプロファイルは削除されません。 また、データレイクから個人情報を削除するプライバシージョブは、プロファイルを削除しないので、プライバシージョブの完了後に取り込まれた情報（そのプロファイル ID を含む）は、通常どおりそのプロファイルを更新します。 これにより、ホックスキーマで使用されるデータを適切に識別する必要性が再確認されます。

[ プライバシーリクエストの ID データ ](../../privacy-service/identity-data.md) と、データ操作を設定し、Privacy Service テクノロジーを活用して、顧客のプライバシーリクエストに適切な ID 情報を効果的に取得する方法について詳しくは、Adobeのドキュメントを参照してください。

データガバナンス用のクエリサービス機能は、データの分類プロセスとデータ使用規制への準拠を簡素化および効率化します。 データが識別されると、クエリサービスを使用して、すべての出力データセットにプライマリ ID を割り当てることができます。 ID をデータセットに追加して **必**）、データプライバシーリクエストを容易にし、データコンプライアンスに取り組みます。

スキーマデータフィールドは、Experience Platform UI を使用して ID フィールドとして設定できます。また、クエリサービスでは [SQL コマンド「ALTER TABLE」を使用してプライマリ ID をマーク ](../sql/syntax.md#alter-table) できます。 `ALTER TABLE` コマンドを使用した ID 設定は、Experience Platform UI を使用してスキーマから直接作成するのではなく、SQL を使用してデータセットを作成する場合に特に便利です。 標準スキーマを使用する際に [UI で ID フィールドを定義 ](../../xdm/ui/fields/identity.md) する方法については、ドキュメントを参照してください。

## データハイジーン {#data-hygiene}

「データハイジーン」とは、古い、不正確な、間違った形式の、重複した、または不完全な可能性のあるデータを修復または削除するプロセスを指します。 これらのプロセスにより、データセットがすべてのシステムで正確で一貫性のあるものになります。 データのジャーニーのすべてのステップに沿って、また最初のデータストレージの場所からも、適切なデータハイジーンを確保することが重要です。 Experience Platform クエリサービスでは、これはデータレイクまたは高速ストアのいずれかです。

Experience Platformの一元化されたデータハイジーンサービスに従って、派生データセットに ID を割り当てて、データ管理を許可できます。

逆に、高速化ストアで集計データセットを作成すると、集計データを使用して元のデータを取得することはできません。 このデータ集計の結果、データハイジーンリクエストを増やす必要がなくなります。

このシナリオの例外は、削除の場合です。 データセットでデータハイジーンの削除がリクエストされた場合、削除が完了する前に、別の派生データセットクエリが実行されると、派生データセットは元のデータセットの情報を取得します。 この場合、データセットの削除リクエストが送信された場合、同じデータセットソースを使用して新しく派生したデータセットクエリを実行しないでください。

Adobe Experience Platformのデータハイジーンについて詳しくは、[ データハイジーンの概要 ](../../hygiene/home.md) を参照してください。
