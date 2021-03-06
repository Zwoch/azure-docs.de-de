---
title: Verwenden des MXChip IoT DevKit zum Herstellen einer Verbindung mit dem Azure IoT Hub Device Provisioning-Dienst | Microsoft Docs
description: Verwenden des MXChip IoT DevKit zum Herstellen einer Verbindung mit dem Azure IoT Hub Device Provisioning-Dienst
services: iot-dps
keywords: ''
author: liydu
ms.author: liydu
ms.date: 02/20/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 502f22a39622e9a8341e1daca8c9899fd8b7d7d1
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
---
# <a name="connect-the-mxchip-iot-devkit-to-the-azure-iot-hub-device-provisioning-service"></a>Verbinden des MXChip IoT DevKit mit dem Azure IoT Hub Device Provisioning-Dienst

In diesem Artikel wird beschrieben, wie Sie das MXChip IoT DevKit mithilfe des Azure IoT Device Provisioning-Diensts für die automatische Registrierung beim Azure IoT Hub konfigurieren. In diesem Tutorial lernen Sie Folgendes:

* Konfigurieren des globalen Endpunkts des Device Provisioning-Diensts auf einem Gerät
* Verwenden eines eindeutigen geheimen Geräteschlüssels (Unique Device Secret, UDS) zum Generieren eines X.509-Zertifikats
* Registrieren eines einzelnen Geräts
* Überprüfen, ob das Gerät registriert ist

Das [MXChip IoT DevKit](https://aka.ms/iot-devkit) ist ein mit Arduino kompatibles All-in-One-Board mit umfangreichen Peripheriegeräten und Sensoren. Sie können dafür mithilfe der [Visual Studio Code-Erweiterung für Arduino](https://aka.ms/arduino) entwickeln. Im Lieferumfang des DevKits enthalten ist ein wachsender [Projektkatalog](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/), der Sie durch das Erstellen eines Prototyps von Lösungen für das Internet der Dinge (Internet of Things, IoT) führt, die Azure-Dienste nutzen.

## <a name="before-you-begin"></a>Voraussetzungen

Um die Schritte in diesem Tutorial auszuführen, erledigen Sie zuerst die folgenden Aufgaben:

* Bereiten Sie Ihr DevKit vor, indem Sie die in [Verbinden von IoT DevKit AZ3166 mit Azure IoT Hub in der Cloud](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started) beschriebenen Schritte ausführen.
* Führen Sie ein Upgrade auf die neueste Firmware (1.3.0 oder höher) mit dem Tutorial [Update der DevKit-Firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) durch.
* Erstellen und Verknüpfen Sie einen IoT-Hub mit einer Device Provisioning-Dienstinstanz, indem Sie die Schritte in [Einrichten des IoT Hub Device Provisioning-Diensts über das Azure-Portal](https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision) ausführen.

## <a name="set-up-the-device-provisioning-service-configuration-on-the-device"></a>Einrichten der Device Provisioning-Dienstkonfiguration auf dem Gerät

Gehen Sie wie folgt vor, um eine Verbindung des DevKits mit der von Ihnen erstellten Instanz des Device Provisioning-Diensts herzustellen:

1. Wählen Sie im Azure-Portal den Bereich **Übersicht** Ihres Device Provisioning-Diensts aus, und notieren Sie sich die Werte von **Globaler Geräteendpunkt** und **ID-Bereich**.
  ![Globaler Endpunkt und ID-Bereich von DPS](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. Vergewissern Sie sich, dass `git` auf Ihrem Computer installiert ist und den Umgebungsvariablen hinzugefügt wurde, auf die das Befehlsfenster Zugriff hat. Navigieren Sie zu [Software Freedom Conservancy's Git-Clienttools](https://git-scm.com/download/), und prüfen Sie, ob die aktuelle Version installiert ist.

3. Öffnen Sie eine Eingabeaufforderung. Klonen Sie das GitHub-Repository für den Beispielcode für den Device Provisioning-Dienst:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. Öffnen Sie Visual Studio Code, verbinden Sie das DevKit mit dem Computer, und öffnen Sie dann den Ordner, der den von Ihnen geklonten Code enthält.

5. Öffnen Sie **DevKitDPS.ino**. Suchen und ersetzen Sie `[Global Device Endpoint]` und `[ID Scope]` durch die Werte, die Sie sich gerade notiert hatten.
  ![DPS-Endpunkt](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) Sie können die **registrationId** leer lassen. Die ID wird von der Anwendung basierend auf der MAC-Adresse und der Firmwareversion generiert. Wenn Sie die Registrierungs-ID anpassen möchten, beachten Sie, dass nur Kombinationen aus alphanumerischen Kleinbuchstaben und Bindestrichen mit maximal 128 Zeichen Länge zulässig sind. Weitere Informationen finden Sie unter [Verwalten von Geräteregistrierungen mit dem Azure-Portal](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-manage-enrollments).

6. Verwenden Sie Quick Open in Visual Studio Code (Windows: `Ctrl+P`, MacOS: `Cmd+P`), und geben Sie *task device-upload* ein, um den Code zu erstellen und in das DevKit hochzuladen.

7. Im Ausgabefenster wird angezeigt, ob der Vorgang erfolgreich ausgeführt wurde.

## <a name="save-a-unique-device-secret-on-an-stsafe-security-chip"></a>Speichern des eindeutigen Geräteschlüssels auf dem STSAFE-Sicherheitschip

Der Device Provisioning-Dienst kann basierend auf dem zugehörigen [Hardwaresicherheitsmodul](https://azure.microsoft.com/en-us/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) auf dem Gerät konfiguriert werden. Das MXChip IoT DevKit verwendet die [Device Identity Composition Engine](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) der [Trusted Computing Group](https://trustedcomputinggroup.org). Ein *eindeutiger geheimer Geräteschlüssel* (UDS), der auf einem STSAFE-Sicherheitschip im DevKit gespeichert ist, wird zum Generieren des eindeutigen [X.509](https://docs.microsoft.com/en-us/azure/iot-dps/tutorial-set-up-device#select-a-hardware-security-module)-Zertifikats für das Gerät verwendet. Das Zertifikat kann später für den Registrierungsprozess im Device Provisioning-Dienst verwendet werden.

Ein typischer eindeutiger geheimer Geräteschlüssel besteht aus einer Zeichenfolge mit 64 Zeichen, wie im folgenden Beispiel zu sehen:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Jeweils zwei Zeichen werden als Hexadezimalwert in der Sicherheitsberechnung verwendet. Das vorstehende UDS wird folgendermaßen aufgelöst: `0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16`, `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`.

Gehen Sie wie folgt vor, um den eindeutigen geheimen Geräteschlüssel im DevKit zu speichern:

1. Öffnen Sie den seriellen Monitor mithilfe eines Tools wie Putty. Details finden Sie unter [Use configuration mode](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) (Verwenden des Konfigurationsmodus).

2. Wenn das DevKit mit Ihrem Computer verbunden ist, halten Sie Taste **A** gedrückt, und drücken Sie dann die **Reset**-Taste, um in den Konfigurationsmodus zu wechseln. Auf dem Bildschirm werden die DevKit-ID und „Konfiguration“ angezeigt.

3. Verwenden Sie die oben aufgeführt lange UDS-Beispielzeichenfolge, und ändern Sie ein oder mehrere Zeichen in andere Werte zwischen `0` und `f`, um Ihren eigenen UDS zu erhalten.

4. Geben Sie im Fenster des seriellen Monitors *set_dps_uds [your_own_uds_value]* ein, und drücken Sie die EINGABETASTE.
  > [!NOTE]
  > Wenn Sie beispielsweise Ihren eigenen UDS durch Ändern der letzten beiden Zeichen in `f` festgelegt haben, müssen Sie den Befehl wie folgt eingeben: set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff.

5. Drücken Sie auf dem DevKit die **Reset**-Taste, ohne das Fenster des seriellen Monitors zu schließen.

6. Notieren Sie sich die Werte der **DevKit-MAC-Adresse** und der **DevKit-Firmwareversion**.
  ![Firmwareversion](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-an-x509-certificate"></a>Generieren eines X.509-Zertifikats

### <a name="windows"></a>Windows

1. Öffnen Sie den Datei-Explorer, und wechseln Sie zu dem Ordner, der den Beispielcode für den Device Provisioning-Dienst enthält, den Sie zuvor geklont hatten. Suchen und kopieren Sie im Ordner **.build** **DPS.ino.bin** und **DPS.ino.map** in den Ordner, der den Code enthält.
  ![Generierte Dateien](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Wenn Sie die Konfiguration von `built.path` für Arduino in einen anderen Ordner geändert haben, müssen Sie diese Dateien in dem von Ihnen konfigurierten Ordner suchen.

2. Fügen Sie diese beiden Dateien in den Ordner **tools** auf der gleichen Ebene wie der **.build**-Ordner ein.

3. Führen Sie **dps_cert_gen.exe** aus. Folgen Sie den Eingabeaufforderungen zur Eingabe Ihres **UDS**, der **MAC-Adresse** für das DevKit und der **Firmwareversion**, um das X.509-Zertifikat zu generieren.
  ![Ausführen der Datei „dps-cert-gen.exe“](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. Nachdem das X.509-Zertifikat generiert wurde, wird ein **PEM**-Zertifikat im gleichen Ordner gespeichert.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Erstellen eines Geräteregistrierungseintrags im Device Provisioning-Dienst

1. Navigieren Sie im Azure-Portal zu Ihrem Provisioning-Dienst. Wählen Sie **Registrierungen verwalten** und dann die Registerkarte **Individuelle Registrierungen** aus. ![Individuelle Registrierungen](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. Wählen Sie **Hinzufügen**.

3. Wählen Sie unter **Mechanismus** die Option **X.509** aus.
  ![Hochladen des Zertifikats](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

4. Laden Sie in **PEM- oder CER-Zertifikatdatei** das soeben generierte **PEM**-Zertifikat hoch.

5. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie **Speichern** aus.

## <a name="start-the-devkit"></a>Starten des DevKit

1. Öffnen Sie VS Code und den seriellen Monitor.

2. Drücken Sie auf Ihrem DevKit die **Reset**-Taste.

Das DevKit beginnt jetzt mit der Registrierung bei Ihrem Device Provisioning-Dienst.

![Visual Studio Code-Ausgabe](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-that-the-devkit-is-registered-with-azure-iot-hub"></a>Überprüfung, ob das DevKit bei Azure IoT Hub registriert ist

Nach dem Starten Ihres Geräts werden die folgenden Aktionen ausgeführt:

1. Das Gerät sendet eine Registrierungsanforderung an Ihren Device Provisioning-Dienst.
2. Der Device Provisioning-Dienst sendet eine Registrierungsaufforderung zurück, auf die Ihr Gerät reagiert.
3. Nach erfolgreicher Registrierung sendet der Device Provisioning-Dienst den IoT Hub-URI, die Geräte-ID und den verschlüsselten Schlüssel an das Gerät zurück.
4. Die IoT Hub-Clientanwendung auf dem Gerät stellt eine Verbindung mit Ihrem Hub her.
5. Nachdem eine Verbindung mit dem Hub erfolgreich hergestellt wurde, wird das Gerät im Device Explorer von IoT Hub angezeigt.
  ![Registriertes Gerät](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="change-the-device-id"></a>Ändern der Geräte-ID

Die beim Azure IoT Hub registrierte standardmäßige Geräte-ID lautet *AZ3166*. Wenn Sie die ID ändern möchten, folgen Sie den Anweisungen in [Anpassen der Geräte-ID](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Probleme und Feedback

Wenn Probleme auftreten, lesen Sie die [häufig gestellten Fragen](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) (FAQs) zum IoT DevKit, oder wenden Sie sich über folgende Kanäle an uns:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stackoverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie ein Gerät mithilfe der Device Identity Composition Engine sicher beim Device Provisioning-Dienst registriert wird, sodass sich das Gerät automatisch beim Azure IoT Hub registrieren kann. 

Zusammengefasst haben Sie die folgenden Verfahren kennengelernt:

> [!div class="checklist"]
> * Konfigurieren des globalen Endpunkts des Device Provisioning-Diensts auf einem Gerät
> * Verwenden eines eindeutigen geheimen Geräteschlüssels (Unique Device Secret) zum Generieren eines X.509-Zertifikats
> * Registrieren eines einzelnen Geräts
> * Überprüfen, ob das Gerät registriert ist

Informationen zum [Erstellen und Bereitstellen eines simulierten Geräts](./quick-create-simulated-device.md)

