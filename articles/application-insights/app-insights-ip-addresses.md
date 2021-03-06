---
title: Von Application Insights und Log Analytics verwendete IP-Adressen | Microsoft-Dokumentation
description: "Für Application Insights erforderliche Serverfirewallausnahmen"
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 44d989f8-bae9-40ff-bfd5-8343d3e59358
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 01/29/2018
ms.author: mbullwin
ms.openlocfilehash: 9b48b17b214f6ff22c7c68421ba8c89104c8b4b1
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2018
---
# <a name="ip-addresses-used-by-application-insights-and-log-analytics"></a>Von Application Insights und Log Analytics verwendete IP-Adressen
Der Dienst [Azure Application Insights](app-insights-overview.md) verwendet eine Reihe von IP-Adressen. Diese müssen Ihnen gegebenenfalls bekannt sein, wenn die überwachte App hinter einer Firewall gehostet wird.

> [!NOTE]
> Diese Adressen sind zwar statisch, müssen jedoch unter Umständen gelegentlich geändert werden.
> 
> 

## <a name="outgoing-ports"></a>Ausgehende Ports
In der Serverfirewall müssen einige ausgehende Ports geöffnet werden, damit das Application Insights-SDK und/oder der Statusmonitor Daten an das Portal senden kann:

| Zweck | URL | IP | Ports |
| --- | --- | --- | --- |
| Telemetrie |dc.services.visualstudio.com<br/>dc.applicationinsights.microsoft.com |40.114.241.141<br/>104.45.136.42<br/>40.84.189.107<br/>168.63.242.221<br/>52.167.221.184<br/>52.169.64.244 |443 |
| Live Metrics Stream |rt.services.visualstudio.com<br/>rt.applicationinsights.microsoft.com |23.96.28.38<br/>13.92.40.198 |443 |
| Interne Telemetrie |breeze.aimon.applicationinsights.io |52.161.11.71 |443 |

## <a name="status-monitor"></a>Statusmonitor
Statusmonitorkonfiguration – nur erforderlich, wenn Änderungen vorgenommen werden.

| Zweck | URL | IP | Ports |
| --- | --- | --- | --- |
| Konfiguration |`management.core.windows.net` | |`443` |
| Konfiguration |`management.azure.com` | |`443` |
| Konfiguration |`login.windows.net` | |`443` |
| Konfiguration |`login.microsoftonline.com` | |`443` |
| Konfiguration |`secure.aadcdn.microsoftonline-p.com` | |`443` |
| Konfiguration |`auth.gfx.ms` | |`443` |
| Konfiguration |`login.live.com` | |`443` |
| Installation |`packages.nuget.org`, `nuget.org`, `api.nuget.org` | |`443` |

## <a name="hockeyapp"></a>HockeyApp
| Zweck | URL | IP | Ports |
| --- | --- | --- | --- |
| Absturzdaten |gate.hockeyapp.net |104.45.136.42 |80, 443 |

## <a name="availability-tests"></a>Verfügbarkeitstests
Diese Liste enthält die Adressen, von denen aus [Verfügbarkeitswebtests](app-insights-monitor-web-app-availability.md) durchgeführt werden. Wenn Sie Webtests für Ihre App durchführen möchten, Ihr Webserver aber auf die Versorgung bestimmter Clients beschränkt ist, müssen Sie eingehenden Datenverkehr von unseren Verfügbarkeitstestservern zulassen.

Öffnen Sie die Ports 80 (HTTP) und 443 (HTTPS) für eingehenden Datenverkehr von folgenden Adressen (die IP-Adressen sind nach Speicherort gruppiert):

```
Australia East
13.70.83.252
13.75.150.96
13.75.153.9
13.75.158.185
Brazil South
191.232.32.122
191.232.172.45
191.232.176.218
191.232.191.225
France South
52.136.140.221
52.136.140.222
52.136.140.223
52.136.140.226
France Central
52.143.140.242
52.143.140.246
52.143.140.247
52.143.140.249
East Asia
13.75.121.122
23.99.115.153
23.99.123.38
23.102.232.186
52.175.38.49
52.175.39.103
North Europe
13.74.184.101
13.74.185.160
40.69.200.198
52.164.224.46
52.169.12.203
52.169.14.11
52.169.237.149
52.178.183.105
Japan East
52.243.33.33
52.243.33.141
52.243.35.253
52.243.41.117
West Europe
52.174.166.113
52.174.178.96
52.174.31.140
52.174.35.14
52.178.104.23
52.178.109.190
52.178.111.139
52.233.166.221
UK South
51.140.79.229
51.140.84.172
51.140.87.211
51.140.105.74
UK West
51.141.25.219
51.141.32.101
51.141.35.167
51.141.54.177
Southeast Asia
52.187.29.7
52.187.179.17
52.187.76.248
52.187.43.24
52.163.57.91
52.187.30.120
West US
104.45.228.236
104.45.237.251
13.64.152.110
13.64.156.54
13.64.232.251
13.64.236.105
13.91.94.59
40.118.131.182
40.83.189.192
40.83.215.122
Central US
52.165.130.58
52.173.142.229
52.173.147.190
52.173.17.41
52.173.204.247
52.173.244.190
52.173.36.222
52.176.1.226
North Central US
23.96.247.139
23.96.249.113
52.162.124.242
52.162.126.139
52.162.241.147
52.162.246.222
52.162.247.136
52.237.153.231
52.237.154.216
52.237.156.14
52.237.157.218
52.237.157.37
South Central US
104.210.145.106
13.84.176.24
13.84.49.16
13.85.11.137
13.85.26.248
13.85.69.145
52.171.136.162
52.171.141.253
52.171.57.172
52.171.58.140
East US
13.82.218.95
13.90.96.71
13.90.98.52
13.92.137.70
40.85.187.235
40.87.61.61
52.168.8.247
52.170.38.79
52.170.80.61
52.179.9.26

```  

## <a name="application-insights-api"></a>Application Insights API
| Zweck | URI | IP | Ports |
| --- | --- | --- | --- |
| API |api.applicationinsights.io<br/>api1.applicationinsights.io<br/>api2.applicationinsights.io<br/>api3.applicationinsights.io<br/>api4.applicationinsights.io<br/>api5.applicationinsights.io |13.82.26.252<br/>40.76.213.73 |80, 443 |
| API-Dokumentation |dev.applicationinsights.io<br/>dev.applicationinsights.microsoft.com<br/>dev.aisvc.visualstudio.com<br/>www.applicationinsights.io<br/>www.applicationinsights.microsoft.com<br/>www.aisvc.visualstudio.com |13.82.24.149<br/>40.114.82.10 |80, 443 |
| Interne API |aigs.aisvc.visualstudio.com<br/>aigs1.aisvc.visualstudio.com<br/>aigs2.aisvc.visualstudio.com<br/>aigs3.aisvc.visualstudio.com<br/>aigs4.aisvc.visualstudio.com<br/>aigs5.aisvc.visualstudio.com<br/>aigs6.aisvc.visualstudio.com |dynamisch|443 |

## <a name="log-analytics-api"></a>Log Analytics-API
| Zweck | URI | IP | Ports |
| --- | --- | --- | --- |
| API |api.loganalytics.io<br/>*.api.loganalytics.io |dynamisch |80, 443 |
| API-Dokumentation |dev.loganalytics.io<br/>docs.loganalytics.io<br/>www.loganalytics.io |dynamisch |80, 443 |

## <a name="application-insights-analytics"></a>Application Insights Analytics

| Zweck | URI | IP | Ports |
| --- | --- | --- | --- |
| Analytics-Portal | analytics.applicationinsights.io | dynamisch | 80, 443 |
| CDN | applicationanalytics.azureedge.net | dynamisch | 80, 443 |
| Medien-CDN | applicationanalyticsmedia.azureedge.net | dynamisch | 80, 443 |

Hinweis: Für die Domäne „*.applicationinsights.io“ ist das Application Insights-Team verantwortlich.

## <a name="log-analytics-portal"></a>Log Analytics-Portal

| Zweck | URI | IP | Ports |
| --- | --- | --- | --- |
| Portal | portal.loganalytics.io | dynamisch | 80, 443 |
| CDN | applicationanalytics.azureedge.net | dynamisch | 80, 443 |

Hinweis: Die Domäne „*.loganalytics.io“ ist im Besitz des Log Analytics-Teams.

## <a name="application-insights-azure-portal-extension"></a>Azure-Portalerweiterung für Application Insights

| Zweck | URI | IP | Ports |
| --- | --- | --- | --- |
| Erweiterung für Application Insights | stamp2.app.insightsportal.visualstudio.com | dynamisch | 80, 443 |
| Erweiterungs-CDN für Application Insights | insightsportal-prod2-cdn.aisvc.visualstudio.com<br/>insightsportal-prod2-asiae-cdn.aisvc.visualstudio.com<br/>insightsportal-cdn-aimon.applicationinsights.io | dynamisch | 80, 443 |

## <a name="application-insights-sdks"></a>SDKs für Application Insights

| Zweck | URI | IP | Ports |
| --- | --- | --- | --- |
| JS-SDK-CDN für Application Insights | az416426.vo.msecnd.net | dynamisch | 80, 443 |
| Java-SDK für Application Insights | aijavasdk.blob.core.windows.net | dynamisch | 80, 443 |

## <a name="profiler"></a>Profiler

| Zweck | URI | IP | Ports |
| --- | --- | --- | --- |
| Agent | agent.azureserviceprofiler.net<br/>*.agent.azureserviceprofiler.net | 51.143.96.206<br/>51.143.98.157<br/>52.161.8.88<br/>52.161.29.225<br/>52.178.149.106<br/>52.178.147.66<br/>40.68.32.221<br/>104.40.217.71 | 443
| Portal | gateway.azureserviceprofiler.net | dynamisch | 443
| Speicher | *.core.windows.net | dynamisch | 443

## <a name="snapshot-debugger"></a>Debuggen von Momentaufnahmen

| Zweck | URI | IP | Ports |
| --- | --- | --- | --- |
| Agent | ppe.azureserviceprofiler.net<br/>*.ppe.azureserviceprofiler.net | 23.101.68.84<br/>52.174.44.101<br/>52.250.121.195<br/>51.143.88.187<br/> | 443
| Portal | ppe.gateway.azureserviceprofiler.net | dynamisch | 443
| Speicher | *.core.windows.net | dynamisch | 443
