---
title: インクルード ファイル
description: インクルード ファイル
services: iot-accelerators
author: avneet723
ms.service: iot-accelerators
ms.topic: include
ms.date: 10/29/2018
ms.author: avneet723
ms.custom: include file
ms.openlocfilehash: 900d75f826830ea7336044a892506d3bec546e30
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283932"
---
## <a name="download-the-source-code"></a>ソース コードをダウンロードする

リモート監視ソース コード リポジトリには、マイクロサービスの Docker イメージを実行するために必要な Docker 構成ファイルが含まれています。

複製してローカル バージョンのリポジトリを作成するには、コマンドライン環境を使用してローカル コンピューター上の適切なフォルダーに移動します。 次に、次のいずれかのコマンド セットを実行して、.NET または Java リポジトリのいずれかを複製します。

.NET マイクロサービスの実装の最新バージョンをダウンロードするには、次のコマンドを実行します。

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-dotnet
git submodule foreach git pull origin master
```

Java マイクロサービスの実装の最新バージョンをダウンロードするには、次のコマンドを実行します。

```cmd/sh
git clone --recurse-submodules https://github.com/Azure/azure-iot-pcs-remote-monitoring-java.git

# To retrieve the latest submodules, run the following command:

cd azure-iot-pcs-remote-monitoring-java
git submodule foreach git pull origin master
```

> [!NOTE]
> これらのコマンドでは、マイクロサービスのローカル実行に使用するスクリプトだけでなく、すべてのマイクロサービスのソース コードがダウンロードされます。 Docker 内のマイクロサービスを実行するためのソース コードは必要ありませんが、後でソリューション アクセラレータを変更し、その変更をローカルにテストすることを予定している場合は、ソース コードが役に立ちます。

## <a name="deploy-the-azure-services"></a>Azure サービスをデプロイする

この記事ではマイクロサービスをローカルに実行する方法を示しますが、これらはクラウド内で実行されている Azure サービスに依存します。 Azure サービスをデプロイするには、次のスクリプトを使用します。 次のスクリプトの例では、Windows コンピューターで .NET リポジトリを使用していることを前提としています。 別の環境で作業している場合は、パス、ファイル拡張子、およびパスの区切り記号を適切に調整してください。

### <a name="create-new-azure-resources"></a>新しい Azure リソースを作成する

必要な Azure リソースをまだ作成していない場合は、次の手順のようにします。

1. コマンドライン環境で、リポジトリの複製コピーの **\services\scripts\local\launch** フォルダーに移動します。

1. 次のコマンドを実行して **pcs** CLI ツールをインストールし、Azure アカウントにサインインします。

    ```cmd
    npm install -g iot-solutions
    pcs login
    ```

1. **start.cmd** スクリプトを実行します。 このスクリプトでは、次の情報を入力するよう求められます。
    * ソリューション名。
    * 使用する Azure サブスクリプション。
    * 使用する Azure データセンターの場所。

    スクリプトで、指定したソリューション名のリソース グループが Azure に作成されます。 このリソース グループには、ソリューション アクセラレータが使用する Azure リソースが含まれます。 該当するリソースが不要になった場合は、このリソース グループを削除できます。

    また、このスクリプトは、プレフィックス **PCS** を持つ一連の環境変数をローカル コンピューターに追加します。 Docker コンテナーをローカルで起動すると、Docker コンテナーはこれらの環境変数からその構成値を読み取ります。

> [!TIP]
> スクリプトが完了すると、環境変数の一覧が表示されます。 これらの値を **services\\scripts\\local\\.env** ファイルに保存すると、将来ソリューション アクセラレータをデプロイする時に使用できます。 **docker-compose** を実行すると、ローカル コンピューターに設定した環境変数が、**services\\scripts\\local\\.env** ファイル内の値をオーバーライドすることに注意してください。

### <a name="use-existing-azure-resources"></a>既存の Azure リソースを使用する

必要な Azure リソースを既に作成している場合は、ローカル コンピューターに対応する環境変数を作成します。 最後のデプロイの一部として、**services\\scripts\\local\\.env** ファイルにこれらの値を保存している可能性があります。 **docker-compose** を実行すると、ローカル コンピューターに設定した環境変数が、**services\\scripts\\local\\.env** ファイル内の値をオーバーライドすることに注意してください。