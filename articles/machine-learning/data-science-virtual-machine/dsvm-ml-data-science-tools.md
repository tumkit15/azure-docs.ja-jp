---
title: 機械学習ツールとデータ サイエンス ツール - Azure | Microsoft Docs
description: 機械学習ツールとデータ サイエンス ツール
keywords: データ サイエンス ツール,データ サイエンス仮想マシン, データ サイエンス用ツール, linux データ サイエンス
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 9308c51c0ff5fc7278eab632f9d23f23cf016a2a
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901670"
---
# <a name="machine-learning-and-data-science-tools"></a>機械学習ツールとデータ サイエンス ツール
Azure データ サイエンス仮想マシンには、Python、R、Julia などの一般的な言語で使用できる、機械学習 (ML) のための豊富な一連のツールおよびライブラリがあります。 

データ サイエンス仮想マシン上の ML ツールおよびライブラリのいくつかを次に示します。 

## <a name="azure-machine-learninghttpsdocsmicrosoftcomazuremachine-learningserviceoverview-what-is-azure-ml-sdk"></a>[Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/overview-what-is-azure-ml) SDK
|    |           |
| ------------- | ------------- |
| 紹介   |   Azure Machine Learning は、ML モデルを開発およびデプロイするために使用できるクラウド サービスです。 Python SDK を使用して、モデルの構築、トレーニング、スケール調整、および管理を行うときにそれらのモデルを追跡できます。 モデルをコンテナーとしてデプロイし、それをクラウド、オンプレミス、または Azure IoT Edge で実行します。   |
| サポートされているエディション     | Windows (conda 環境: AzureML)、Linux (conda 環境: py36)    |
| 標準的な使用      | 一般的な ML プラットフォーム      |
| 構成またはインストール方法      |  GPU サポートと共にインストールされます   |
| 使用または実行方法      | Python SDK および Azure CLI として。 Windows エディションでは conda 環境 `AzureML` に、*または* Linux エディションでは `py36` にアクティブ化します。      |
| サンプルへのリンク      | サンプルの Jupyter ノートブックは、`AzureML` ディレクトリの notebooks の下に含まれています。  |
| 関連するツール      | Visual Studio Code、Jupyter   |

## <a name="xgboost"></a>XGBoost 
|    |           |
| ------------- | ------------- |
| 紹介   |    XGBoost は、Python、R、Java、Scala、C++ などのための、高速で、移植可能な、分散型の勾配ブースティング (GBDT、GBRT、または GBM) ライブラリです。 これは 1 台のコンピューター、Hadoop、および Spark 上で実行されます。    |
| サポートされているエディション     | Windows、Linux     |
| 標準的な使用      | 一般的な ML ライブラリ      |
| 構成またはインストール方法      |  GPU サポートと共にインストールされます   |
| 使用または実行方法      | Python ライブラリ (2.7 および 3.5)、R パッケージ、パスの通ったコマンド ライン ツール (Windows では `C:\dsvm\tools\xgboost\bin\xgboost.exe`、Linux では `/dsvm/tools/xgboost/xgboost`) として    |
| サンプルへのリンク      | サンプルは、VM 上の Linux では `/dsvm/tools/xgboost/demo`、Windows では `C:\dsvm\tools\xgboost\demo` に含まれています。   |
| 関連するツール      | LightGBM、MXNet   |



## <a name="vowpal-wabbit"></a>Vowpal Wabbit
|    |           |
| ------------- | ------------- |
| 紹介   |   Vowpal Wabbit ("VW" とも呼ばれます) は、オープンソースの、高速な、out-of-core 方式の学習システム ライブラリです。    |
| サポートされているエディション     | Windows、Linux     |
| 標準的な使用      | 一般的な ML ライブラリ      |
| 構成またはインストール方法      |  Windows--msi インストーラー、Linux--apt-get |
| 使用または実行方法      | パスの通ったコマンド ライン ツール (Windows では `C:\Program Files\VowpalWabbit\vw.exe`、Linux では `/usr/bin/vw`) として    |
| サンプルへのリンク      | [VowPal Wabbit サンプル](https://github.com/JohnLangford/vowpal_wabbit/wiki/Examples) |
| 関連するツール      |LightGBM、MXNet、XGBoost   |


## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| 紹介   |  Weka は、データ マイニング タスクのための ML アルゴリズムのコレクションです。 これらのアルゴリズムは、データ セットに直接適用するか、または独自の Java コードから呼び出すことができます。 Weka には、データの前処理、分類、回帰、クラスタリング、アソシエーション ルール、および視覚化のためのツールが含まれています。 |
| サポートされているエディション     | Windows、Linux     |
| 標準的な使用      | 一般的な ML ツール     |
| 使用または実行方法      | Windows では、[スタート] メニューで Weka を探します。 Linux では、X2Go でサインインし、**[アプリケーション]** > **[開発]** > **[Weka]** に移動します。 |
| サンプルへのリンク      | [Weka サンプル](http://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| 関連するツール      |LightGBM、Rattle、XGBoost   |

## <a name="rattle"></a>Rattle
|    |           |
| ------------- | ------------- |
| 紹介   |   Rattle は、R を使用したデータ マイニングのためのグラフィカル ユーザー インターフェイスです。   |
| サポートされているエディション     | Windows、Linux     |
| 標準的な使用      | R 用の一般的な UI データ マイニング ツール    |
| 使用または実行方法      | UI ツール。 Windows では、コマンド プロンプトを起動し、R を実行してから、R 内で `rattle()` を実行します。 Linux では、X2Go で接続し、ターミナルを起動し、R を実行してから、R 内で `rattle()` を実行します。 |
| サンプルへのリンク      | [Rattle](https://togaware.com/onepager/) |
| 関連するツール      |LightGBM、Weka、XGBoost   |

## <a name="lightgbm"></a>LightGBM
|    |           |
| ------------- | ------------- |
| 紹介   | LightGBM は、ディシジョン ツリー アルゴリズムに基づく、高速な、分散型、高パフォーマンスの勾配ブースティング (GBDT、GBRT、GBM、または MART) フレームワークです。 これは、ランク付け、分類、およびその他の多くの ML タスクに使用されます。    |
| サポートされているバージョン      | Windows、Linux    |
| 標準的な使用      | 汎用の勾配ブースティング フレームワーク      |
| 構成またはインストール方法      | Windows では、LightGBM は Python パッケージとしてインストールされます。 Linux では、コマンド ラインの実行可能ファイルは `/opt/LightGBM/lightgbm` にあり、R パッケージがインストールされ、Python パッケージがインストールされます。     |
| サンプルへのリンク      | [LightGBM Guide (LightGBM ガイド)](https://github.com/Microsoft/LightGBM/tree/master/examples/python-guide)   |
| 関連するツール      | MXNet、XgBoost  |

## <a name="h2o"></a>H2O
|    |           |
| ------------- | ------------- |
| 紹介   | H2O は、インメモリ、分散型、高速で、かつスケーラブルな ML をサポートするオープンソースの AI プラットフォームです。  |
| サポートされているバージョン      | Linux   |
| 標準的な使用      | 汎用で、分散型のスケーラブル ML   |
| 構成またはインストール方法      | H2O は `/dsvm/tools/h2o` にインストールされます。      |
| 使用または実行方法      | X2Go を使用して VM に接続します。 新しいターミナルを起動し、`java -jar /dsvm/tools/h2o/current/h2o.jar` を実行します。 次に、Web ブラウザーを起動して `http://localhost:54321` に接続します。      |
| サンプルへのリンク      | サンプルは、VM 上の Jupyter の `h2o` ディレクトリの下にあります。      |
| 関連するツール      | Apache Spark、MXNet、XGBoost、Sparkling Water、Deep Water    |

データ サイエンス仮想マシン上には、データ サイエンス仮想マシンにインストールされる Anaconda Python ディストリビューションの一部として付属する一般的な `scikit-learn` パッケージなど、その他の ML ライブラリがいくつか存在します。 Python、R、および Julia で使用可能なパッケージの一覧を確認するには、それぞれのパッケージ マネージャーを実行します。
