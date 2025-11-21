# Azure Sentinel SOC Home Lab - Dokumentacja Projektu

Ten projekt dokumentuje budowę domowego laboratorium SOC (Security Operations Center) opartego na chmurze Microsoft Azure (Sentinel). Celem było zasymulowanie cyberataków, ich detekcja za pomocą reguł analitycznych (KQL) oraz automatyzacja reakcji (SOAR).

## 1. Architektura i Konfiguracja Środowiska
W tej sekcji przedstawiono topologię sieci oraz proces podłączania maszyn (zarówno Azure, jak i on-premise) do usługi Log Analytics Workspace.

* **Topologia sieci:**
    ![Diagram sieci](Screenshots/Diagram.png)
    *Opisuje przepływ logów z maszyn wirtualnych do honeypota i Azure Sentinel.*

* **Konfiguracja Agenta i Azure Arc:**
    * Podłączenie maszyn spoza Azure: ![Agent Setup](Screenshots/agent_non_Azure_WM.jpg)
    * Proces instalacji agenta: ![Instalacja](Screenshots/installation.jpg)
    * Widok w Azure Arc: ![Azure Arc](Screenshots/azure_arc.png)
    * Logowanie do panelu Arc: ![Logon Arc](Screenshots/logon_Azure_Arc.png)

---

## 2. Symulacja Ataków (Red Teaming)
W tej fazie przeprowadzono kontrolowane ataki, aby wygenerować logi zdarzeń (Security Events). Skupiono się na technikach Discovery, Persistence oraz Defense Evasion.

> **Zastosowane Techniki (zgodnie z MITRE ATT&CK):**
> 1.  **Discovery:** Skanowanie i rekonesans (`whoami`, `ipconfig`, `net user`).
> 2.  **Persistence:** Dodawanie użytkownika do grupy Administratorów.
> 3.  **Impact/Evasion:** Zatrzymywanie usług i czyszczenie logów.

* **Wykonywanie skryptów Powershell:**
    ![Symulacja ataku](Screenshots/simulation_attack_powershell.png)
* **Komendy rozpoznawcze (Discovery):**
    ![Komendy PowerShell](Screenshots/commends_powershell.png)
    *Widoczne użycie komend generujących szum w logach, wykrywanych przez korelacje procesów cmd.exe/powershell.exe.*

---

## 3. Detekcja i Analiza Zagrożeń (Blue Teaming)
Wykorzystanie języka KQL (Kusto Query Language) do tworzenia reguł analitycznych w Microsoft Sentinel.

* **Reguły Analityczne (Analytics Rules):**
    ![Lista reguł](Screenshots/sentinel_rules.png)
* **Szczegóły reguły (DCR):**
    ![Data Collection Rule](Screenshots/rule_dcr.png)
* **Analiza logów Brute Force / Logowania:**
    ![Logi Brute Force](Screenshots/bruteforce_log_info.jpg)
    ![Uwierzytelnianie](Screenshots/authentication.png)
    *Analiza Event ID 4624/4625 oraz prób przełamania haseł.*
* **Threat Hunting:**
    ![Threat Hunting](Screenshots/threat_hunting_powershell_detection.png) 

---

## 4. Zarządzanie Incydentami i Reakcja (Incident Response)
Proces triażu incydentów wygenerowanych przez alerty oraz podgląd szczegółów zdarzenia.

* **Główny panel incydentów:**
    ![Strona incydentów](Screenshots/incident_page.jpg)
* **Szczegóły konkretnego incydentu:**
    ![Szczegóły incydentu](Screenshots/incident_2.png)
    ![Analiza incydentu](Screenshots/incident_response.jpg)
* **Obsługa błędów:**
    ![Błędy](Screenshots/error.jpg)

---

## 5. Automatyzacja (SOAR)
Konfiguracja Playbooków w Azure Logic Apps do automatycznej reakcji na incydenty (np. izolacja maszyny, wysłanie powiadomienia, założenie biletu w Jira).

* **Logika Playbooka (Logic App):**
    ![Azure Logic Apps](Screenshots/SOAR_azure_logic_apps.png)
* **Integracja z Jira (Ticket System):**
    ![Bilet w Jira](Screenshots/Jira_Ticket.png)
    *Automatyczne tworzenie zadania dla analityka po wykryciu incydentu o wysokim priorytecie.*

---

## 6. Wizualizacja Danych (Workbooks)
Dashboardy służące do monitorowania stanu bezpieczeństwa w czasie rzeczywistym.

* **Wykresy i trendy:**
    ![Wykresy](Screenshots/graphs.png)
* **Tabela zdarzeń:**
    ![Tabela](Screenshots/tabela.png)
* **Panel główny:**
    ![Panel](Screenshots/panel.jpg)
* **Mostek sieciowy (Bridge):**
    ![Bridge](Screenshots/bridge.jpg)
