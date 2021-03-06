---
title: Azure Active Directory Identity Protection Benachrichtigungen
description: Erfahren Sie, wie Benachrichtigungen Ihre Untersuchungsaktivitäten unterstützen.
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: how-to
ms.date: 06/05/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9090ca5b8057179b0cbef1d0a87ae563303ed2c1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85130431"
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Identity Protection Benachrichtigungen

Von Azure AD Identity Protection werden zwei Arten von automatisierten Benachrichtigungs-E-Mails gesendet, die der Verwaltung von Benutzerrisiko- und Risikoerkennungen dienen:

- E-Mail für erkannte gefährdete Benutzer
- Wöchentliche E-Mail mit Übersicht

Dieser Artikel gibt Ihnen eine Übersicht zu beiden Benachrichtigungs-E-Mails.

## <a name="users-at-risk-detected-email"></a>E-Mail für erkannte gefährdete Benutzer

Als Reaktion auf ein erkanntes gefährdetes Konto erstellt Azure Active Directory Identity Protection eine E-Mail-Warnung mit dem Betreff **Gefährdete Benutzer erkannt**. Die E-Mail enthält einen Link zum Bericht vom Typ **[Benutzer mit Risikokennzeichnung](../reports-monitoring/concept-user-at-risk.md)** . Als bewährte Methode sollten Sie die gefährdeten Benutzer sofort untersuchen.

Durch Konfiguration dieser Warnung können Sie angeben, bei welcher Benutzerrisikostufe die Warnung generiert werden soll. Die E-Mail wird generiert, wenn die angegebene Risikostufe des Benutzers erreicht wird. Wenn Sie beispielsweise die Richtlinie für Warnungen auf ein mittleres Benutzerrisiko festlegen und die Risikobewertung Ihres Benutzers Johann aufgrund eines Echtzeit-Anmelderisikos die mittlere Risikostufe erreicht, erhalten Sie eine E-Mail des Typs „Gefährdete Benutzer erkannt“. Wenn für den Benutzer anschließend erneut Risiken ermittelt werden, aufgrund derer die Risikostufe des Benutzers die festgelegte Risikostufe bzw. eine höhere Risikostufe erreicht, erhalten Sie weitere E-Mails dieses Typs, sobald die Risikobewertung des Benutzers neu berechnet wurde. Wenn für einen Benutzer z. B. am 1. Januar die mittlere Risikostufe erreicht wird und Sie in Ihren Einstellungen festgelegt haben, dass bei mittlerem Risiko eine Warnung ausgegeben werden soll, erhalten Sie eine E-Mail-Benachrichtigung. Wenn für denselben Benutzer am 5. Januar ein weiteres mittleres Risiko ermittelt wird und die Risikobewertung des Benutzers nach der Neuberechnung weiterhin ein mittleres Risiko anzeigt, erhalten Sie eine weitere E-Mail-Benachrichtigung. 

Diese zusätzliche E-Mail-Benachrichtigung wird jedoch nur gesendet, wenn der Zeitpunkt der Risikoermittlung (aufgrund derer die Risikostufe des Benutzers geändert wurde) nach dem Zeitpunkt liegt, an dem die letzte E-Mail gesendet wurde. Beispiel: Ein Benutzer meldet sich am 1. Januar um 5:00 Uhr an, und es liegt kein Echtzeitrisiko vor (aufgrund dieser Anmeldung würde also keine E-Mail generiert). Zehn Minuten später, um 5:10 Uhr, meldet sich der Benutzer erneut an, dieses Mal jedoch mit einem hohen Echtzeitrisiko. Aufgrund dieser Anmeldung würde die Risikostufe des Benutzers in „Hoch“ geändert, und es würde eine E-Mail gesendet. Um 5:15 Uhr wird die Offlinerisikobewertung für die ursprüngliche Anmeldung um 5:00 Uhr durch die Offlinerisikoverarbeitung in ein hohes Risiko geändert. Da der Zeitpunkt der ersten Anmeldung vor dem Zeitpunkt der zweiten Anmeldung liegt, für die bereits eine E-Mail-Benachrichtigung gesendet wurde, wird keine zusätzliche E-Mail gesendet.

Um eine übermäßig große Anzahl von E-Mails zu vermeiden, erhalten Sie innerhalb eines Zeitraums von fünf Sekunden nur eine E-Mail für erkannte gefährdete Benutzer. Wenn innerhalb dieses Zeitraums von fünf Sekunden für mehrere Benutzer die angegebene Risikostufe erreicht wird, wird diese Änderung der Risikostufe für alle Benutzer in einer E-Mail zusammengefasst.

![E-Mail für erkannte gefährdete Benutzer](./media/howto-identity-protection-configure-notifications/01.png)

### <a name="configure-users-at-risk-detected-alerts"></a>Konfigurieren von Warnungen zu erkannten gefährdeten Benutzern

Als Administrator können Sie Folgendes festlegen:

- **Die Benutzerrisikostufe, bei der die Erstellung dieser E-Mail ausgelöst wird**: Standardmäßig ist die Risikostufe auf „Hoch“ festgelegt.
- **Die Empfänger dieser E-Mail**: Standardmäßig umfassen die Empfänger alle globalen Administratoren. Globale Administratoren können außerdem weitere globale Administratoren, Sicherheitsadministratoren und Sicherheitsleseberechtigte hinzufügen.
   - Optional können Sie **zusätzliche E-Mails für den Empfang von Warnungsbenachrichtigungen hinzufügen**. Dieses Feature ist eine Vorschau, und die definierten Benutzer müssen über die entsprechenden Berechtigungen zum Anzeigen der verknüpften Berichte im Azure-Portal verfügen.

Konfigurieren Sie die E-Mail-Adresse der gefährdeten Benutzers im **Azure-Portal** unter **Azure Active Directory** > **Sicherheit** > **Identitätsschutz** > **Warnungen zu erkannten gefährdeten Benutzern**.

## <a name="weekly-digest-email"></a>Wöchentliche E-Mail mit Übersicht

Die wöchentliche Übersichts-E-Mail enthält eine Zusammenfassung der neuen Risikoerkennungen.  
Sie hat folgenden Inhalt:

- Neue Benutzer mit Risiko erkannt
- Neue Risikoanmeldungen erkannt (in Echtzeit)
- Links zu verwandten Berichten in Identity Protection

![Wöchentliche E-Mail mit Übersicht](./media/howto-identity-protection-configure-notifications/weekly-digest-email.png)

Standardmäßig umfassen die Empfänger alle globalen Administratoren. Globale Administratoren können außerdem weitere globale Administratoren, Sicherheitsadministratoren und Sicherheitsleseberechtigte hinzufügen.

### <a name="configure-weekly-digest-email"></a>Konfigurieren von wöchentlichen Zusammenfassung per E-Mail

Als Administrator können Sie das Senden einer wöchentlichen Zusammenfassung per E-Mail aktivieren oder deaktivieren und die Benutzer auswählen, denen die E-Mail gesendet werden soll.

Konfigurieren Sie die wöchentliche Zusammenfassung per E-Mail im **Azure-Portal** unter **Azure Active Directory** > **Sicherheit** > **Informationsschutz** > **Wöchentliche Übersicht**.

## <a name="see-also"></a>Weitere Informationen

- [Azure Active Directory Identity Protection](../active-directory-identityprotection.md)
