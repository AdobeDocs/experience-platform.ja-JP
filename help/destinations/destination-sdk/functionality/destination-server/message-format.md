---
description: このページでは、Adobe Experience Platform から宛先に書き出されたデータのメッセージ形式およびプロファイル変換について説明します。
title: メッセージ形式
source-git-commit: e500d05858a3242295c6e5aac8284ad301d0cd17
workflow-type: ht
source-wordcount: '2237'
ht-degree: 100%

---


# メッセージ形式

## 前提条件 - Adobe Experience Platform の概念 {#prerequisites}

メッセージ形式、プロファイル設定、アドビ側での変換プロセスについて把握するには、以下の Experience Platform の概念についてよく理解してください。

* **エクスペリエンスデータモデル（XDM）**。[XDM の概要](../../../../xdm/home.md)および [Adobe Experience Platform での XDM スキーマの作成方法](../../../../xdm/tutorials/create-schema-ui.md)。
* **クラス**。[UI でのクラスの作成と編集](../../../../xdm/ui/resources/classes.md)。
* **identityMap**。ID マップは、Adobe Experience Platform のすべてのエンドユーザー ID のマップを表します。[XDM フィールド辞書](../../../../xdm/schema/field-dictionary.md)の `xdm:identityMap` を参照してください。
* **segmentMembership**。[segmentMembership](../../../../xdm/schema/field-dictionary.md) XDM 属性は、プロファイルが属するオーディエンスを知らせます。`status` フィールドの 3 つの異なる値については、[オーディエンスメンバーシップの詳細スキーマフィールドグループ](../../../../xdm/field-groups/profile/segmentation.md)に関するドキュメントを参照してください。

>[!IMPORTANT]
>
>Destination SDK でサポートされているすべてのパラメーター名および値は、**大文字と小文字が区別**&#x200B;されます。大文字と小文字を区別することに関するエラーを避けるために、ドキュメントに示すように、パラメーター名および値を正確に使用してください。

## サポートされる統合タイプ {#supported-integration-types}

このページで説明される機能をサポートする統合のタイプについて詳しくは、以下の表を参照してください。

| 統合タイプ | 機能のサポート |
|---|---|
| リアルタイム（ストリーミング）統合 | ○ |
| ファイルベースの（バッチ）統合 | ○（後述の図の手順 1 および 2 のみ） |

## 概要 {#overview}

このページでは、Adobe Experience Platform から宛先に書き出されたデータのメッセージ形式およびプロファイル変換について説明します。

Adobe Experience Platform は、様々なデータ形式で、数多くの宛先にデータを書き出します。宛先タイプの例の一部には、広告プラットフォーム（Google）、ソーシャルネットワーク（Facebook）、クラウドストレージの場所（Amazon S3、Azure Event Hubs）があります。

Experience Platform は、お客様側で想定される形式に一致するように、書き出されたプロファイルのメッセージ形式を調整できます。このカスタマイズを理解するには、以下の概念が重要です。

* Adobe Experience Platform のソース（1）およびターゲット（2）XDM スキーマ
* パートナー側で想定されるメッセージ形式（3）
* XDM スキーマと想定されるメッセージ形式の間の変換レイヤー（[メッセージ変換テンプレート](#using-templating)を作成することで定義できます）。

![JSON 変換に対するスキーマ](../../assets/functionality/destination-server/transformations-3-steps.png)

Experience Platform では、XDM スキーマを使用して、一貫性のある再利用可能な方法でデータの構造を記述します。

<!--

Users who want to activate data to your destination need to map the fields in their Experience Platform datasets to a schema that translates to your destination's expected format. Adobe will create a custom field group for your company to add to the target schema. The fields in the field group depend on the profile attribute fields that you can receive.

-->

**ソース XDM スキーマ（1）**：この項目は、顧客が Experience Platform で使用するスキーマを参照します。Experience Platform では、宛先のアクティブ化ワークフローの[マッピング手順](../../../ui/activate-segment-streaming-destinations.md#mapping)で、顧客が XDM スキーマから宛先のターゲットスキーマ（2）にフィールドをマッピングします。

**ターゲット XDM スキーマ（2）**：宛先が解釈できる、宛先の想定される形式および属性の JSON 標準スキーマ（3）に基づいて、ターゲット XDM スキーマのプロファイル属性および ID を定義できます。[schemaConfig](../../functionality/destination-configuration/schema-configuration.md) および [identityNamespaces](../../functionality/destination-configuration/identity-namespace-configuration.md) オブジェクトの宛先設定でこれを実行できます。

**宛先プロファイル属性の JSON 標準スキーマ（3）**：この例では、プラットフォームがサポートするすべてのプロファイル属性とそのタイプ（例：オブジェクト、文字列、配列）の [JSON スキーマ](https://json-schema.org/learn/miscellaneous-examples.html)を表します。宛先がサポートできるフィールドの例として、`firstName`、`lastName`、`gender`、`email`、`phone`、`productId`、`productName` などがあります。Experience Platform から書き出されたデータを想定される形式にカスタマイズするには、[メッセージ変換テンプレート](#using-templating)が必要です。

上記のスキーマ変換に基づいて、プロファイル設定がソース XDM スキーマとパートナー側のサンプルスキーマの間でどのように変化するかを示します。

![変換メッセージの例](../../assets/functionality/destination-server/transformations-with-examples.png)

## はじめに - 3 つの基本属性の変換 {#getting-started}

プロファイル変換プロセスを示すために、以下の例では、Adobe Experience Platform で一般的な 3 つのプロファイル属性（**名**、**姓**&#x200B;および&#x200B;**メールアドレス**）を使用しています。

>[!NOTE]
>
>顧客は、[宛先のアクティブ化ワークフロー](../../../ui/activate-segment-streaming-destinations.md#mapping)の&#x200B;**マッピング**&#x200B;手順で、Adobe Experience Platform UI でソース XDM スキーマからパートナー XDM スキーマに属性をマッピングします。

プラットフォームが以下のようなメッセージ形式を受信できるとします。

```shell
POST https://YOUR_REST_API_URL/users/
Content-Type: application/json
Authorization: Bearer YOUR_REST_API_KEY

{
  "attributes":
    {
      "first_name": "Yours",
      "last_name": "Truly",
      "external_id": "yourstruly@adobe.com"
    }
}
```

メッセージ形式を考慮した、対応する変換を以下に示します。

| アドビ側でのパートナー XDM スキーマの属性 | 変換 | お客様側の HTTP メッセージの属性 |
|---------|----------|---------|
| `_your_custom_schema.firstName` | ` attributes.first_name` | `first_name` |
| `_your_custom_schema.lastName` | `attributes.last_name` | `last_name` |
| `personalEmail.address` | `attributes.external_id` | `external_id` |

{style="table-layout:auto"}

## Experience Platform のプロファイル構造 {#profile-structure}

このページで後述する例を理解するには、Experience Platform でのプロファイルの構造を知ることが重要です。

プロファイルには、以下の 3 つのセクションがあります。

* `segmentMembership`（常にプロファイルに存在）
   * このセクションには、プロファイルに存在するすべてのオーディエンスが含まれます。オーディエンスのステータスは、2 つ（`realized` または `exited`）のいずれかになります。
* `identityMap`（常にプロファイルに存在）
   * このセクションには、プロファイル（メール、Google GAID、Apple IDFA など）に存在し、アクティベーションワークフローで書き出すためにユーザーがマッピングしたすべての ID が含まれます。
* 属性（宛先設定に応じて、これらはプロファイルに存在する可能性があります）。また、事前定義済みの属性とフリーフォーム属性には、注意すべきわずかな違いがあります。
   * *フリーフォーム属性*&#x200B;では、属性がプロファイルに存在する場合、`.value` パスが含まれます（例 1 の `lastName` 属性を参照）。属性がプロファイルに存在しない場合は、`.value` パスは含まれません（例 1 の `firstName` 属性を参照）。
   * *事前定義済みの属性*&#x200B;では、`.value` パスは含まれません。プロファイルに存在する、マッピングされたすべての属性は、属性マップに存在します。存在しない属性は存在しません（例 2 を参照 - `firstName` 属性はプロファイルに存在しません）。

Experience Platform のプロファイルの以下の 2 つの例を参照してください。

### 例 1 `segmentMembership`、`identityMap` およびフリーフォーム属性の属性を含む {#example-1}

```json
{
  "segmentMembership": {
    "ups": {
      "11111111-1111-1111-1111-111111111111": {
        "lastQualificationTime": "2019-04-15T02:41:50.000+0000",
        "status": "realized"
      }
    }
  },
  "identityMap": {
    "mobileIds": [
      {
        "id": "e86fb215-0921-4537-bc77-969ff775752c"
      }
    ]
  },
  "attributes": {
    "firstName": {
    },
    "lastName": {
      "value": "lastName"
    }
  }
}
```

### 例 2 `segmentMembership`、`identityMap` および事前定義済みの属性の属性を含む {#example-2}

```json
{
  "segmentMembership": {
    "ups": {
      "11111111-1111-1111-1111-111111111111": {
        "lastQualificationTime": "2019-04-15T02:41:50.000+0000",
        "status": "realized"
      }
    }
  },
  "identityMap": {
    "mobileIds": [
      {
        "id": "e86fb215-0921-4537-bc77-969ff775752c"
      }
    ]
  },
  "attributes": {
    "lastName": "lastName"
  }
}
```

## ID、属性およびオーディエンスメンバーシップの変換へのテンプレート言語の使用 {#using-templating}

アドビは [Jinja](https://jinja.palletsprojects.com/en/2.11.x/) と類似したテンプレート言語である [Pebble テンプレート](https://pebbletemplates.io/)を使用して、Experience Platform XDM スキーマのフィールドを宛先でサポートされる形式に変換します。

この節では、これらの変換がどのように行われるか（入力 XDM スキーマから、テンプレートを経て、宛先で受け入れられるペイロード形式に出力するまで）について、いくつかの例を示します。後述の例は、以下のように複雑さが増していきます。

1. シンプルな変換例。[プロファイル属性](#attributes)、[オーディエンスメンバーシップ](#segment-membership)および [ID](#identities) フィールドに対するシンプルな変換でのテンプレートの仕組みを説明します。
2. 上記のフィールドを組み合わせてテンプレートの複雑さが増した例：[オーディエンスおよび ID を送信するテンプレートの作成](./message-format.md#segments-and-identities)および[セグメント、ID およびプロファイル属性を送信するテンプレートの作成](#segments-identities-attributes)。
3. 集約キーが含まれるテンプレート。宛先設定で[設定可能な集約](../../functionality/destination-configuration/aggregation-policy.md#configurable-aggregation)を使用する場合、Experience Platform は、条件（オーディエンス ID、オーディエンスステータス、ID 名前空間など）に基づいて、宛先に書き出されたプロファイルをグループ化します。

### プロファイル属性 {#attributes}

宛先に書き出されたプロファイル属性を変換するには、以下の JSON およびコードサンプルを参照してください。

>[!IMPORTANT]
>
>Adobe Experience Platform で使用できるすべてのプロファイル属性のリストについては、[XDM フィールド辞書](../../../../xdm/schema/field-dictionary.md)を参照してください。


**入力**

プロファイル 1：

```json
{
    "attributes": {
        "firstName": {
            "value": "Hermione"
    },
    "birthDate": {}
  }
}
```

プロファイル 2：

```json
{
  "attributes": {
    "firstName": {
      "value": "Harry"
    },
    "birthDate": {
        "value": "1980/07/31"
    }
  }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートについて、[宛先サーバー設定](../../authoring-api/destination-server/create-destination-server.md)に[テンプレート](../../functionality/destination-server/templating-specs.md)を挿入する前に、無効な文字（二重引用符 `""` など）をエスケープする必要があります。二重引用符のエスケープについて詳しくは、[JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)の第 9 章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            {% for attribute in profile.attributes %}
            "{{ attribute.key }}":
                {% if attribute.value is empty %}
                    null
                {% else %}
                    "{{ attribute.value.value }}"
                {% endif %}
            {% if not loop.last %},{% endif %}
            {% endfor %}
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**


```json
{
    "profiles": [
        {
            "firstName": "Hermione",
            "birthDate": null
        },
        {
            "firstName": "Harry",
            "birthDate": "1980/07/31"
        }
    ]
}
```

### オーディエンスのメンバーシップ {#audience-membership}

[segmentMembership](../../../../xdm/schema/field-dictionary.md) XDM 属性は、プロファイルが属するのはどのオーディエンスかを知らせます。
`status` フィールドの 3 つの異なる値については、[オーディエンスメンバーシップの詳細スキーマフィールドグループ](../../../../xdm/field-groups/profile/segmentation.md)に関するドキュメントを参照してください。

**入力**

プロファイル 1：

```json
{
  "segmentMembership": {
    "ups": {
      "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "realized"
      },
      "788d8874-8007-4253-92b7-ee6b6c20c6f3": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "realized"
      },
      "8f812592-3f06-416b-bd50-e7831848a31a": {
        "lastQualificationTime": "2019-11-20T13:15:49Z",
        "status": "exited"
      }
    }
  }
}
```

プロファイル 2：

```json
{
  "segmentMembership": {
    "ups": {
      "32396e4b-16f6-4033-9702-fc69b5e24e7c": {
        "lastQualificationTime": "2021-08-20T17:23:04Z",
        "status": "realized"
      },
      "af854278-894a-4192-a96b-320fbf2623fd": {
        "lastQualificationTime": "2021-08-20T16:44:37Z",
        "status": "realized"
      },
      "66505bf9-bc08-4bac-afbc-8b6706650ea4": {
        "lastQualificationTime": "2019-08-20T17:23:04Z",
        "status": "realized"
      }
    }
  }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートについて、[宛先サーバー設定](../../authoring-api/destination-server/create-destination-server.md)に[テンプレート](../../functionality/destination-server/templating-specs.md)を挿入する前に、無効な文字（二重引用符 `""` など）をエスケープする必要があります。二重引用符のエスケープについて詳しくは、[JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)の第 9 章を参照してください。


```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "AdobeExperiencePlatformSegments": {
                "add": [
                {% for segment in profile.segmentMembership.ups | added %}
                "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ],
                "remove": [
                {# Alternative syntax for filtering audiences by status: #}
                {% for segment in removedSegments(profile.segmentMembership.ups) %}
                "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ]
            }
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

```json
{
    "profiles": [
        {
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075",
                    "788d8874-8007-4253-92b7-ee6b6c20c6f3"
                ],
                "remove": [
                    "8f812592-3f06-416b-bd50-e7831848a31a"
                ]
            }
        },
        {
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "32396e4b-16f6-4033-9702-fc69b5e24e7c",
                    "af854278-894a-4192-a96b-320fbf2623fd",
                    "66505bf9-bc08-4bac-afbc-8b6706650ea4"
                ],
                "remove": [
                ]
            }
        }
    ]
}
```

### ID {#identities}

Experience Platform の ID について詳しくは、[ID 名前空間の概要](../../../../identity-service/namespaces.md)を参照してください。

**入力**

プロファイル 1：

```json
{
    "identityMap": {
        "email": [
            {
                "id": "johndoe@example.com"
            },
            {
                "id": "jd@example.com"
            }
        ],
        "external_id": [
            {
                "id": "123456"
            }
        ]
    }
}
```

プロファイル 2：

```json
{
    "identityMap": {
        "email": [
            {
                "id": "jane.doe@example.com"
            }
        ]
    }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートについて、[宛先サーバー設定](../../authoring-api/destination-server/create-destination-server.md)に[テンプレート](../../functionality/destination-server/templating-specs.md)を挿入する前に、無効な文字（二重引用符 `""` など）をエスケープする必要があります。二重引用符のエスケープについて詳しくは、[JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)の第 9 章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "identities": [
                {% for email in profile.identityMap.email %}
                {
                    "type": "email",
                    "id": "{{ email.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}

                {# Add a comma only if you have both emails and external_ids. #}
                {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}
                    ,
                {% endif %}

                {% for external in profile.identityMap.external_id %}
                {
                    "type": "external_id",
                    "id": "{{ external.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
            ]
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

```json
{
    "profiles": [
        {
            "identities": [
                {
                    "type": "email",
                    "id": "johndoe@example.com"
                },
                {
                    "type": "email",
                    "id": "jd@example.com"
                },
                {
                    "type": "external_id",
                    "id": "123456"
                }
            ]
        },
        {
            "identities": [
                {
                    "type": "email",
                    "id": "jane.doe@example.com"
                }
            ]
        }
    ]
}
```

### オーディエンスおよび ID を送信するテンプレートの作成 {#segments-and-identities}

この節では、Adobe XDM スキーマとパートナー宛先スキーマの間で一般的に使用される変換の例を示します。
以下の例に、オーディエンスメンバーシップおよび ID 形式の変換方法と宛先への出力方法を示します。

**入力**

プロファイル 1：

```json
{
    "identityMap": {
        "email": [
            {
                "id": "johndoe@example.com"
            },
            {
                "id": "jd@example.com"
            }
        ],
        "external_id": [
            {
                "id": "123456"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "realized"
            },
            "788d8874-8007-4253-92b7-ee6b6c20c6f3": {
              "lastQualificationTime": "2019-11-20T13:15:49Z",
              "status": "realized"
            },
            "8f812592-3f06-416b-bd50-e7831848a31a": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "exited"
            }
        }
    }
}
```

プロファイル 2：

```json
{
    "identityMap": {
        "email": [
            {
                "id": "jane.doe@example.com"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2021-08-31T10:01:42Z",
                "status": "realized"
            }
        }
    }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートについて、[宛先サーバー設定](../../authoring-api/destination-server/create-destination-server.md)に[テンプレート](../../functionality/destination-server/templating-specs.md)を挿入する前に、無効な文字（二重引用符 `""` など）をエスケープする必要があります。二重引用符のエスケープについて詳しくは、[JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)の第 9 章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "identities": [
                {% for email in profile.identityMap.email %}
                {
                    "type": "email",
                    "id": "{{ email.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
                
                {# Add a comma only if you have both emails and external_ids. #}
                {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}
                    ,
                {% endif %}
                
                {% for external in profile.identityMap.external_id %}
                {
                    "type": "external_id",
                    "id": "{{ external.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    {% for segment in profile.segmentMembership.ups | added %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                    {% endfor %}
                ],
                "remove": [
                    {# Alternative syntax for filtering audiences by status: #}
                    {% for segment in removedSegments(profile.segmentMembership.ups) %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                    {% endfor %}
                ]
            }
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

以下の `json` は、Adobe Experience Platform から書き出されたデータを表します。

```json
{
    "profiles": [
        {
            "identities": [
                {
                    "type": "email",
                    "id": "johndoe@example.com"
                },
                {
                    "type": "email",
                    "id": "jd@example.com"
                },
                {
                    "type": "external_id",
                    "id": "123456"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075",
                    "788d8874-8007-4253-92b7-ee6b6c20c6f3"
                ],
                "remove": [
                    "8f812592-3f06-416b-bd50-e7831848a31a"
                ]
            }
        },
        {
            "identities": [
                {
                    "type": "email",
                    "id": "jane.doe@example.com"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075"
                ],
                "remove": []
            }
        }
    ]
}
```

### セグメント、ID およびプロファイル属性を送信するテンプレートの作成 {#segments-identities-attributes}

この節では、Adobe XDM スキーマとパートナー宛先スキーマの間で一般的に使用される変換の例を示します。

もうひとつの一般的なユースケースは、オーディエンスメンバーシップ、ID（例：メールアドレス、電話番号、広告 ID）およびプロファイル属性が含まれるデータの書き出しです。この方法でデータを書き出すには、以下の例を参照してください。

**入力**

プロファイル 1：

```json
{
    "attributes": {
        "firstName": {
            "value": "Hermione"
        },
        "birthDate": {}
    },
    "identityMap": {
        "email": [
            {
                "id": "johndoe@example.com"
            },
            {
                "id": "jd@example.com"
            }
        ],
        "external_id": [
            {
                "id": "123456"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "realized"
            },
            "788d8874-8007-4253-92b7-ee6b6c20c6f3": {
              "lastQualificationTime": "2019-11-20T13:15:49Z",
              "status": "realized"
            },
            "8f812592-3f06-416b-bd50-e7831848a31a": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "exited"
            }
        }
    }
}
```

プロファイル 2：

```json
{
    "attributes": {
        "firstName": {
            "value": "Harry"
        },
        "birthDate": {
            "value": "1980/07/31"
        }
    },
    "identityMap": {
        "email": [
            {
                "id": "harry.p@example.com"
            }
        ]
    },
    "segmentMembership": {
        "ups": {
            "36a51c13-9dd6-4d2c-8aa3-07d785ea5075": {
                "lastQualificationTime": "2019-11-20T13:15:49Z",
                "status": "realized"
            }
        }
    }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートについて、[宛先サーバー設定](../../authoring-api/destination-server/create-destination-server.md)に[テンプレート](../../functionality/destination-server/templating-specs.md)を挿入する前に、無効な文字（二重引用符 `""` など）をエスケープする必要があります。二重引用符のエスケープについて詳しくは、[JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)の第 9 章を参照してください。

```python
{
    "profiles": [
        {% for profile in input.profiles %}
        {
            "attributes": {
            {% for attribute in profile.attributes %}
                "{{ attribute.key }}":
                    {% if attribute.value is empty %}
                        null
                    {% else %}
                        "{{ attribute.value.value }}"
                    {% endif %}
                {% if not loop.last %},{% endif %}
            {% endfor %}
            },
            "identities": [
                {% for email in profile.identityMap.email %}
                {
                    "type": "email",
                    "id": "{{ email.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}

                {# Add a comma only if we have both emails and external_ids. #}
                {% if profile.identityMap.email is not empty and profile.identityMap.external_id is not empty %}
                    ,
                {% endif %}

                {% for external in profile.identityMap.external_id %}
                {
                    "type": "external_id",
                    "id": "{{ external.id }}"
                }{% if not loop.last %},{% endif %}
                {% endfor %}
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                {% for segment in profile.segmentMembership.ups | added %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ],
                "remove": [
                {# Alternative syntax for filtering audiences by status: #}
                {% for segment in removedSegments(profile.segmentMembership.ups) %}
                    "{{ segment.key }}"{% if not loop.last %},{% endif %}
                {% endfor %}
                ]
            }
        }
    ]
}
```

**結果**

以下の `json` は、Adobe Experience Platform から書き出されたデータを表します。

```json
{
    "profiles": [
        {
            "attributes": {
                "firstName": "Hermione",
                "birthDate": null
            },
            "identities": [
                {
                    "type": "email",
                    "id": "johndoe@example.com"
                },
                {
                    "type": "email",
                    "id": "jd@example.com"
                },
                {
                    "type": "external_id",
                    "id": "123456"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075",
                    "788d8874-8007-4253-92b7-ee6b6c20c6f3"
                ],
                "remove": [
                    "8f812592-3f06-416b-bd50-e7831848a31a"
                ]
            }
        },
        {
            "attributes": {
                "firstName": "Harry",
                "birthDate": "1980/07/21"
            },
            "identities": [
                {
                    "type": "email",
                    "id": "harry.p@example.com"
                }
            ],
            "AdobeExperiencePlatformSegments": {
                "add": [
                    "36a51c13-9dd6-4d2c-8aa3-07d785ea5075"
                ],
                "remove": []
            }
        }
    ]
}
```

### 様々な条件でグループ化されて書き出されたプロファイルにアクセスするために、テンプレートに集計キーを含める {#template-aggregation-key}

宛先設定で[設定可能な集約](../../functionality/destination-configuration/aggregation-policy.md#configurable-aggregation)を使用する場合、条件（オーディエンス ID、オーディエンスエイリアス、オーディエンスメンバーシップ、ID 名前空間など）に基づいて、宛先に書き出されたプロファイルグループ化できます。

メッセージ変換テンプレートでは、以下の節の例に示すように、前述の集計キーにアクセスできます。宛先で想定される形式およびレート制限に合致するように、集計キーを使用して、Experience Platform から書き出された HTTP メッセージを構造化します。

#### テンプレートでのオーディエンス ID 集約キーの使用 {#aggregation-key-segment-id}

[設定可能な集約](../../functionality/destination-configuration/aggregation-policy.md#configurable-aggregation)を使用して `includeSegmentId` を true に設定すると、宛先に書き出された HTTP メッセージのプロファイルは、オーディエンス ID でグループ化されます。テンプレートのオーディエンス ID にアクセスできる方法については、以下を参照してください。

**入力**

以下の 4 つのプロファイルについて考えてみます。

* 最初の 2 つは、オーディエンス ID `788d8874-8007-4253-92b7-ee6b6c20c6f3` のオーディエンスの一部
* 3 番目のプロファイルは、オーディエンス ID `8f812592-3f06-416b-bd50-e7831848a31a` のオーディエンスの一部
* 4 番目のプロファイルは、上記の両方のオーディエンスの一部。

プロファイル 1：

```json
{
   "attributes":{
      "firstName":{
         "value":"Hermione"
      }
   },
   "segmentMembership":{
      "ups":{
         "788d8874-8007-4253-92b7-ee6b6c20c6f3":{
            "lastQualificationTime":"2020-11-20T13:15:49Z",
            "status":"realized"
         }
      }
   }
}
```

プロファイル 2：

```json
{
   "attributes":{
      "firstName":{
         "value":"Harry"
      }
   },
   "segmentMembership":{
      "ups":{
         "788d8874-8007-4253-92b7-ee6b6c20c6f3":{
            "lastQualificationTime":"2020-11-20T13:15:49Z",
            "status":"realized"
         }
      }
   }
}
```

プロファイル 3：

```json
{
   "attributes":{
      "firstName":{
         "value":"Tom"
      }
   },
   "segmentMembership":{
      "ups":{
         "8f812592-3f06-416b-bd50-e7831848a31a":{
            "lastQualificationTime":"2021-02-20T12:00:00Z",
            "status":"realized"
         }
      }
   }
}
```

プロファイル 4：

```json
{
   "attributes":{
      "firstName":{
         "value":"Jerry"
      }
   },
   "segmentMembership":{
      "ups":{
         "8f812592-3f06-416b-bd50-e7831848a31a":{
            "lastQualificationTime":"2021-02-20T12:00:00Z",
            "status":"realized"
         },
         "788d8874-8007-4253-92b7-ee6b6c20c6f3":{
            "lastQualificationTime":"2020-11-20T13:15:49Z",
            "status":"realized"
         }
      }
   }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートについて、[宛先サーバー設定](../../authoring-api/destination-server/create-destination-server.md)に[テンプレート](../../functionality/destination-server/templating-specs.md)を挿入する前に、無効な文字（二重引用符 `""` など）をエスケープする必要があります。二重引用符のエスケープについて詳しくは、[JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)の第 9 章を参照してください。

以下で、オーディエンス ID にアクセスするために、テンプレートでどのように `audienceId` が使用されているかに注意してください。この例では、宛先の分類のオーディエンスメンバーシップに `audienceId` を使用することを想定しています。独自の分類に応じて、代わりにその他のフィールド名を使用できます。

```python
{
    "audienceId": "{{ input.aggregationKey.segmentId }}",
    "profiles": [
        {% for profile in input.profiles %}
        {
            "first_name": "{{ profile.attributes.firstName.value }}"
        }{% if not loop.last %},{% endif %}
        {% endfor %}
    ]
}
```

**結果**

宛先に書き出されると、プロファイルは、そのオーディエンス ID に基づいて 2 つのグループに分割されます。

```json
{
   "audienceId":"788d8874-8007-4253-92b7-ee6b6c20c6f3",
   "profiles":[
      {
         "firstName":"Hermione"
      },
      {
         "firstName":"Harry"
      },
      {
         "firstName":"Jerry"
      }
   ]
}
```

```json
{
   "audienceId":"8f812592-3f06-416b-bd50-e7831848a31a",
   "profiles":[
      {
         "firstName":"Tom"
      },
      {
         "firstName":"Jerry"
      }
   ]
}
```

#### テンプレートでのオーディエンスエイリアス集約キーの使用 {#aggregation-key-segment-alias}

[設定可能な集約](../../functionality/destination-configuration/aggregation-policy.md#configurable-aggregation)を使用して `includeSegmentId` を true に設定すると、テンプレートのオーディエンスエイリアスにもアクセスできます。

テンプレートに以下の行を追加して、オーディエンスエイリアスでグループ化されて書き出されたプロファイルにアクセスします。

```python
customerList={{input.aggregationKey.segmentAlias}}
```

#### テンプレートでのオーディエンスステータス集約キーの使用 {#aggregation-key-segment-status}

[設定可能な集約](../../functionality/destination-configuration/aggregation-policy.md#configurable-aggregation)を使用して `includeSegmentId` および `includeSegmentStatus` を true に設定すると、テンプレートのオーディエンスステータスにアクセスできます。この方法で、プロファイルがセグメントに追加／セグメントから削除される必要があるかどうかに基づいて、宛先に書き出された HTTP メッセージのプロファイルをグループ化できます。

使用可能な値：

* realized
* existing
* exited

テンプレートに以下の行を追加して、上記の値に基づいて、プロファイルをセグメントに追加したりセグメントから削除したりします。

```python
action={% if input.aggregationKey.segmentStatus == "exited" %}REMOVE{% else %}ADD{% endif%}
```

#### テンプレートでの ID 名前空間集計キーの使用 {#aggregation-key-identity}

以下に、`"namespaces": ["email", "phone"]` および `"namespaces": ["GAID", "IDFA"]` の形式で、宛先設定の[設定可能な集計](../../functionality/destination-configuration/aggregation-policy.md#configurable-aggregation)が、書き出されたプロファイルを ID 名前空間で集計するように設定されている例を示します。グループ化について詳しくは、[宛先設定の作成](../../authoring-api/destination-configuration/create-destination-configuration.md)ドキュメントの `groups` パラメーターを参照してください。

**入力**

プロファイル 1：

```json
{
   "identityMap":{
      "email":[
         {
            "id":"e1@example.com"
         },
         {
            "id":"e2@example.com"
         }
      ],
      "phone":[
         {
            "id":"+40744111222"
         }
      ],
      "IDFA":[
         {
            "id":"AEBE52E7-03EE-455A-B3C4-E57283966239"
         }
      ],
      "GAID":[
         {
            "id":"e4fe9bde-caa0-47b6-908d-ffba3fa184f2"
         }
      ]
   }
}
```

プロファイル 2：

```json
{
   "identityMap":{
      "email":[
         {
            "id":"e3@example.com"
         }
      ],
      "phone":[
         {
            "id":"+40744333444"
         },
         {
            "id":"+40744555666"
         }
      ],
      "IDFA":[
         {
            "id":"134GHU45-34HH-GHJ7-K0H8-LHN665998NN0"
         }
      ],
      "GAID":[
         {
            "id":"47bh00i9-8jv6-334n-lll8-nb7f24sghg76"
         }
      ]
   }
}
```

**テンプレート**

>[!IMPORTANT]
>
>使用するすべてのテンプレートについて、[宛先サーバー設定](../../authoring-api/destination-server/create-destination-server.md)に[テンプレート](../../functionality/destination-server/templating-specs.md)を挿入する前に、無効な文字（二重引用符 `""` など）をエスケープする必要があります。二重引用符のエスケープについて詳しくは、[JSON 標準](https://www.ecma-international.org/publications-and-standards/standards/ecma-404/)の第 9 章を参照してください。

以下のテンプレートで `input.aggregationKey.identityNamespaces` が使用されていることに注意してください。

```python
{
            "profiles": [
            {% for profile in input.profiles %}
            {
                {% for ns in input.aggregationKey.identityNamespaces %}
                "{{ns}}": [
                    {% for id in profile.identityMap[ns] %}
                    "{{id.id}}"{% if not loop.last %},{% endif %}
                    {% endfor %}
                ]{% if not loop.last %},{% endif %}
                {% endfor %}
            }{% if not loop.last %},{% endif %}
            {% endfor %}
        ]
}
```

**結果**

宛先に書き出されると、プロファイルは、その ID 名前空間に基づいて 2 つのグループに分割されます。メールおよび電話は 1 つのグループであるのに対して、GAID および IDFA は別のグループです。

```json
{
   "profiles":[
      {
         "email":[
            "e1@example.com",
            "e2@example.com"
         ],
         "phone":[
            "+40744111222"
         ]
      },
      {
         "email":[
            "e3@example.com"
         ],
         "phone":[
            "+40744333444",
            "+40744555666"
         ]
      }
   ]
}
```

```json
{
   "profiles":[
      {
         "IDFA":[
            "AEBE52E7-03EE-455A-B3C4-E57283966239"
         ],
         "GAID":[
            "e4fe9bde-caa0-47b6-908d-ffba3fa184f2"
         ]
      },
      {
         "IDFA":[
            "134GHU45-34HH-GHJ7-K0H8-LHN665998NN0"
         ],
         "GAID":[
            "47bh00i9-8jv6-334n-lll8-nb7f24sghg76"
         ]
      }
   ]
}
```

#### URL テンプレートでの集計キーの使用 {#aggregation-key-url-template}

以下に示すように、ユースケースに応じて、ここで説明した集計キーを URL で使用することもできます。

```python
https://api.example.com/audience/{{input.aggregationKey.segmentId}}
```

### リファレンス：変換テンプレートで使用されるコンテキストと関数 {#reference}

テンプレートに提供されるコンテキストには、`input`（この呼び出しで書き出されるプロファイル／データ）および `destination`（アドビがデータを送信する宛先に関するデータで、すべてのプロファイルに対して有効）が含まれます。

以下の表で、上記の例の関数について説明します。

| 関数 | 説明 |
|---------|----------|
| `input.profile` | プロファイル（[JsonNode](https://fasterxml.github.io/jackson-databind/javadoc/2.11/com/fasterxml/jackson/databind/node/JsonNodeType.html) として表されます）。このページで前述したパートナー XDM スキーマに従います。 |
| `destination.segmentAliases` | Adobe Experience Platform 名前空間のオーディエンス ID からパートナーのシステムのオーディエンスエイリアスにマッピングします。 |
| `destination.segmentNames` | Adobe Experience Platform 名前空間のオーディエンス名からパートナーのシステムのオーディエンス名にマッピングします。 |
| `addedSegments(listOfSegments)` | ステータス `realized` を持つオーディエンスのみを返します。 |
| `removedSegments(listOfSegments)` | ステータス `exited` を持つオーディエンスのみを返します。 |

{style="table-layout:auto"}

## 次の手順 {#next-steps}

このドキュメントでは、Experience Platform から書き出されたデータがどのように変換されるかを確認しました。次に、宛先用のメッセージ変換テンプレートの作成について完全に理解するために、以下のページを参照してください。

* [メッセージ変換テンプレートの作成とテスト](../../testing-api/streaming-destinations/create-template.md)
* [テンプレートレンダリング API の操作](../../testing-api/streaming-destinations/render-template-api.md)
* [Destination SDK でサポートされる変換関数](../destination-server/supported-functions.md)

その他の宛先サーバーコンポーネントについて詳しくは、以下の記事を参照してください。

* [Destination SDK で作成される宛先のサーバー仕様](server-specs.md)
* [テンプレートの仕様](templating-specs.md)
* [ファイル形式設定](file-formatting.md)