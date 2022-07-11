---
description: このページでは、「/authoring/sample-profiles」 API エンドポイントを使用して、宛先テストで使用するサンプルプロファイルを生成するために実行できるすべての API 操作について説明します。
title: サンプルプロファイル生成 API の操作
exl-id: 5f1cd00a-8eee-4454-bcae-07b05afa54af
source-git-commit: 789a3928379d200af292c722806f7ca72441d9f3
workflow-type: tm+mt
source-wordcount: '975'
ht-degree: 15%

---

# サンプルプロファイル生成 API の操作 {#sample-profile-api-operations}

>[!IMPORTANT]
>
>**API エンドポイント**：`https://platform.adobe.io/data/core/activation/authoring/sample-profiles`

このページでは、`/authoring/sample-profiles` API エンドポイントを使用して実行できるすべての API の操作について説明します。

## 様々な API に対して異なるプロファイルタイプを生成する {#different-profiles-different-apis}

>[!IMPORTANT]
>
>この API エンドポイントを使用して、2 つの異なる使用例のサンプルプロファイルを生成します。 次のいずれかを実行できます。
>* 次の場合に使用するプロファイルを生成 [メッセージ変換テンプレートの作成とテスト](./create-template.md)  — 使用 *宛先 ID* をクエリパラメーターとして設定します。
>* を呼び出す際に使用するプロファイルを生成する [宛先が正しく設定されているかどうかをテストします。](./test-destination.md)  — 使用 *宛先インスタンス ID* をクエリパラメーターとして設定します。


Adobeの XDM ソーススキーマ（宛先のテスト時に使用）または宛先でサポートされているターゲットスキーマ（テンプレートの作成時に使用）に基づいて、サンプルプロファイルを生成できます。 AdobeXDM ソーススキーマとターゲットスキーマの違いを理解するには、 [メッセージの形式](./message-format.md) 記事。

サンプルプロファイルを使用する目的は入れ替えできないことに注意してください。 に基づいて生成されたプロファイル *宛先 ID* は、 *宛先インスタンス ID* は、宛先エンドポイントのテストにのみ使用できます。

## サンプルプロファイル生成 API 操作の概要 {#get-started}

続ける前に「[はじめる前に](./getting-started.md)」を参照し、必要な宛先オーサリング権限および必要なヘッダーの取得方法など、API の呼び出しを正常に行うために必要となる重要な情報を確認してください。

## 宛先のテスト時に使用するソーススキーマに基づいてサンプルプロファイルを生成します {#generate-sample-profiles-source-schema}

>[!IMPORTANT]
>
>ここで生成されたサンプルプロファイルを HTTP 呼び出しの [宛先のテスト](./test-destination.md).

に対してGETリクエストを実行することで、ソーススキーマに基づいてサンプルプロファイルを生成できます `authoring/sample-profiles/` エンドポイントを作成し、テストする宛先設定に基づいて作成した宛先インスタンスの ID を指定します。

宛先インスタンスの ID を取得するには、まず、Experience PlatformUI で宛先への接続を作成してから、宛先のテストを試みる必要があります。 詳しくは、 [宛先のチュートリアルをアクティブ化](/help/destinations/ui/activation-overview.md) この API で使用する宛先インスタンス ID を取得する方法については、以下のヒントを参照してください。

>[!TIP]
>
>* 宛先との接続を参照する際に、URL から、ここで使用する必要がある宛先インスタンス ID を取得します。
   >![UI 画像宛先インスタンス ID の取得方法](./assets/get-destination-instance-id.png)


**API 形式**


```http
GET authoring/sample-profiles?destinationInstanceId={DESTINATION_INSTANCE_ID}&count={COUNT}
```

| クエリパラメータ | 説明 |
| -------- | ----------- |
| `{DESTINATION_INSTANCE_ID}` | サンプルプロファイルを生成する際に基づく宛先インスタンスの ID。 |
| `{COUNT}` | *オプション*。生成するサンプルプロファイルの数。 パラメーターは、 `1 - 1000`. <br> count パラメーターが指定されていない場合、生成されるプロファイルのデフォルト数は、 `maxUsersPerRequest` 値を [宛先サーバーの設定](./destination-server-api.md#create). このプロパティを定義しない場合、Adobeは 1 つのサンプルプロファイルを生成します。 |

{style=&quot;table-layout:auto&quot;}


**リクエスト**

次のリクエストは、 `{DESTINATION_INSTANCE_ID}` および `{COUNT}` クエリパラメーター。

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/activation/authoring/sample-profiles?destinationInstanceId=49966037-32cd-4457-a105-2cbf9c01826a&count=3' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、指定された数のサンプルプロファイルと共に HTTP ステータス 200 を返します。このとき、セグメントメンバーシップ、ID、およびソース XDM スキーマに対応するプロファイル属性が返されます。

>[!TIP]
>
> 応答は、宛先インスタンスで使用されるセグメントのメンバーシップ、ID、プロファイル属性のみを返します。 ソーススキーマに他のフィールドが含まれている場合でも、それらは無視されます。

```json
[
    {
        "segmentMembership": {
            "ups": {
                "03fb9938-8537-4b4c-87f9-9c4d413a0ee5": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591378Z",
                    "status": "realized"
                },
                "27e05542-d6a3-46c7-9c8e-d59d50229530": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591380Z",
                    "status": "realized"
                }
            }
        },
        "personalEmail": {
            "address": "john.smith@abc.com"
        },
        "identityMap": {
            "ECID": [
                {
                    "id": "ECID-7VEsJ"
                }
            ]
        },
        "person": {
            "name": {
                "firstName": "string"
            }
        }
    },
    {
        "segmentMembership": {
            "ups": {
                "03fb9938-8537-4b4c-87f9-9c4d413a0ee5": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591378Z",
                    "status": "realized"
                },
                "27e05542-d6a3-46c7-9c8e-d59d50229530": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591380Z",
                    "status": "realized"
                }
            }
        },
        "personalEmail": {
            "address": "john.smith@abc.com"
        },
        "identityMap": {
            "ECID": [
                {
                    "id": "ECID-Y55JJ"
                }
            ]
        },
        "person": {
            "name": {
                "firstName": "string"
            }
        }
    },
    {
        "segmentMembership": {
            "ups": {
                "03fb9938-8537-4b4c-87f9-9c4d413a0ee5": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591378Z",
                    "status": "realized"
                },
                "27e05542-d6a3-46c7-9c8e-d59d50229530": {
                    "lastQualificationTime": "2021-06-30T18:40:07.591380Z",
                    "status": "realized"
                }
            }
        },
        "personalEmail": {
            "address": "john.smith@abc.com"
        },
        "identityMap": {
            "ECID": [
                {
                    "id": "ECID-Nd9GK"
                }
            ]
        },
        "person": {
            "name": {
                "firstName": "string"
            }
        }
    }
]
```

| プロパティ | 説明 |
| -------- | ----------- |
| `segmentMembership` | 個人のセグメントメンバーシップを表す map オブジェクト。 詳しくは、 `segmentMembership`，読み取り [セグメントメンバーシップの詳細](https://experienceleague.adobe.com/docs/experience-platform/xdm/field-groups/profile/segmentation.html). |
| `lastQualificationTime` | このプロファイルが最後にセグメントで認定された時刻のタイムスタンプ。 |
| `xdm:status` | 現在のリクエストの一環としてセグメントのメンバーシップが認識されたかどうかを示す文字列フィールド。 次の値を使用できます。 <ul><li>`existing`:プロファイルは、リクエストの前に既にセグメントに含まれていて、引き続きメンバーシップを維持します。</li><li>`realized`:プロファイルは、現在のリクエストの一部としてセグメントに入っています。</li><li>`exited`:プロファイルは、現在のリクエストの一環としてセグメントから退出しています。</li></ul> |
| `identityMap` | 個々の ID の様々な値と、関連する名前空間を説明する map-type フィールドです。 詳しくは、 `identityMap`，読み取り [スキーマ構成の基本](https://experienceleague.adobe.com/docs/experience-platform/xdm/schema/composition.html?lang=en#identityMap). |

{style=&quot;table-layout:auto&quot;}

## メッセージ変換テンプレートの作成時に使用するターゲットスキーマに基づいて、サンプルプロファイルを生成します {#generate-sample-profiles-target-schema}

>[!IMPORTANT]
>
>テンプレートを作成する際にここで生成されたサンプルプロファイルを、 [テンプレートレンダリングステップ](./render-template-api.md#multiple-profiles-with-body).

に対してGETリクエストをおこなうターゲットスキーマに基づいて、サンプルプロファイルを生成できます `authoring/sample-profiles/` エンドポイントを作成し、テンプレートを作成する場所に基づいて宛先設定の宛先 ID を指定します。

>[!TIP]
>
>* ここで使用する必要がある宛先 ID は `instanceId` で、`/destinations` エンドポイントを使用して作成された、宛先の設定に対応します。詳しくは、 [宛先設定 API リファレンス](./destination-configuration-api.md#retrieve-list).


**API 形式**


```http
GET authoring/sample-profiles?destinationId={DESTINATION_ID}&count={COUNT}
```

| クエリパラメータ | 説明 |
| -------- | ----------- |
| `{DESTINATION_ID}` | サンプルプロファイルを生成する場所に基づく宛先設定の ID。 |
| `{COUNT}` | *オプション*。生成するサンプルプロファイルの数。 パラメーターは、 `1 - 1000`. <br> count パラメーターが指定されていない場合、生成されるプロファイルのデフォルト数は、 `maxUsersPerRequest` 値を [宛先サーバーの設定](./destination-server-api.md#create). このプロパティを定義しない場合、Adobeは 1 つのサンプルプロファイルを生成します。 |

{style=&quot;table-layout:auto&quot;}

**リクエスト**

次のリクエストは、 `{DESTINATION_ID}` および `{COUNT}` クエリパラメーター。

```shell
curl --location --request GET 'https://platform.adobe.io/data/core/activation/authoring/sample-profiles?destinationId=49966037-32cd-4457-a105-2cbf9c01826a&count=3' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'x-api-key: {API_KEY}' \
--header 'Authorization: Bearer {ACCESS_TOKEN}' \
--header 'x-gw-ims-org-id: {ORG_ID}' \
--header 'x-sandbox-name: {SANDBOX_NAME}' \
```

**応答**

正常な応答は、指定された数のサンプルプロファイルと共に HTTP ステータス 200 と、ターゲット XDM スキーマに対応するセグメントメンバーシップ、ID、プロファイル属性を返します。

```json
[
    {
        "segmentMembership": {
            "ups": {
                "segmentid1": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609326Z",
                    "status": "existing"
                },
                "segmentid3": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609328Z",
                    "status": "exited"
                },
                "segmentid2": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609328Z",
                    "status": "realized"
                }
            }
        },
        "identityMap": {
            "phone_sha256": [
                {
                    "id": "phone_sha256-vizii"
                }
            ],
            "gaid": [
                {
                    "id": "gaid-adKYs"
                }
            ],
            "idfa": [
                {
                    "id": "idfa-t4sKv"
                }
            ],
            "extern_id": [
                {
                    "id": "extern_id-C3enB"
                }
            ],
            "email_lc_sha256": [
                {
                    "id": "email_lc_sha256-bfnbs"
                }
            ]
        }
    },
    {
        "segmentMembership": {
            "ups": {
                "segmentid1": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609626Z",
                    "status": "existing"
                },
                "segmentid3": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609627Z",
                    "status": "exited"
                },
                "segmentid2": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609627Z",
                    "status": "realized"
                }
            }
        },
        "identityMap": {
            "phone_sha256": [
                {
                    "id": "phone_sha256-6YjGc"
                }
            ],
            "gaid": [
                {
                    "id": "gaid-SfJ21"
                }
            ],
            "idfa": [
                {
                    "id": "idfa-eQMWS"
                }
            ],
            "extern_id": [
                {
                    "id": "extern_id-d3WzP"
                }
            ],
            "email_lc_sha256": [
                {
                    "id": "email_lc_sha256-eWfFn"
                }
            ]
        }
    },
    {
        "segmentMembership": {
            "ups": {
                "segmentid1": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609823Z",
                    "status": "existing"
                },
                "segmentid3": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609824Z",
                    "status": "exited"
                },
                "segmentid2": {
                    "lastQualificationTime": "2021-06-30T18:42:27.609824Z",
                    "status": "realized"
                }
            }
        },
        "identityMap": {
            "phone_sha256": [
                {
                    "id": "phone_sha256-2PMjZ"
                }
            ],
            "gaid": [
                {
                    "id": "gaid-3aLez"
                }
            ],
            "idfa": [
                {
                    "id": "idfa-D2H1J"
                }
            ],
            "extern_id": [
                {
                    "id": "extern_id-i6PsF"
                }
            ],
            "email_lc_sha256": [
                {
                    "id": "email_lc_sha256-VPUtZ"
                }
            ]
        }
    }
]
```

## API エラー処理 {#api-error-handling}

Destination SDK API エンドポイントは、一般的な Experience Platform API エラーメッセージの原則に従います。Platform トラブルシューティングガイドの [API ステータスコード](../../landing/troubleshooting.md#api-status-codes)および[リクエストヘッダーエラー](../../landing/troubleshooting.md#request-header-errors)を参照してください。

## 次の手順

このドキュメントを読むと、次の場合に使用するサンプルプロファイルを生成する方法がわかります。 [メッセージ変換テンプレートのテスト](./create-template.md) または [宛先が正しく設定されているかどうかのテスト](./test-destination.md).
