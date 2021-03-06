---
title: Anbieten von Diensten in Azure Stack | Microsoft-Dokumentation
description: Als Cloudbetreiber können Sie Dienste für Ihre Benutzer anbieten.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: 5b117a9b174f5d2419ff596cc90436e4b9d21645
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2018
---
# <a name="overview-of-offering-services-in-azure-stack"></a>Übersicht über das Anbieten von Diensten in Azure Stack

*Gilt für: integrierte Azure Stack-Systeme und Azure Stack Development Kit*

[Microsoft Azure Stack](azure-stack-poc.md) ist eine Hybrid-Cloudplattform, die das Bereitstellen von Diensten aus Ihrem Datencenter ermöglicht. Als Dienstanbieter können Sie Dienste für Ihre Mandanten anbieten. Innerhalb eines Unternehmens oder einer Behörde ist es möglich, den Mitarbeitern lokale Dienste anzubieten. Die Dienste, die Sie bereitstellen können umfassen u.a.:

- PaaS-Dienste (Platform-as-a-Service) wie App Services, API-Apps, API-Funktionen, SQL, MySQL.

Sie können auch Dienste kombinieren, um komplexe Lösungen für verschiedene Benutzer zu integrieren und zu erstellen.

Sie müssen [Pläne, Angebote und Kontingente](azure-stack-plan-offer-quota-overview.md) erstellen, um diese Dienste für Ihre Benutzer bereitzustellen. Ihre Benutzer können Ihre Angebote dann abonnieren, um die Dienste zu nutzen.

## <a name="plan-your-service-offers"></a>Planen Ihrer Dienstangebote

Berücksichtigen Sie beim Planen Ihrer Angebote Folgendes:

**Testangebote**: Testangebote sind praktisch, um neue Benutzer zu gewinnen, die dann ein Upgrade für zusätzliche Dienste durchführen können. Erstellen Sie für ein Testangebot einen kleinen [Basisplan](azure-stack-plan-offer-quota-overview.md#base-plan) mit einem optionalen größeren Add-On-Plan.

**Kapazitätsplanung**: Möglicherweise haben Sie Bedenken, dass Benutzer große Mengen an Ressourcen abrufen und das System für alle Benutzer lahmlegen. Sie können [Ihre Pläne mit Kontingenten konfigurieren](azure-stack-plan-offer-quota-overview.md#plans), um eine Obergrenze für die Nutzung festzulegen und so die Leistung zu verbessern.

**Delegierte Anbieter**: Bieten Sie anderen die Möglichkeit, Angebote in Ihrer Umgebung zu erstellen. Wenn Sie z.B. ein Dienstanbieter sind, können Sie diese Möglichkeit an Ihre Wiederverkäufer [delegieren](azure-stack-delegated-provider.md). Oder Unternehmen nutzen diese Möglichkeit, um solche Optionen an bestimmte Abteilungen oder Niederlassungen zu delegieren.

## <a name="next-steps"></a>Nächste Schritte
[Erstellen von Angeboten in Azure Stack](azure-stack-create-offer.md)

