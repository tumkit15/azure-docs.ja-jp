---
title: インクルード ファイル
description: インクルード ファイル
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 07/18/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 03fe587ede297ac7dea90b7a5fb2d5323f60659e
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2018
ms.locfileid: "51628165"
---
| リソース | ターゲット | ハード制限 |
|----------|--------------|------------|
| サブスクリプションごとのストレージ同期サービス数 | リージョンあたり 15 個のストレージ同期サービス数 | いいえ  |
| ストレージ同期サービスごとの同期グループ数 | 100 の同期グループ | [はい] |
| ストレージ同期サービスごとの登録サーバー数 | 99 サーバー | [はい] |
| 同期グループあたりのクラウド エンドポイント数 | 1 クラウド エンドポイント | [はい] |
| 同期グループあたりのサーバー エンドポイント数 | 50 サーバー エンドポイント | いいえ  |
| サーバーあたりのサーバー エンドポイント数 | 33 ～99 サーバー エンドポイント | はい。ただし、構成 (CPU、メモリ、ボリューム、ファイル チャーン、ファイル数など) に基づいて異なります。 |
| エンドポイントのサイズ | 4 TiB | いいえ  |
| 同期グループあたりのファイル システム オブジェクト数 (ディレクトリとファイル) | 2,500 万オブジェクト | いいえ  |
| ディレクトリ内のファイル システム オブジェクト (ディレクトリとファイル) の最大数 | 20 万オブジェクト | [はい] |
| オブジェクト (ディレクトリとファイル) の名前の最大長 | 255 文字 | [はい] |
| オブジェクト (ディレクトリとファイル) のセキュリティ記述子の最大サイズ | 4 KiB | [はい] |
| ファイル サイズ | 100 GiB | いいえ  |
| 階層化するファイルの最小ファイル サイズ | 64 KiB | [はい] |
| 同時同期セッション数 | V4 エージェント: 制限は、使用できるシステム リソースに応じて変わります。 <BR> V3 エージェント: プロセッサごとに 2 つのアクティブな同期セッション、またはサーバーごとに最大 8 つのアクティブな同期セッション | [はい]
