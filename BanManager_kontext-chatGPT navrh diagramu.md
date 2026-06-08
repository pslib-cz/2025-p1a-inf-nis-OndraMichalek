# Kontext projektu BanManager

## Zadání
Projekt NIS – návrh a dokumentace informačního systému.

Termíny:
- Koncept projektu (README): do 27.5.
- Katalog požadavků: do 3.6.
- Diagramy: do 10.6.

## Projekt
Název systému: BanManager

Účel:
- Evidence banů na Minecraft serveru.
- Synchronizace banů z Minecraft pluginu.
- Veřejný seznam banů.
- Odvolání proti banům pomocí ticketů.
- Administrace ticketů.
- Auditní log administrátorských akcí.

## Použité technologie
- Minecraft Plugin: Java + Paper API
- Backend: Spring Boot
- Databáze: MySQL
- Frontend: React
- Komunikace: REST API

## Role
1. Návštěvník
2. Hráč
3. Administrátor

## Doporučené diagramy
1. UML Use Case Diagram
2. UML Class Diagram (ER Diagram)
3. UML Sequence Diagram
4. UML Deployment Diagram
5. UML Component Diagram

## Poznámky z konzultace
- Class Diagram bude pravděpodobně nejdůležitější diagram.
- Sequence Diagram dobře ukazuje komunikaci mezi frontendem, backendem a databází.
- DFD může ukazovat tok dat mezi pluginem, backendem a databází.
- Diagramy mají odpovídat UML notaci.
- Požadavek je možnost generovat výsledné diagramy přímo jako SVG soubory.

## Aktuální stav
- Katalog požadavků je hotový.
- Dalším krokem bude generování vybraných UML diagramů ve formátu SVG.
