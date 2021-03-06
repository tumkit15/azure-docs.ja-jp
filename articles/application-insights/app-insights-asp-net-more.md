---
title: Azure Application Insights をさらに活用する | Microsoft Docs
description: Application Insights の利用開始後に使用できる機能の概要を示します。
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 02/03/2017
ms.author: mbullwin
ms.openlocfilehash: a1244076efe1c920a23f29de9d25ab6845747fe4
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2018
ms.locfileid: "51245853"
---
# <a name="more-telemetry-from-application-insights"></a>Application Insights からのテレメトリの追加
[Application Insights を ASP.NET コードに追加](app-insights-asp-net.md)した後、さらに多くのテレメトリを取得するためにできることがいくつかあります。 

| [操作] | 取得内容|
|---|---|
|(IIS サーバー) 各サーバー コンピューターに [Status Monitor をインストール](https://go.microsoft.com/fwlink/?LinkId=506648)する。<br/>(Azure Web アプリ) Web アプリの Azure コントロール パネルで Application Insights ブレードを開く。| [**パフォーマンス カウンター**](app-insights-performance-counters.md)<br/>[**例外**](app-insights-asp-net-exceptions.md) - 詳細なスタック トレース<br/>[**依存関係**](app-insights-asp-net-dependencies.md)|
|[Web ページに JavaScript のスニペットを追加する](app-insights-javascript.md)|[ページのパフォーマンス](app-insights-usage-overview.md)、ブラウザーの例外、AJAX のパフォーマンス。 カスタム クライアント側テレメトリ。|
|[可用性 Web テストを作成する](app-insights-monitor-web-app-availability.md)|サイトが使用できなくなった場合、アラートを取得します|
|MSBuild によって [buildinfo.config が生成されることを確認する](https://msdn.microsoft.com/library/dn449058.aspx)|[メトリック チャートで注釈を構築します](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[カスタム イベントとメトリックを作成する](app-insights-api-custom-events-metrics.md)|ビジネス イベントとメトリックのカウント、詳細な使用状況の追跡、その他を行います。|
|[ライブ サイトのプロファイリング](https://aka.ms/AIProfilerPreview)|ライブ Web アプリからの詳細な関数タイミング|






