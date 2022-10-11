# data-platform-sql-dbs-and-tables-bulk-setup

data-platform-sql-dbs-and-tables-bulk-setup は 指定したホストとポートのSQL に指定したデータベースおよびテーブルの作成、削除をまとめて処理するマイクロサービスです。


## 動作環境
- Python

## 動作手順
### ライブラリのインストール
仮想環境を作成し、必要なライブラリをインストールします。
```
python3 -m venv venv
. venv/bin/activate
pip install -r requirements.txt
```

### 環境変数、jsonファイルの設定
`my.conf` を作成し、SQL のユーザー、ユーザーパスワード、ホストを設定します。

`my-root.conf` を作成し、SQL のルートユーザー、ルートユーザーパスワード、ホストを設定します。

`.env` を作成し、SQL のユーザーを設定します。

`list.json` にポート番号、データベース名、sqlファイル名、テーブル名等を設定します。

### リポジトリのクローン
#### SQL を立ち上げ得るリポジトリとsqlファイルを含むリポジトリを使用する場合
以下のコマンドで、Kubernetes 上でSQL のPod を立ち上げるリポジトリとSQL のテーブルを作成を行うリポジトリのクローンをします。
```
python setup_sql.py clone
```

- Kubernetes 上でSQL のPod を立ち上げるリポジトリの例
  - [data-platfrom-authenticator-mysql-kube](https://github.com/latonaio/data-platform-authenticator-mysql-kube)

- SQL のテーブルを作成を行うリポジトリの例
  - [data-platform-authenticator-sql](https://github.com/latonaio/data-platform-authenticator-sql)


#### sqlファイルを手動で配置する場合
以下のフォルダ構成を`[podName]`、`[databaseName]`、`[sqlListName]` および`[sqlFileName]` は`list.json` と同じ名前を指定して作成します。

また、`list.json` の`port`、`tableName` および`indexNumber` も設定します。

`indexNumber` はテーブルを作成する順番を示します。

`list.json` の残りのキーは空白でも問題ないです。
```
resources
└── pods
    ├──[podName]
    │   └── databases
    │       └── [databaseName]
    │           └── tables
    │               └── [sqlListName]
    │                   └── [sqlFileName]
    └──[podName]
        └── ...
```

### DBとテーブルの登録準備
#### list.jsonへの追加
一括登録したいデータベース、テーブルを、以下のようにlist.jsonに配置します。

```
[
    {
          "podName": "data-platform-masters-and-transactions-mysql-kube",
          "podRepositoryUrl": "https://github.com/latonaio/data-platform-masters-and-transactions-mysql-kube.git",
          "branch": "master",
          "port": "30010",
          "databases": [
              {
                  "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                  "tables": [
                      {
                          "sqlListName": "data-platform-plant-sql",
                          "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-plant-sql.git",
                          "branch": "master",
                          "sqlFiles": [
                              {
                                  "sqlFileName": "data_platform_plant_sql_address_data.sql",
                                  "tableName": "data_platform_plant_address_data",
                                  "indexNumber": 2
                              },
                              {
                                  "sqlFileName": "data_platform_plant_sql_general_data.sql",
                                  "tableName": "data_platform_plant_general_data",
                                  "indexNumber": 1
                              }
                          ]
                      }
                  ]
              },
              {
                  "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                  "tables": [
                      {
                          "sqlListName": "data-platform-business-partner-sql",
                          "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-business-partner-sql.git",
                          "branch": "master",
                          "sqlFiles": [
                              {
                                  "sqlFileName": "data-platform-business-partner-customer-accounting-data.sql",
                                  "tableName": "data_platform_business_partner_customer_accounting_dat",
                                  "indexNumber": 2
                              },
                              {
                                "sqlFileName": "data-platform-business-partner-sql-accounting-data.sql",
                                "tableName": "data_platform_business_partner_accounting_data",
                                "indexNumber": 3
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-address-data.sql",
                                  "tableName": "data_platform_business_partner_address_data",
                                  "indexNumber": 4
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-bank-data.sql",
                                  "tableName": "data_platform_business_partner_bank_data",
                                  "indexNumber": 5
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-customer-data.sql",
                                  "tableName": "data_platform_business_partner_customer_data",
                                  "indexNumber": 6
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-customer-partner-function-data.sql",
                                  "tableName": "data_platform_business_partner_customer_partner_function_data",
                                  "indexNumber": 7
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-customer-partner-plant-data.sql",
                                  "tableName": "data_platform_business_partner_customer_partner_plant_data",
                                  "indexNumber": 8
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-customer-sales-area-data.sql",
                                  "tableName": "data_platform_business_partner_customer_sales_area_data",
                                  "indexNumber": 9
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-general-data.sql",
                                  "tableName": "data_platform_business_partner_general_data",
                                  "indexNumber": 1
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-relationship-data.sql",
                                  "tableName": "data_platform_business_partner_relationship_data",
                                  "indexNumber": 10
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-role-data.sql",
                                  "tableName": "data_platform_business_partner_role_data",
                                  "indexNumber": 11
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-supplier-accounting-data.sql",
                                  "tableName": "data_platform_business_partner_supplier_accounting_data",
                                  "indexNumber": 12
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-supplier-data.sql",
                                  "tableName": "data_platform_business_partner_supplier_data",
                                  "indexNumber": 13
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-supplier-partner-function-data.sql",
                                  "tableName": "data_platform_business_partner_supplier_partner_function_data",
                                  "indexNumber": 14
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-supplier-partner-plant-data.sql",
                                  "tableName": "data_platform_business_partner_supplier_partner_plant_data",
                                  "indexNumber": 15
                              },
                              {
                                  "sqlFileName": "data-platform-business-partner-sql-supplier-purchasing-organization-data.sql",
                                  "tableName": "data_platform_business_partner_supplier_purchasing_organization_data",
                                  "indexNumber": 16
                              }
                          ]
                      }
                  ]
              },
              {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-price-master-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-price-master-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_price_master_sql_price_master_data.sql",
                                "tableName": "data_platform_price_master_price_master_data",
                                "indexNumber": 1
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-bank-master-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-bank-master-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_bank_master_sql_bank_data.sql",
                                "tableName": "data_platform_bank_master_bank_data",
                                "indexNumber": 1
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-supply-chain-relationship-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-supply-chain-relationship-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_supply_chain_realtionship_sql_header_data",
                                "tableName": "data_platform_supply_chain_realtionship_header_data",
                                "indexNumber": 1
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-general-ledger-account-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-general-ledger-account-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_general_ledger_account_sql_chart_of_accounts_data",
                                "tableName": "data_platform_general_ledger_account_chart_of_accounts_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_general_ledger_account_sql_text_data",
                                "tableName": "data_platform_general_ledger_account_text_data",
                                "indexNumber": 2
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-address-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-address-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_address_sql_address_data",
                                "tableName": "data_platform_address_address_data",
                                "indexNumber": 1
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-distribution-channel-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-distribution-channel-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_distribution_channel_sql_distribution_channel_data",
                                "tableName": "data_platform_distribution_channel_distribution_channel_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_distribution_channel_sql_text_data",
                                "tableName": "data_platform_distribution_channel_text_data",
                                "indexNumber": 2
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-industry-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-industry-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_industry_sql_industry_data",
                                "tableName": "data_platform_industry_industry_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_industry_sql_text_data",
                                "tableName": "data_platform_industry_text_data",
                                "indexNumber": 2
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-currency-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-currency-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_currency_sql_currency_data",
                                "tableName": "data_platform_currency_currency_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_currency_sql_currency_text_data",
                                "tableName": "data_platform_currency_currency_text_data",
                                "indexNumber": 2
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-product-master-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-product-master-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_product_master_sql_accounting_data",
                                "tableName": "data_platform_product_master_accounting_data",
                                "indexNumber": 2
                            },
                            {
                                "sqlFileName": "data_platform_product_master_sql_bp_plant_data",
                                "tableName": "data_platform_product_master_bp_plant_data",
                                "indexNumber": 3
                            },
                            {
                                "sqlFileName": "data_platform_product_master_sql_business_partner_data",
                                "tableName": "data_platform_product_master_business_partner_data",
                                "indexNumber": 4
                            },
                            {
                                "sqlFileName": "data_platform_product_master_sql_general_data",
                                "tableName": "data_platform_product_master_general_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_product_master_sql_mrp_area_data",
                                "tableName": "data_platform_product_master_mrp_area_data",
                                "indexNumber": 5
                            },
                            {
                                "sqlFileName": "data_platform_product_master_sql_procurement_data",
                                "tableName": "data_platform_product_master_procurement_data",
                                "indexNumber": 6
                            },
                            {
                                "sqlFileName": "data_platform_product_master_sql_product_description_data",
                                "tableName": "data_platform_product_master_product_description_data",
                                "indexNumber": 7
                            },
                            {
                                "sqlFileName": "data_platform_product_master_sql_sales_delivery_data",
                                "tableName": "data_platform_product_master_sales_delivery_data",
                                "indexNumber": 8
                            },
                            {
                                "sqlFileName": "data_platform_product_master_sql_sales_tax_data",
                                "tableName": "data_platform_product_master_sales_tax_data",
                                "indexNumber": 9
                            },
                            {
                                "sqlFileName": "data_platform_product_master_sql_storage_location_data",
                                "tableName": "data_platform_product_master_storage_location_data",
                                "indexNumber": 10
                            },
                            {
                                "sqlFileName": "data_platform_product_master_sql_work_scheduling_data",
                                "tableName": "data_platform_product_master_work_scheduling_data",
                                "indexNumber": 11
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-quantity-unit-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-quantity-unit-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_quantity_unit_sql_quantity_unit_data",
                                "tableName": "data_platform_quantity_unit_quantity_unit_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_quantity_unit_sql_text_data",
                                "tableName": "data_platform_quantity_unit_text_data",
                                "indexNumber": 2
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-product-stock-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-product-stock-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_product_stock_sql_product_stock_data",
                                "tableName": "data_platform_product_stock_product_stock_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_product_stock_sql_product_stock_availability_data",
                                "tableName": "data_platform_product_stock_product_stock_availability_data",
                                "indexNumber": 2
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-reservation-document-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-reservation-document-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_reservation_document_sql_header_data",
                                "tableName": "data_platform_reservation_document_header_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_reservation_document_sql_item_data",
                                "tableName": "data_platform_reservation_document_item_data",
                                "indexNumber": 2
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-goods-movement-document-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-goods-movement-document-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_goods_movement_document_sql_header_data",
                                "tableName": "data_platform_goods_movement_document_header_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_goods_movement_document_sql_item_data",
                                "tableName": "data_platform_goods_movement_document_item_data",
                                "indexNumber": 2
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-orders-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-orders-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_orders_sql_header_data",
                                "tableName": "data_platform_orders_header_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_orders_sql_header_partner_data",
                                "tableName": "data_platform_orders_header_partner_data",
                                "indexNumber": 2
                            },
                            {
                                "sqlFileName": "data_platform_orders_sql_header_partner_plant_data",
                                "tableName": "data_platform_orders_header_partner_plant_data",
                                "indexNumber": 3
                            },
                            {
                                "sqlFileName": "data_platform_orders_sql_item_data",
                                "tableName": "data_platform_orders_item_data",
                                "indexNumber": 4
                            },
                            {
                                "sqlFileName": "data_platform_orders_sql_item_partner_data",
                                "tableName": "data_platform_orders_item_partner_data",
                                "indexNumber": 5
                            },
                            {
                                "sqlFileName": "data_platform_orders_sql_item_pricing_element_data",
                                "tableName": "data_platform_orders_item_pricing_element_data",
                                "indexNumber": 6
                            },
                            {
                                "sqlFileName": "data_platform_orders_sql_item_schedule_line_data",
                                "tableName": "data_platform_orders_item_schedule_line_data",
                                "indexNumber": 7
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-delivery-document-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-delivery-document-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_delivery_document_sql_header_data",
                                "tableName": "data_platform_delivery_document_header_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_delivery_document_sql_header_partner_data",
                                "tableName": "data_platform_delivery_document_header_partner_data",
                                "indexNumber": 2
                            },
                            {
                                "sqlFileName": "data_platform_delivery_document_sql_item_data",
                                "tableName": "data_platform_delivery_document_item_data",
                                "indexNumber": 3
                            },
                            {
                                "sqlFileName": "data_platform_delivery_document_sql_partner_address_data",
                                "tableName": "data_platform_delivery_document_partner_address_data",
                                "indexNumber": 4
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-customer-product-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-customer-product-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_customer_product_sql_customer_product_data",
                                "tableName": "data_platform_customer_product_customer_product_data",
                                "indexNumber": 1
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-incoterms-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-incoterms-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_incoterms_sql_incoterms_classification_data",
                                "tableName": "data_platform_incoterms_incoterms_classification_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_incoterms_sql_incoterms_classification_text_data",
                                "tableName": "data_platform_incoterms_incoterms_classification_text_data",
                                "indexNumber": 2
                            },
                            {
                                "sqlFileName": "sap_incoterms_sql_incoterms_version_data",
                                "tableName": "sap_incoterms_incoterms_version_data",
                                "indexNumber": 3
                            },
                            {
                                "sqlFileName": "data_platform_incoterms_sql_version_text_data",
                                "tableName": "data_platform_incoterms_version_text_data",
                                "indexNumber": 4
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-purchase-requisition-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-purchase-requisition-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_purchase_requisition_sql_header_data",
                                "tableName": "data_platform_purchase_requisition_header_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_purchase_requisition_sql_item_data",
                                "tableName": "data_platform_purchase_requisition_item_data",
                                "indexNumber": 2
                            },
                            {
                                "sqlFileName": "data_platform_purchase_requisition_sql_item_delivery_address_data",
                                "tableName": "data_platform_purchase_requisition_item_delivery_address_data",
                                "indexNumber": 3
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-purchase-requisition-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-purchase-requisition-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_purchase_requisition_sql_header_data",
                                "tableName": "data_platform_purchase_requisition_header_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_purchase_requisition_sql_item_data",
                                "tableName": "data_platform_purchase_requisition_item_data",
                                "indexNumber": 2
                            },
                            {
                                "sqlFileName": "data_platform_purchase_requisition_sql_item_delivery_address_data",
                                "tableName": "data_platform_purchase_requisition_item_delivery_address_data",
                                "indexNumber": 3
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-payment-method-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-payment-method-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_payment_method_sql_payment_method_data",
                                "tableName": "data_platform_payment_method_payment_method_data",
                                "indexNumber": 1
                            },
                            {
                                "sqlFileName": "data_platform_payment_method_sql_text_data",
                                "tableName": "data_platform_payment_method_text_data",
                                "indexNumber": 2
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-payment-requisition-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-payment-requisition-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_payment_requisition_sql_payment_requisition_data",
                                "tableName": "data_platform_payment_requisition_payment_requisition_data",
                                "indexNumber": 1
                            }
                        ]
                    }
                ]
            },
            {
                "databaseName": "DataPlatformMastersAndTransactionsMysqlKube",
                "tables": [
                    {
                        "sqlListName": "data-platform-payment-requisition-sql",
                        "sqlListRepositoryUrl": "https://github.com/latonaio/data-platform-payment-requisition-sql.git",
                        "branch": "master",
                        "sqlFiles": [
                            {
                                "sqlFileName": "data_platform_payment_requisition_sql_payment_requisition_data",
                                "tableName": "data_platform_payment_requisition_payment_requisition_data",
                                "indexNumber": 1
                            }
                        ]
                    }
                ]
            }
            
          ]
    }
  
]
```

### データベースとテーブルの作成の実行
#### ユーザーにデータベースとテーブルの作成権限がある場合
以下のコマンドで、指定したデータベースとテーブルを作成します。
```
python setup_sql.py create
```

#### ユーザーにデータベースとテーブルの作成権限がない場合
以下のコマンドで、指定したユーザーに権限の付与をしてから、指定したデータベースとテーブルを作成します。
```
python setup_sql.py create --perms
```

### 権限の付与
以下のコマンドで、指定したユーザーに権限を付与します。
```
python setup_sql.py grant
```

### データベースとテーブルの削除
以下のコマンドで、指定したデータベースとテーブルを削除します。
```
python setup_sql.py delete
```