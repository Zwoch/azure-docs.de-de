---
title: Referenz zu Azure IoT EdgeAgent und EdgeHub | Microsoft-Dokumentation
description: Enthält die spezifischen Eigenschaften und die dazugehörigen Werte für die Modulzwillinge edgeAgent und edgeHub.
services: iot-edge
keywords: ''
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 03/14/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 17c97c6f233c9861ac42abc0a1f11089bb938e7c
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2018
---
# <a name="properties-of-the-edge-agent-and-edge-hub-module-twins"></a>Eigenschaften der Modulzwillinge „Edge-Agent“ und „Edge-Hub“

Der Edge-Agent und der Edge-Hub sind die zwei Module, aus denen die IoT Edge-Laufzeit besteht. Weitere Informationen dazu, welche Aufgaben von einem Modul jeweils durchgeführt werden, finden Sie unter [Grundlegendes zur Azure IoT Edge-Laufzeit und ihrer Architektur]](iot-edge-runtime.md). 

Dieser Artikel enthält die gewünschten Eigenschaften und gemeldeten Eigenschaften der Laufzeitmodulzwillinge. Weitere Informationen zur Bereitstellung von Modulen auf IoT Edge-Geräten finden Sie unter [Bereitstellung und Überwachung][lnk-deploy].

## <a name="edgeagent-desired-properties"></a>Gewünschte EdgeAgent-Eigenschaften

Der Modulzwilling für den Edge-Agent heißt `$edgeAgent` und koordiniert die Kommunikationsvorgänge zwischen dem auf einem Gerät ausgeführten Edge-Agent und IoT Hub. Die gewünschten Eigenschaften werden festgelegt, wenn ein Bereitstellungsmanifest auf einem bestimmten Gerät im Zuge einer Einzelgeräte- oder einer umfassenden Bereitstellung angewendet wird. 

| Eigenschaft | BESCHREIBUNG | Erforderlich |
| -------- | ----------- | -------- |
| Schemaversion | Muss „1.0“ sein. | Ja |
| runtime.type | Muss „Docker“ sein. | Ja |
| runtime.settings.minDockerVersion | Legen Sie hier die für dieses Bereitstellungsmanifest mindestens erforderliche Docker-Version fest. | Ja |
| runtime.settings.loggingOptions | In eine Zeichenfolge umgewandelter JSON-Code mit den Protokollierungsoptionen für den Edge-Agent-Container. [Docker-Protokollierungsoptionen][lnk-docker-logging-options] | Nein  |
| systemModules.edgeAgent.type | Muss „Docker“ sein. | Ja |
| systemModules.edgeAgent.settings.image | Der URI des Edge-Agent-Image. Der Edge-Agent ist gegenwärtig nicht in der Lage, sich selbst zu aktualisieren. | Ja |
| systemModules.edgeAgent.settings.createOptions | In eine Zeichenfolge umgewandelter JSON-Code, der die Optionen für die Erstellung des Edge-Agent-Containers enthält. [Optionen zum Erstellen von Docker][lnk-docker-create-options] | Nein  |
| systemModules.edgeAgent.configuration.id | Die ID der Bereitstellung, von der dieses Modul bereitgestellt wurde. | Sie wird von IoT Hub festgelegt, wenn dieses Manifest mithilfe einer Bereitstellung angewendet wird. Nicht Teil eines Bereitstellungsmanifests. |
| systemModules.edgeHub.type | Muss „Docker“ sein. | Ja |
| systemModules.edgeHub.type | Muss „running“ sein. | Ja |
| systemModules.edgeHub.restartPolicy | Muss „always“ sein. | Ja |
| systemModules.edgeHub.settings.image | Der URI des Image des Edge-Hubs. | Ja |
| systemModules.edgeHub.settings.createOptions | In eine Zeichenfolge umgewandelter JSON-Code mit den Optionen für die Erstellung des Edge-Hubcontainers. [Optionen zum Erstellen von Docker][lnk-docker-create-options] | Nein  |
| systemModules.edgeHub.configuration.id | Die ID der Bereitstellung, von der dieses Modul bereitgestellt wurde. | Sie wird von IoT Hub festgelegt, wenn dieses Manifest mithilfe einer Bereitstellung angewendet wird. Nicht Teil eines Bereitstellungsmanifests. |
| modules.{moduleId}.version | Benutzerdefinierte Zeichenfolge, die die Version des Moduls darstellt. | Ja |
| modules.{moduleId}.type | Muss „Docker“ sein. | Ja |
| modules.{moduleId}.restartPolicy | {"never" \| "on-failed" \| "on-unhealthy" \| "always"} | Ja |
| modules.{moduleId}.settings.image | URI des Modulimage. | Ja |
| modules.{moduleId}.settings.createOptions | In eine Zeichenfolge umgewandelter JSON-Code mit den Optionen für die Erstellung des Modulcontainers. [Optionen zum Erstellen von Docker][lnk-docker-create-options] | Nein  |
| modules.{moduleId}.configuration.id | Die ID der Bereitstellung, von der dieses Modul bereitgestellt wurde. | Sie wird von IoT Hub festgelegt, wenn dieses Manifest mithilfe einer Bereitstellung angewendet wird. Nicht Teil eines Bereitstellungsmanifests. |

## <a name="edgeagent-reported-properties"></a>Gemeldete EdgeAgent-Eigenschaften

Zu den vom Edge-Agent gemeldeten Eigenschaften gehören im Wesentlichen drei Informationen:

1. Status der Anwendung der zuletzt erkannten gewünschten Eigenschaften
2. Status der Module, die nach Angaben des Edge-Agents derzeit auf dem Gerät ausgeführt werden
3. Kopie der gewünschten Eigenschaften, die derzeit auf dem Gerät ausgeführt werden

Diese letzte Information ist nützlich für den Fall, dass die letzten gewünschten Eigenschaften nicht erfolgreich von der Runtime angewendet wurden und das Gerät demzufolge noch ein vorheriges Bereitstellungsmanifest ausführt.

> [!NOTE]
> Die gemeldeten Eigenschaften des Edge-Agents sind hilfreich, da sie mit der [IoT Hub-Abfragesprache][lnk-iothub-query] abgefragt werden können, um den Status der Bereitstellungen großflächig zu untersuchen. Weitere Informationen zur Verwendung dieses Features finden Sie unter [Bereitstellungen][lnk-deploy].

In der folgenden Tabelle sind die aus den gewünschten Eigenschaften kopierten Informationen nicht enthalten.

| Eigenschaft | BESCHREIBUNG |
| -------- | ----------- |
| lastDesiredVersion | Dieser ganzzahlige Wert bezeichnet die letzte vom Edge-Agent verarbeitete Version der gewünschten Eigenschaften. |
| lastDesiredStatus.code | Dieser Statuscode nennt die letzten gewünschten Eigenschaften, die der Edge-Agent erkannt hat. Zulässige Werte: `200` Success (Erfolg), `400` Invalid Configuration (Ungültige Konfiguration), `412` Invalid Schema Version (Ungültige Schemaversion), `417` The Desired Properties are Empty (Die gewünschten Eigenschaften sind leer), `500` Failed (Fehler) |
| lastDesiredStatus.description | Textbeschreibung des Status |
| deviceHealth | `healthy`, wenn der Runtimestatus aller Module entweder `running` oder `stopped` ist, andernfalls `unhealthy` |
| configurationHealth.{deploymentId}.health | `healthy`, wenn der von der Bereitstellung {deploymentId} festgelegte Runtimestatus aller Module `running` oder `stopped` ist, andernfalls `unhealthy` |
| runtime.platform.OS | Meldung des auf dem Gerät ausgeführten Betriebssystems |
| runtime.platform.architecture | Meldung der CPU-Architektur auf dem Gerät |
| systemModules.edgeAgent.runtimeStatus | Gemeldeter Status des Edge-Agents: {"running" \| "unhealthy"} |
| systemModules.edgeAgent.statusDescription | Textbeschreibung des gemeldeten Status des Edge-Agents. |
| systemModules.edgeHub.runtimeStatus | Aktueller Status des Edge-Hubs: { "running" \| "stopped" \| "failed" \| "backoff" \| "unhealthy" } |
| systemModules.edgeHub.statusDescription | Textbeschreibung des aktuellen Status des Edge-Hub, falls „fehlerhaft“ |
| systemModules.edgeHub.exitCode | Der vom Edge-Hubcontainer gemeldete Exitcode beim Beenden |
| systemModules.edgeHub.startTimeUtc | Zeitpunkt des letzten Starts des Edge-Hubs |
| systemModules.edgeHub.lastExitTimeUtc | Zeitpunkt des letzten Beendens des Edge-Hubs |
| systemModules.edgeHub.lastRestartTimeUtc | Zeitpunkt des letzten Neustarts des Edge-Hubs |
| systemModules.edgeHub.lastRestartTimeUtc | Neustarthäufigkeit dieses Moduls aufgrund der Neustartrichtlinie |
| modules.{moduleId}.runtimeStatus | Aktueller Status des Moduls: { "running" \| "stopped" \| "failed" \| "backoff" \| "unhealthy" } |
| modules.{moduleId}.statusDescription | Textbeschreibung des aktuellen Modulstatus, falls fehlerhaft |
| modules.{moduleId}.exitCode | Der vom Modulcontainer gemeldete Exitcode beim Beenden |
| modules.{moduleId}.startTimeUtc | Zeitpunkt des letzten Modulstarts |
| modules.{moduleId}.lastExitTimeUtc | Zeitpunkt der letzten Modulbeendigung |
| modules.{moduleId}.lastRestartTimeUtc | Zeitpunkt des letzten Modulneustarts |
| modules.{moduleId}.restartCount | Neustarthäufigkeit dieses Moduls aufgrund der Neustartrichtlinie |

## <a name="edgehub-desired-properties"></a>Gewünschte EdgeHub-Eigenschaften

Der Modulzwilling für den Edge-Hub wird als `$edgeHub` bezeichnet und koordiniert die Kommunikationsvorgänge zwischen dem auf einem Gerät ausgeführten Edge-Hub und IoT Hub. Die gewünschten Eigenschaften werden festgelegt, wenn ein Bereitstellungsmanifest auf einem bestimmten Gerät im Zuge einer Einzelgeräte- oder einer umfassenden Bereitstellung angewendet wird. 

| Eigenschaft | BESCHREIBUNG | Im Bereitstellungsmanifest erforderlich |
| -------- | ----------- | -------- |
| Schemaversion | Muss „1.0“ sein. | Ja |
| routes.{routeName} | Eine Zeichenfolge, die eine Edge-Hubroute darstellt. | Das `routes`-Element kann vorhanden, aber leer sein. |
| storeAndForwardConfiguration.timeToLiveSecs | Zeit in Sekunden, wie lange der Edge-Hub Nachrichten nach einer Trennung von Routingendpunkten aufbewahrt, z.B. nach einer Trennung vom IoT Hub oder einem lokalen Modul | Ja |

## <a name="edgehub-reported-properties"></a>Gemeldete EdgeHub-Eigenschaften

| Eigenschaft | Beschreibung |
| -------- | ----------- |
| lastDesiredVersion | Dieser ganzzahlige Wert gibt die letzte Version der gewünschten Eigenschaften an, die vom Edge-Hub verarbeitet wurde. |
| lastDesiredStatus.code | Dieser Statuscode nennt die letzten gewünschten Eigenschaften, die der Edge-Hub erkannt hat. Zulässige Werte: `200` Success (Erfolg), `400` Invalid Configuration (ungültige Konfiguration), `500` Failed (Fehler) |
| lastDesiredStatus.description | Textbeschreibung des Status |
| clients.{device or module identity}.status | Konnektivitätsstatus dieses Geräts oder Moduls. Mögliche Werte: {"connected" \| "disconnected"}. Nur Modulidentitäten können den Status „disconnected“ aufweisen. Nachgeschaltete Geräte, die eine Verbindung mit dem Edge-Hub herstellen, erscheinen nur bei erfolgreicher Verbindung. |
| clients.{device or module identity}.lastConnectTime | Letzter Verbindungszeitpunkt des Geräts oder Moduls |
| clients.{device or module identity}.lastDisconnectTime | Letzter Trennungszeitpunkt des Geräts oder Moduls |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Verwendung dieser Eigenschaften zum Erstellen von Bereitstellungsmanifesten finden Sie unter [Verstehen, wie IoT Edge-Module verwendet, konfiguriert und wiederverwendet werden können](module-composition.md).

<!--links -->
[lnk-deploy]: module-deployment-monitoring.md
[lnk-docker-create-options]: https://docs.docker.com/engine/api/v1.32/#operation/ContainerCreate
[lnk-docker-logging-options]: https://docs.docker.com/engine/admin/logging/overview/
[lnk-iothub-query]: ../iot-hub/iot-hub-devguide-query-language.md
