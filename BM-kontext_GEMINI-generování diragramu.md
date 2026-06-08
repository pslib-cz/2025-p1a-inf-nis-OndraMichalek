# Dokumentace sekvenčních diagramů – Projekt BanManager

Tento dokument shrnuje aktuální stav návrhu dynamického chování informačního systému **BanManager** v rámci školního projektu NIS. Všechny diagramy byly navrženy striktně v UML notaci a vygenerovány jako čisté vektorové grafiky (SVG).

## Obsah a architektura projektu
Systém je rozdělen na tři hlavní logické vrstvy (3-tier architektura):
1. **Frontend:** React aplikace běžící v prohlížeči uživatele.
2. **Backend:** Spring Boot (Java) poskytující REST API.
3. **Herní vrstva & Perzistence:** Minecraft server s vlastním Java pluginem (Paper API) a relační databáze MySQL.

---

## Přehled vytvořených UML sekvenčních diagramů

### 1. Udělení banu ve hře (Zápis dat)
* **Účel:** Mapuje herní akci administrátora a její okamžitou synchronizaci do centrální databáze přes REST API.
* **Životní čáry (Lifelines):** `:Admin` (Aktér), `Minecraft Server (Plugin)`, `BanManager Backend (Spring)`, `MySQL Database`.
* **Klíčové kroky komunikace:**
  1. Admin zadá ve hře příkaz `/ban <hráč> <důvod>`.
  2. Plugin zachytí událost a posílá asynchronní/synchronní HTTP POST požadavek s JSON daty na backend.
  3. Spring Boot backend provede validaci formátu dat a správnosti UUID hráče.
  4. Backend provede databázovou operaci `SQL INSERT INTO bans (...)`.
  5. MySQL potvrdí úspěšný zápis, backend odpoví stavovým kódem `HTTP 201 Created` a plugin vypíše adminovi potvrzení do herního chatu.

### 2. Prohlížení seznamu banů na webu (Čtení dat)
* **Účel:** Vizualizuje, jak běžný návštěvník přistupuje k veřejným datům přes webový prohlížeč. Implementuje výkonnostní požadavek **NF-06** (načtení do 3 sekund).
* **Životní čáry:** `:Návštěvník` (Aktér), `React Frontend`, `BanManager Backend (Spring)`, `MySQL Database`.
* **Klíčové kroky komunikace:**
  1. Uživatel otevře stránku s bany, React Frontend iniciuje požadavek `HTTP GET /api/bans`.
  2. Backend požadavek přijme a skrze Repository vrstvu vyvolá dotaz `SQL SELECT * FROM bans`.
  3. Databáze vrátí sadu řádků, kterou backend namapuje na DTO (Data Transfer Object) a odešle jako JSON v těle odpovědi `HTTP 200 OK`.
  4. React data zpracuje a vykreslí přehlednou tabulku uživateli.

### 3. Podání odvolání (Hlídání integrity dat)
* **Účel:** Popisuje složitější byznys logiku při zakládání ticketu (odvolání). Striktně hlídá nefunkční požadavek **NF-08** (integrita dat – duplicita ticketů k jednomu trestu).
* **Životní čáry:** `:Hráč` (Aktér), `React Frontend`, `BanManager Backend (Spring)`, `MySQL Database`.
* **UML Konstrukce:** Použití robustního fragmentu **Alternatives (`alt` / `else`)** pro větvení logiky.
* **Klíčové kroky komunikace:**
  1. Zabanovaný hráč odešle formulář z webu (`HTTP POST /api/tickets`).
  2. Backend se nejdříve dotáže databáze pomocí `SELECT COUNT(*) WHERE ban_id = ...`.
  3. **Větev alt [count > 0]:** Pokud ticket už existuje, backend vrací chybový kód `HTTP 400 Bad Request` a frontend uživateli zobrazí varování.
  4. **Větev else (úspěch):** Pokud je count = 0, backend v rámci jedné atomické `@Transactional` transakce provede `SQL INSERT` pro novou entitu `Ticket` i úvodní `Message`. Následně vrací `HTTP 201 Created` a frontend uživatele přesměruje do rozhraní ticketu.

### 4. Přihlášení přes Microsoft OAuth2 (Zabezpečení)
* **Účel:** Detailně popisuje implementaci požadavku **NF-02** na bezpečné ověření identity bez nutnosti ukládání uživatelských hesel.
* **Životní čáry:** `:Hráč` (Aktér), `React Frontend`, `BanManager Backend`, `Microsoft OAuth API` (Externí systém), `MySQL DB`.
* **Klíčové kroky komunikace:**
  1. Hráč klikne na webu na přihlášení a je přesměrován na oficiální přihlašovací stránku Microsoftu.
  2. Po úspěšném zadání údajů a schválení práv vrátí Microsoft prohlížeči jednorázový autorizační kód (`code`).
  3. React předá tento kód backendu (`POST /api/auth/callback`).
  4. Backend provede bezpečné **Server-to-Server** volání na Microsoft API, kde kód vymění za Access Token.
  5. Backend si vyžádá profil uživatele z MS (krok `GET /v1.0/me`), získá Minecraft UUID a jméno.
  6. V lokální MySQL databázi proběhne `Upsert` (kontrola/zápis) hráče, backend vygeneruje vlastní aplikační **JWT (JSON Web Token)** a pošle ho frontendu v odpovědi `HTTP 200 OK`. Frontend token bezpečně uloží do `LocalStorage`.

---

## Další kroky v dokumentaci (Strukturální diagramy)
Pro kompletní odevzdání projektu NIS je doporučeno navázat na tyto diagramy vytvořením strukturálních pohledů:
1. **Diagram komponent (Component Diagram):** Zobrazení statických vazeb mezi Reactem, Spring Bootem, MySQL a Mailerem pomocí UML rozhraní (porty/lízátka).
2. **Diagram nasazení (Deployment Diagram):** Fyzické rozdělení systémů (Uživatel ➔ VPS Server s Dockerem pro web/API ➔ Oddělený dedikovaný herní Minecraft Server).

---
*Uloženo: 2026-06-08*