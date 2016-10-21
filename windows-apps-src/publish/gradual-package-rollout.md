---
author: JnHs
Description: "Wenn Ihre App ein AdMediatorControl- oder ein AdControl-Element zum Anzeigen von Werbebannern verwendet, können Sie Ihre Anzeigenfüllrate und Ihren Umsatz steigern, indem Sie in Ihrer App Microsoft-Partneranzeigen anzeigen."
title: Schrittweises Paketrollout
translationtype: Human Translation
ms.sourcegitcommit: ac9eed95edba99cdba914ff21b25383f35a20012
ms.openlocfilehash: 3ec642ef0a21d06b20cabb12a47d2abcfd19fe59

---

# Schrittweises Paketrollout

Wenn Sie ein Update für eine Übermittlung veröffentlichen möchten, können Sie die aktualisierten Pakete schrittweise an Kunden Ihrer App unter Windows10 bereitstellen. So können Sie Feedback und Analysedaten für die jeweiligen Pakete überwachen und vor einem umfassenden Rollout sicherstellen, dass das Update ordnungemäß funktioniert. Sie können den Prozentsatz jederzeit erhöhen (oder das Update stoppen), ohne eine neue Übermittlung zu erstellen. 

> **Wichtig** Ihre Rolloutauswahl gilt für alle Pakete, wird jedoch nur bei Kunden mit Betriebssystemversionen mit Unterstützung für Flight-Pakete (Windows.Desktop Build 10586 oder höher, Windows.Mobile Build 10586.63 oder höher und Xbox) übernommen, einschließlich bei Kunden, die Ihre App über [Vom Store verwaltete (Online-)Lizenzierung](organizational-licensing.md) aus dem [Windows Store für Unternehmen](https://www.microsoft.com/business-store) beziehen. Beim schrittweisen Paketrollout erhalten Kunden mit älteren Betriebssystemversionen keine Pakete aus der aktuellen Übermittlung, bis Sie das Paketrollout wie nachfolgend beschrieben abschließen.

Beachten Sie, dass alle Ihre Kunden den mit Ihrer letzten Übermittlung eingegebenen Store-Eintrag anzeigen können. Die Rollout-Einstellungen gelten nur für die Pakete, die Kunden erhalten (Neukunden und Updates für vorhandene Kunden).

> **Tipp** Beim Paketrollout werden Pakete an eine zufällige Auswahl von Kunden in Höhe des von Ihnen angegebenen Prozentsatzes verteilt. Um bestimmte Pakete an von Ihnen ausgewählte Kunden zu verteilen, können Sie Flight-Pakete verwenden.  Sie können das Rollout auch mit Ihren Flight-Paketen kombinieren, wenn Sie ein Update schrittweise auf eine Ihrer Test-Flight-Gruppen verteilen möchten.

## Festlegen eines Rollout-Prozentsatzes

Sie können das Rollout eines Updates auf der Seite **Pakete** einer aktualisierten Einreichung auswählen. Aktivieren Sie hierfür das Kontrollkästchen **Update-Rollout schrittweise nach Veröffentlichung dieser Übermittlung (nur für Windows10-Kunden)**. Geben Sie den Prozentsatz an Kunden an, die das Update bei erster Veröffentlichung der Übermittlung erhalten sollen. Geben Sie z.B. 5 ein, wenn Sie das Update zunächst nur für einen kleinen Teil Ihrer App-Kunden veröffentlichen möchten.

Klicken Sie auf **Update**, um Ihre Auswahl zu speichern. Nachdem Ihre App den Zertifizierungsprozess abgeschlossen hat, werden die Pakete an Kunden gemäß dem von Ihnen angegebenen Prozentsatz verteilt (dies gilt für Neukunden und Updates für vorhandene Kunden).

## Anpassen des Rollouts, nachdem die Übermittlung veröffentlicht wurde

Sie können das Rollout, nachdem die Übermittlung veröffentlicht wurde, in der App-Übersicht anpassen. Sie können den Prozentsatz der Kunden, die das Paket Ihrer aktuellen Übermittlung erhalten, über den Schieberegler ändern. Klicken Sie auf **Update**, um Ihre Auswahl zu speichern. Die Pakete werden gemäß dem von Ihnen angegebenen Prozentsatz an Kunden verteilt (dies gilt für Neukunden und Updates für vorhandene Kunden).

## Abschließen des Rollouts

Bevor Sie eine neue Übermittlung erstellen können, müssen Sie das Paketrollout abschließen. Sie können das Rollout **abschließen** und die aktuellen Pakete an alle Ihre Kunden verteilen, oder das Rollout **anhalten**, um die Verteilung der aktuellen Pakete zu stoppen.

Wenn Sie das Update für zuverlässig halten und dieses all Ihren Kunden verfügbar machen möchten, klicken Sie auf **Paketrollout abschließen**. Dann werden die neuesten Pakete an alle Kunden verteilt.

> **Tipp** Wenn Sie den Prozentsatz für das Rollout auf 100% festlegen, wird dadurch nicht sichergestellt, dass alle Ihre Kunden die Pakete von der aktuellen Übermittlung erhalten, da einige Kunden möglicherweise Betriebssystemversionen verwenden, die ein Rollout nicht unterstützen. Sie müssen das Rollout abschließen, um die Verteilung von älteren Pakete zu beenden, und allen vorhandenen Kunden das Update bereitstellen.

Wenn Sie feststellen, dass ein Problem mit dem Update besteht, und Sie dieses nicht mehr verbreiten möchten, klicken Sie auf **Paketrollout anhalten**, um die Verteilung von Paketen der letzten Übermittlung zu beenden. Wenn Sie ein Paketrollout anhalten, werden diese Pakete nicht mehr an Kunden verteilt. Nur die Pakete aus der vorherigen Übermittlung werden an neue oder vorhandene Kunden verteilt. Alle Kunden, die bereits über die neueren Pakete verfügen, behalten diese Pakete auch. Ein Rollback auf die vorherige Version wird nicht ausgeführt. Um diesen Kunden ein Update bereitzustellen, müssen Sie eine neue Übermittlung mit den Paketen erstellen, die Sie bereitstellen möchten. Beachten Sie, dass bei einem schrittweisen Rollout bei der nächsten Übermittlung Kunden mit Paketen, die Sie angehalten haben, neue Updates in der gleichen Reihenfolge angeboten werden wie bei der ursprünglichen Verteilung. Das neue Rollout liegt zwischen der letzten abgeschlossenen Übermittlung und Ihrer aktuellen Übermittlung. Nachdem Sie einen Paketrollout anhalten, werden diese Pakete nicht mehr an Kunden verteilt.



<!--HONumber=Aug16_HO5-->


