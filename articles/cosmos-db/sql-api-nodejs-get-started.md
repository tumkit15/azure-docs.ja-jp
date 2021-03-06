---
title: Azure Cosmos DB 用 SQL API の Node.js チュートリアル
description: SQL API を使用して Azure Cosmos DB に接続して照会する方法を示す Node.js チュートリアル
services: cosmos-db
author: deborahc
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: dech
ms.openlocfilehash: 8d2272ca468f7c415d9d03506ecaa3fbe667f0c0
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2018
ms.locfileid: "52873174"
---
# <a name="tutorial-build-a-nodejs-console-app-with-javascript-sdk-to-manage-azure-cosmos-db-sql-api-data"></a>チュートリアル: JavaScript SDK を使用して、Azure Cosmos DB SQL API データを管理するための Node.js コンソール アプリを構築する

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET (プレビュー)](sql-api-dotnet-get-started-preview.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET Core (プレビュー)](sql-api-dotnet-core-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

このチュートリアルでは、Azure Cosmos DB リソースを作成してそれらに対するクエリを実行する Node.js コンソール アプリケーションを構築する方法について説明します。

このチュートリアルに含まれるタスクは次のとおりです。

> [!div class="checklist"]
> * Azure Cosmos DB アカウントを作成し、アカウントに接続する
> * アプリケーションをセットアップする
> * データベースを作成する
> * コンテナーを作成する
> * コンテナーに項目を追加する
> * 項目、コンテナー、およびデータベースに対する CRUD 操作を実行する

アプリケーションを作成する時間がありませんか。 心配はありません。 [GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started ) で完全なソリューションを入手できます。 簡単な手順については、この記事の[完全なソリューションの入手](#GetSolution)に関するセクションを参照してください。

## <a name="prerequisites"></a>前提条件 

以下のリソースがそろっていることを確認してください。

* アクティブな Azure アカウントアカウントがない場合、Azure 試用版にサインアップして、最大 10 件の無料 Mobile Apps を入手できます。 これがない場合は、 [Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/)にサインアップできます。 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Node.js](https://nodejs.org/) バージョン v6.0.0 以降。

## <a name="step-1-create-an-azure-cosmos-db-account"></a>手順 1: Azure Cosmos DB アカウントを作成する

それでは、Azure Cosmos DB アカウントを作成してみましょう。 使用するアカウントが既にある場合は、「[Node.js アプリケーションをセットアップする](#SetupNode)」に進んでかまいません。 Azure Cosmos DB Emulator を使用する場合は、[Azure Cosmos DB Emulator](local-emulator.md) に関する記事に記載されている手順に従ってエミュレーターをセットアップし、「[Node.js アプリケーションをセットアップする](#SetupNode)」に進んでください。 

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="SetupNode"></a>手順 2: Node.js アプリケーションをセットアップする

1. お使いのターミナルを開きます。
2. Node.js アプリケーションを保存するフォルダーまたはディレクトリを見つけます。
3. 次のコマンドを使用して、2 つの空の JavaScript ファイルを作成します。

   * Windows:
     * ```fsutil file createnew app.js 0```
     * ```fsutil file createnew config.js 0```

   * Linux/OS X:
     * ```touch app.js```
     * ```touch config.js```

4. npm で @azure/cosmos モジュールをインストールします。 次のコマンドを使用します。
   * ```npm install @azure/cosmos --save```

## <a id="Config"></a>手順 3: アプリの構成を設定する

1. 普段使用しているテキスト エディターで ```config.js``` を開きます。

1. 以下のコード スニペットをコピーして貼り付け、プロパティ ```config.endpoint``` と ```config.primaryKey``` を Azure Cosmos DB エンドポイント URI と主キーに設定します。 これらの構成はどちらも [Azure ポータル](https://portal.azure.com)にあります。

   ![Azure portal からキーを取得するスクリーンショット][keys]

   ```nodejs
   // ADD THIS PART TO YOUR CODE
   var config = {}

   config.endpoint = "~your Azure Cosmos DB endpoint uri here~";
   config.primaryKey = "~your primary key here~";
   ``` 

1. ```database```、```container```、および ```items``` データをコピーして ```config``` オブジェクトに貼り付けます。```config.endpoint``` プロパティと ```config.primaryKey``` プロパティの設定に続けて追加してください。 データベースに保存するデータが既にある場合は、データをここに定義する代わりに、Azure Cosmos DB の[データ移行ツール](import-data.md)を使用できます。

   ```nodejs
   var config = {}

   config.endpoint = "~your Azure Cosmos DB account endpoint uri here~";
   config.primaryKey = "~your primary key here~";

   config.database = {
      "id": "FamilyDatabase"
   };

   config.container = {
     "id": "FamilyContainer"
   };

   config.items = {
      "Andersen": {
          "id": "Anderson.1",
          "lastName": "Andersen",
          "parents": [{
            "firstName": "Thomas"
        }, {
                "firstName": "Mary Kay"
            }],
        "children": [{
            "firstName": "Henriette Thaulow",
            "gender": "female",
            "grade": 5,
            "pets": [{
                "givenName": "Fluffy"
            }]
        }],
        "address": {
            "state": "WA",
            "county": "King",
            "city": "Seattle"
        }
    },
    "Wakefield": {
        "id": "Wakefield.7",
        "parents": [{
            "familyName": "Wakefield",
            "firstName": "Robin"
        }, {
                "familyName": "Miller",
                "firstName": "Ben"
            }],
        "children": [{
            "familyName": "Merriam",
            "firstName": "Jesse",
            "gender": "female",
            "grade": 8,
            "pets": [{
                "givenName": "Goofy"
            }, {
                    "givenName": "Shadow"
                }]
        }, {
                "familyName": "Miller",
                "firstName": "Lisa",
                "gender": "female",
                "grade": 1
            }],
        "address": {
            "state": "NY",
            "county": "Manhattan",
            "city": "NY"
        },
        "isRegistered": false
      }
   };
   ```

   以前のバージョンの JavaScript SDK に慣れている方は、"コレクション" や "ドキュメント" といった用語をよく目にしたかと思います。 Azure Cosmos DB は[複数の API モデル](https://docs.microsoft.com/azure/cosmos-db/introduction#key-capabilities)をサポートしているため、バージョン 2.0 以上の JavaScript SDK では、"コンテナー" と "項目" という一般的な用語が使用されます。 コンテナーは、コレクション、グラフ、またはテーブルを表します。 項目は、ドキュメント、エッジ/頂点、行など、コンテナー内の内容を表します。 

1. 最後に、```app.js``` ファイル内で参照できるように、```config``` オブジェクトをエクスポートします。

   ```nodejs
        },
        "isRegistered": false
       }
   };

   // ADD THIS PART TO YOUR CODE
   module.exports = config;
   ```

## <a id="Connect"></a>手順 4: Azure Cosmos DB アカウントに接続する

1. テキスト エディターで、空の ```app.js``` ファイルを開きます。 ```@azure/cosmos``` モジュールと新たに作成した ```config``` モジュールをインポートするために、以下のコードをコピーして貼り付けます。

   ```nodejs
   // ADD THIS PART TO YOUR CODE
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');
   const url = require('url');
   ```

1. 先ほど保存した ```config.endpoint``` と ```config.primaryKey``` を使用して新しい CosmosClient を作成するために、以下のコードをコピーして貼り付けます。

   ```nodejs
   const url = require('url');

   // ADD THIS PART TO YOUR CODE
   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });
   ```

Azure Cosmos DB クライアントを初期化するためのコードは以上で完成です。続いて、Azure Cosmos DB リソースの使用方法について説明します。

## <a name="step-5-create-a-database"></a>手順 5: データベースを作成する

1. データベース ID およびコンテナー ID を設定するために、以下のコードをコピーして貼り付けます。Azure Cosmos DB クライアントは、これらの ID を通じて適切なデータベースとコンテナーを検出します。

   ```nodejs
   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   // ADD THIS PART TO YOUR CODE
   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;
   ```

   データベースは、**Databases** クラスの [createIfNotExists](/javascript/api/%40azure/cosmos/databases) または [create](/javascript/api/%40azure/cosmos/databases) 関数を使用して作成できます。 データベースは、コンテナーに分割された項目の論理上のコンテナーです。 

2. app.js ファイルの ```databaseId``` と ```containerId``` の定義の下に、**createDatabase** メソッドと **readDatabase** メソッドをコピーして貼り付けます。 **createDatabase** 関数は、```config``` オブジェクトから指定された ID が ```FamilyDatabase``` の新しいデータベースを作成します (該当するデータベースが存在しない場合)。 **readDatabase** 関数は、データベースの定義を読み取り、データベースが存在することを確認します。

   ```nodejs
   /**
    * Create the database if it does not exist
    */
   async function createDatabase() {
       const { database } = await client.databases.createIfNotExists({ id: databaseId });
       console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
      const { body: databaseDefinition } = await client.database(databaseId).read();
      console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }
   ```

3. 終了メッセージを印刷するヘルパー関数 **exit** を追加するために、**createDatabase** 関数と **readDatabase** 関数を設定したコードの下に以下のコードをコピーして貼り付けます。 

   ```nodejs
   // ADD THIS PART TO YOUR CODE
   function exit(message) {
      console.log(message);
      console.log('Press any key to exit');
      process.stdin.setRawMode(true);
      process.stdin.resume();
      process.stdin.on('data', process.exit.bind(process, 0));
   };
   ```

4. **createDatabase** 関数と **readDatabase** 関数を呼び出すために、**exit** 関数を設定したコードの下に以下のコードをコピーして貼り付けます。

   ```nodejs
   createDatabase()
     .then(() => readDatabase())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error \${JSON.stringify(error)}`) });
   ```

   この時点で、```app.js``` のコードは次のようになります。

   ```nodejs
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');
   const url = require('url');

   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;

    /**
    * Create the database if it does not exist
    */
    async function createDatabase() {
     const { database } = await client.databases.createIfNotExists({ id: databaseId });
     console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
     const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }

   /**
   * Exit the app with a prompt
   * @param {message} message - The message to display
   */
   function exit(message) {
     console.log(message);
     console.log('Press any key to exit');
     process.stdin.setRawMode(true);
     process.stdin.resume();
     process.stdin.on('data', process.exit.bind(process, 0));
   }

   createDatabase()
     .then(() => readDatabase())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error) }`) });
   ```

5. ターミナルで、```app.js``` ファイルを見つけ、コマンドを実行します。 

   ```bash 
   node app.js
   ```

お疲れさまでした。 これで、Azure Cosmos DB データベースが作成されました。

## <a id="CreateContainer"></a>手順 6: コンテナーを作成する

> [!WARNING]
> **createContainer** 関数を呼び出すと、新しいコンテナーが作成されます。これは、価格に関連します。 詳細については、[価格のページ](https://azure.microsoft.com/pricing/details/cosmos-db/)を参照してください。

コンテナーは、**Containers** クラスの [createIfNotExists](/javascript/api/%40azure/cosmos/containers) または [create](/javascript/api/%40azure/cosmos/containers) 関数を使用して作成できます。  コンテナーは、項目 (SQL API の場合は JSON ドキュメント) と、それに関連する JavaScript アプリケーション ロジックで構成されます。

1. app.js ファイルの **readDatabase** 関数の下に、**createContainer** 関数と **readContainer** 関数をコピーして貼り付けます。 **createContainer** 関数は、```config``` オブジェクトから指定された ```containerId``` を持つ新しいコンテナーを作成します (該当するコンテナーが存在しない場合)。 **readContainer** 関数は、コンテナー定義を読み取り、コンテナーが存在することを確認します。

   ```nodejs
   /**
   * Create the container if it does not exist
   */
   async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
   }

   /**
   * Read the container definition
   */
   async function readContainer() {
     const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
     console.log(`Reading container:\n${containerDefinition.id}\n`);
   }
   ```

1. **createContainer** 関数と **readContainer** 関数を実行するために、**readDatabase** の呼び出しの下に以下のコードをコピーして貼り付けます。

   ```nodejs
   createDatabase()
     .then(() => readDatabase())

     // ADD THIS PART TO YOUR CODE
     .then(() => createContainer())
     .then(() => readContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

   この時点で、```app.js``` のコードは次のようになります。

   ```nodejs
   const CosmosClient = require('@azure/cosmos').CosmosClient;

   const config = require('./config');
   const url = require('url');

   const endpoint = config.endpoint;
   const masterKey = config.primaryKey;

   const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

   const HttpStatusCodes = { NOTFOUND: 404 };

   const databaseId = config.database.id;
   const containerId = config.container.id;

   /**
   * Create the database if it does not exist
   */
   async function createDatabase() {
     const { database } = await client.databases.createIfNotExists({ id: databaseId });
     console.log(`Created database:\n${database.id}\n`);
   }

   /**
   * Read the database definition
   */
   async function readDatabase() {
     const { body: databaseDefinition } = await client.database(databaseId).read();
     console.log(`Reading database:\n${databaseDefinition.id}\n`);
   }

   /**
   * Create the container if it does not exist
   */
   async function createContainer() {
     const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
     console.log(`Created container:\n${config.container.id}\n`);
   }

   /**
   * Read the container definition
   */
   async function readContainer() {
     const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
     console.log(`Reading container:\n${containerDefinition.id}\n`);
   }

   /**
   * Exit the app with a prompt
   * @param {message} message - The message to display
   */
   function exit(message) {
     console.log(message);
     console.log('Press any key to exit');
     process.stdin.setRawMode(true);
     process.stdin.resume();
     process.stdin.on('data', process.exit.bind(process, 0));
   }

   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. ターミナルで、```app.js``` ファイルを見つけ、コマンドを実行します。 

   ```bash 
   node app.js
   ```

お疲れさまでした。 これで、Azure Cosmos DB コンテナーが作成されました。

## <a id="CreateItem"></a>手順 7: 項目を作成する

項目は、**Items** クラスの [create](/javascript/api/%40azure/cosmos/items) 関数を使用して作成できます。 SQL API を使用すると、項目はドキュメントとして投影されます。これは、ユーザー定義の (任意の) JSON コンテンツです。 これで、Azure Cosmos DB に項目を挿入できます。

1. **readContainer** 関数の下に、**createFamilyItem** 関数をコピーして貼り付けます。 **createFamilyItem** 関数は、```config``` オブジェクトに保存された JSON データを含む項目を作成します。 項目を作成する前に、同じ ID を持つ項目が存在していないことを確認します。

   ```nodejs
   /**
   * Create family item if it does not exist
   */
  async function createFamilyItem(itemBody) {
     try {
         // read the item to see if it exists
         const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).read();
         console.log(`Item with family id ${itemBody.id} already exists\n`);
     }
     catch (error) {
        // create the family item if it does not exist
        if (error.code === HttpStatusCodes.NOTFOUND) {
            const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
            console.log(`Created family item with id:\n${itemBody.id}\n`);
        } else {
            throw error;
        }
     }
   };
   ```

1. **createFamilyItem** 関数を実行するために、**readContainer** の呼び出しの下に以下のコードをコピーして貼り付けます。

   ```nodejs
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())

     // ADD THIS PART TO YOUR CODE
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. ターミナルで、```app.js``` ファイルを見つけ、コマンドを実行します。 

   ```bash 
   node app.js
   ```

お疲れさまでした。 これで、Azure Cosmos DB 項目が作成されました。


## <a id="Query"></a>手順 8: Azure Cosmos DB リソースにクエリを実行する
Azure Cosmos DB では、各コンテナーに格納された JSON ドキュメントに対する[豊富なクエリ](how-to-sql-query.md)がサポートされています。 次のサンプル コードは、コンテナー内のドキュメントに対して実行できるクエリを示しています。

1. app.js ファイルの **createFamilyItem** 関数の下に、**queryContainer** 関数をコピーして貼り付けます。 以下のとおり、Azure Cosmos DB は SQL に似たクエリをサポートしています。 複雑なクエリを構築する方法の詳細については、[Query Playground](https://www.documentdb.com/sql/demo) および[クエリに関するドキュメント](how-to-sql-query.md)を参照してください。

   ```nodejs
   /**
   * Query the container using SQL
    */
   async function queryContainer() {
     console.log(`Querying container:\n${config.container.id}`);

     // query to return all children in a family
     const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
   };
   ```

1. **createFamilyItem** の呼び出しの下に、**queryContainer** 関数を実行するためのコードをコピーして貼り付けます。

   ```nodejs
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))

     // ADD THIS PART TO YOUR CODE
     .then(() => queryContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. ターミナルで、```app.js``` ファイルを見つけ、コマンドを実行します。

   ```bash 
   node app.js
   ```

お疲れさまでした。 これで、Azure Cosmos DB 項目に対するクエリが実行されました。

## <a id="ReplaceItem"></a>手順 9: 項目を置き換える
Azure Cosmos DB は、項目の内容の置換をサポートします。

1. app.js ファイルの **queryContainer** 関数の下に、**replaceFamilyItem** 関数をコピーして貼り付けます。 子のプロパティ "grade" が以前の 5 から 6 に変更されていることに注意してください。

   ```nodejs
   // ADD THIS PART TO YOUR CODE
   /**
   * Replace the item by ID.
   */
   async function replaceFamilyItem(itemBody) {
      console.log(`Replacing item:\n${itemBody.id}\n`);
      // Change property 'grade'
      itemBody.children[0].grade = 6;
      const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).replace(itemBody);
   };
   ```

1. **replaceFamilyItem** 関数を実行するために、**queryContainer** の呼び出しの下に以下のコードをコピーして貼り付けます。 さらに、もう一度 **queryContainer** を呼び出して項目が正常に変更されたことを確認するためのコードを追加します。

   ```nodejs
   createDatabase()
     .then(() => readDatabase())
     .then(() => createContainer())
     .then(() => readContainer())
     .then(() => createFamilyItem(config.items.Andersen))
     .then(() => createFamilyItem(config.items.Wakefield))
     .then(() => queryContainer())

     // ADD THIS PART TO YOUR CODE
     .then(() => replaceFamilyItem(config.items.Andersen))
     .then(() => queryContainer())
     // ENDS HERE

     .then(() => { exit(`Completed successfully`); })
     .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. ターミナルで、```app.js``` ファイルを見つけ、コマンドを実行します。

   ```bash 
   node app.js
   ```

お疲れさまでした。 これで、Azure Cosmos DB 項目が置換されました。

## <a id="DeleteItem"></a>手順 10: 項目を削除する

Azure Cosmos DB は、JSON 項目の削除をサポートします。

1. **replaceFamilyItem** 関数の下に、**deleteFamilyItem** 関数をコピーして貼り付けます。

   ```nodejs
  /**
  * Delete the item by ID.
  */
   async function deleteFamilyItem(itemBody) {
      await client.database(databaseId).container(containerId).item(itemBody.id).delete(itemBody);
      console.log(`Deleted item:\n${itemBody.id}\n`);
   };
   ```

1. **deleteFamilyItem** 関数を実行するために、2 つ目の **queryContainer** の呼び出しの下に以下のコードをコピーして貼り付けます。

   ```nodejs
   createDatabase()
      .then(() => readDatabase())
      .then(() => createContainer())
      .then(() => readContainer())
      .then(() => createFamilyItem(config.items.Andersen))
      .then(() => createFamilyItem(config.items.Wakefield))
      .then(() => queryContainer
      ())
      .then(() => replaceFamilyItem(config.items.Andersen))
      .then(() => queryContainer())

    // ADD THIS PART TO YOUR CODE
      .then(() => deleteFamilyItem(config.items.Andersen))
    // ENDS HERE

    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

1. ターミナルで、```app.js``` ファイルを見つけ、コマンドを実行します。 

   ```bash 
   node app.js
   ```

お疲れさまでした。 これで、Azure Cosmos DB 項目が削除されました。

## <a id="DeleteDatabase"></a>手順 11: データベースを削除する

作成したデータベースを削除すると、データベースとすべての子リソース (コンテナー、項目など) が削除されます。

1. データベースとそのすべての子リソースを削除するために、**deleteFamilyItem** 関数の下に **cleanup** 関数をコピーして貼り付けます。

   ```nodejs
   /**
   * Cleanup the database and container on completion
   */
   async function cleanup() {
     await client.database(databaseId).delete();
   }
   ```

1. **cleanup** 関数を実行するために、**deleteFamilyItem** の呼び出しの下に以下のコードをコピーして貼り付けます。

   ```nodejs
   createDatabase()
      .then(() => readDatabase())
      .then(() => createContainer())
      .then(() => readContainer())
      .then(() => createFamilyItem(config.items.Andersen))
      .then(() => createFamilyItem(config.items.Wakefield))
      .then(() => queryContainer())
      .then(() => replaceFamilyItem(config.items.Andersen))
      .then(() => queryContainer())
      .then(() => deleteFamilyItem(config.items.Andersen))

      // ADD THIS PART TO YOUR CODE
      .then(() => cleanup())
      // ENDS HERE

      .then(() => { exit(`Completed successfully`); })
      .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
   ```

## <a id="Run"></a>手順 12: Node.js アプリケーションの全体的な実行の流れ

全体的なコードは次のようになります。

```nodejs
const CosmosClient = require('@azure/cosmos').CosmosClient;

const config = require('./config');
const url = require('url');

const endpoint = config.endpoint;
const masterKey = config.primaryKey;

const HttpStatusCodes = { NOTFOUND: 404 };

const databaseId = config.database.id;
const containerId = config.container.id;

const client = new CosmosClient({ endpoint: endpoint, auth: { masterKey: masterKey } });

/**
 * Create the database if it does not exist
 */
async function createDatabase() {
    const { database } = await client.databases.createIfNotExists({ id: databaseId });
    console.log(`Created database:\n${database.id}\n`);
}

/**
 * Read the database definition
 */
async function readDatabase() {
    const { body: databaseDefinition } = await client.database(databaseId).read();
    console.log(`Reading database:\n${databaseDefinition.id}\n`);
}

/**
 * Create the container if it does not exist
 */
async function createContainer() {
    const { container } = await client.database(databaseId).containers.createIfNotExists({ id: containerId });
    console.log(`Created container:\n${config.container.id}\n`);
}

/**
 * Read the container definition
 */
async function readContainer() {
    const { body: containerDefinition } = await client.database(databaseId).container(containerId).read();
    console.log(`Reading container:\n${containerDefinition.id}\n`);
}

/**
 * Create family item if it does not exist
 */
async function createFamilyItem(itemBody) {
    try {
        // read the item to see if it exists
        const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).read();
        console.log(`Item with family id ${itemBody.id} already exists\n`);
    }
    catch (error) {
        // create the family item if it does not exist
        if (error.code === HttpStatusCodes.NOTFOUND) {
            const { item } = await client.database(databaseId).container(containerId).items.create(itemBody);
            console.log(`Created family item with id:\n${itemBody.id}\n`);
        } else {
            throw error;
        }
    }
};

/**
 * Query the container using SQL
 */
async function queryContainer() {
    console.log(`Querying container:\n${config.container.id}`);

    // query to return all children in a family
    const querySpec = {
        query: "SELECT VALUE r.children FROM root r WHERE r.lastName = @lastName",
        parameters: [
            {
                name: "@lastName",
                value: "Andersen"
            }
        ]
    };

    const { result: results } = await client.database(databaseId).container(containerId).items.query(querySpec).toArray();
    for (var queryResult of results) {
        let resultString = JSON.stringify(queryResult);
        console.log(`\tQuery returned ${resultString}\n`);
    }
};

/**
 * Replace the item by ID.
 */
async function replaceFamilyItem(itemBody) {
    console.log(`Replacing item:\n${itemBody.id}\n`);
    // Change property 'grade'
    itemBody.children[0].grade = 6;
    const { item } = await client.database(databaseId).container(containerId).item(itemBody.id).replace(itemBody);
};

/**
 * Delete the item by ID.
 */
async function deleteFamilyItem(itemBody) {
    await client.database(databaseId).container(containerId).item(itemBody.id).delete(itemBody);
    console.log(`Deleted item:\n${itemBody.id}\n`);
};

/**
 * Cleanup the database and container on completion
 */
async function cleanup() {
    await client.database(databaseId).delete();
}

/**
 * Exit the app with a prompt
 * @param {message} message - The message to display
 */
function exit(message) {
    console.log(message);
    console.log('Press any key to exit');
    process.stdin.setRawMode(true);
    process.stdin.resume();
    process.stdin.on('data', process.exit.bind(process, 0));
}

createDatabase()
    .then(() => readDatabase())
    .then(() => createContainer())
    .then(() => readContainer())
    .then(() => createFamilyItem(config.items.Andersen))
    .then(() => createFamilyItem(config.items.Wakefield))
    .then(() => queryContainer())
    .then(() => replaceFamilyItem(config.items.Andersen))
    .then(() => queryContainer())
    .then(() => deleteFamilyItem(config.items.Andersen))
    .then(() => cleanup())
    .then(() => { exit(`Completed successfully`); })
    .catch((error) => { exit(`Completed with error ${JSON.stringify(error)}`) });
```

ターミナルで、```app.js``` ファイルを見つけ、コマンドを実行します。 

```bash 
node app.js
```

開始したアプリケーションの出力が表示されます。 出力は、次のテキスト例のようなものになるはずです。

   ```
    Created database:
    FamilyDatabase

    Reading database:
    FamilyDatabase

    Created container:
    FamilyContainer

    Reading container:
    FamilyContainer

    Created family item with id:
    Anderson.1

    Created family item with id:
    Wakefield.7

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":5,"pets":[{"givenName":"Fluffy"}]}]

    Replacing item:
    Anderson.1

    Querying container:
    FamilyContainer
            Query returned [{"firstName":"Henriette Thaulow","gender":"female","grade":6,"pets":[{"givenName":"Fluffy"}]}]

    Deleted item:
    Anderson.1

    Completed successfully
    Press any key to exit
   ```

お疲れさまでした。 Node.js チュートリアルが完了し、初めての Azure Cosmos DB コンソール アプリケーションが完成しました。

## <a id="GetSolution"></a>完全な Node.js チュートリアル ソリューションを入手する

このチュートリアルの手順を実行する時間がない場合や、コードをダウンロードするだけの場合は、[GitHub](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started ) から入手できます。

この記事のすべてのコードを含む使用の開始ソリューションを実行するには、以下の手順が必要です。

* [Azure Cosmos DB アカウント][create-account]。
* GitHub で入手可能な[使用の開始](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-nodejs-getting-started)ソリューション。

npm で **@azure/cosmos** モジュールをインストールします。 次のコマンドを使用します。

* ```npm install @azure/cosmos --save```

次に、```config.js``` ファイルで、「[手順 3: アプリの構成を設定する](#Config)」の説明に従って、config.endpoint と config.primaryKey の値を更新します。 

次に、ターミナルで ```app.js``` ファイルを見つけ、次のコマンドを実行します。 

```bash 
node app.js
```

以上で完了です。 

## <a name="next-steps"></a>次の手順

* さらに複雑な Node.js のサンプルが必要ですか。 「[Azure Cosmos DB を使用した Node.js Web アプリケーションの作成](sql-api-nodejs-application.md)」を参照してください。
* [Azure Cosmos DB アカウントを監視する](monitor-accounts.md)方法を学習します。
* [クエリのプレイ グラウンド](https://www.documentdb.com/sql/demo)でサンプル データセットに対してクエリを実行します。

[create-account]: create-sql-api-dotnet.md#create-account
[keys]: media/sql-api-nodejs-get-started/node-js-tutorial-keys.png
