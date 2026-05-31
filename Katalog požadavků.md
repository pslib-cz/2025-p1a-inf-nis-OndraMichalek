# Katalog požadavků projektu BanManager

## 1. Úvod

### 1.1 Název systému

**BanManager**

### 1.2 Účel systému

BanManager je informační systém určený pro správu a evidenci banů na Minecraft serveru. Systém je propojen s Minecraft serverem pomocí pluginu, který zaznamenává udělené bany do databáze.

Uživatelé mohou prostřednictvím webového rozhraní prohlížet evidované bany a po přihlášení pomocí Microsoft účtu komunikovat s administrátory serveru prostřednictvím ticketů. Systém slouží ke zvýšení transparentnosti udělených trestů a zjednodušení komunikace mezi hráči a vedením serveru.

---

# 2. Role uživatelů

## 2.1 Návštěvník

Nepřihlášený uživatel.

### Oprávnění

* Prohlížet seznam banů.
* Vyhledávat bany.
* Zobrazovat detail banu.

---

## 2.2 Hráč

Uživatel přihlášený pomocí Microsoft účtu propojeného s Minecraft účtem.

### Oprávnění

* Prohlížet seznam banů.
* Vyhledávat bany.
* Zobrazovat detail banu.
* Vytvářet tickety ke svému aktivnímu banu.
* Komunikovat s administrátory v rámci ticketu.
* Prohlížet vlastní tickety.

### Omezení

* Ticket lze vytvořit pouze k vlastnímu banu.
* Ticket lze vytvořit pouze k aktivnímu banu.
* Na jeden ban lze vytvořit pouze jeden ticket.
* Pokud Microsoft účet není propojen s Minecraft účtem, nelze vytvořit ticket.

---

## 2.3 Administrátor

Člen realizačního týmu serveru.

### Oprávnění

* Prohlížet všechny bany.
* Spravovat tickety.
* Odpovídat v ticketech.
* Přijímat nebo zamítat odvolání.
* Uzavírat tickety.
* Prohlížet auditní záznamy.

---

# 3. Funkční požadavky

## 3.1 Evidence banů

### FP-01

Systém musí evidovat permanentní bany.

### FP-02

Systém musí evidovat dočasné bany.

### FP-03

Systém musí evidovat IP bany.

### FP-04

Systém musí automaticky načítat bany z externího administračního pluginu serveru.

### FP-05

Systém musí ukládat následující informace o banu:

* Minecraft nick hráče
* IP adresu hráče
* UUID hráče (viditelné pouze pro administrátory)
* důvod banu
* administrátora, který ban udělil
* datum udělení
* datum vypršení
* stav banu

### FP-06

Systém musí rozlišovat stavy banu:

* aktivní
* vypršelý
* ručně zrušený

### FP-07

IP adresa hráče nesmí být veřejně zobrazována.

---

## 3.2 Veřejný seznam banů

### FP-08

Systém musí zobrazovat seznam všech evidovaných banů.

### FP-09

Systém musí umožňovat vyhledávání podle hráčského nicku.

### FP-10

Systém musí umožňovat zobrazení detailu banu.

### FP-11

Detail banu musí obsahovat:

* nick hráče
* důvod banu
* administrátora
* datum udělení
* datum vypršení
* aktuální stav

---

## 3.3 Přihlášení

### FP-12

Systém musí umožňovat přihlášení pomocí Microsoft OAuth.

### FP-13

Po přihlášení musí systém ověřit propojení Microsoft účtu s Minecraft účtem.

### FP-14

Systém musí ukládat základní informace o přihlášeném uživateli.

---

## 3.4 Tickety

### FP-15

Přihlášený hráč musí mít možnost vytvořit ticket ke svému aktivnímu banu.

### FP-16

Ticket nelze vytvořit k vypršelému banu.

### FP-17

Ticket nelze vytvořit ke zrušenému banu.

### FP-18

Na jeden ban lze vytvořit pouze jeden ticket.

### FP-19

Ticket musí obsahovat:

* autora ticketu
* související ban
* datum vytvoření
* zprávy komunikace
* aktuální stav

### FP-20

Administrátor musí mít možnost odpovídat v ticketu.

### FP-21

Hráč musí mít možnost odpovídat v ticketu.

### FP-22

Administrátor musí mít možnost ticket uzavřít.

### FP-23

Administrátor musí mít možnost schválit odvolání.

### FP-24

Administrátor musí mít možnost zamítnout odvolání.

---

## 3.5 E-mailové notifikace

### FP-25

Systém musí zaslat e-mail při vytvoření ticketu.

### FP-26

Systém musí zaslat e-mail při nové odpovědi v ticketu.

### FP-27

Systém musí zaslat e-mail při uzavření ticketu.

---

## 3.6 Auditní log

### FP-28

Systém musí zaznamenávat administrátorské akce.

### FP-29

Auditní log musí obsahovat:

* administrátora
* čas akce
* typ akce
* související objekt

### FP-30

Auditní log musí být dostupný pouze administrátorům.

### FP-31

Administrátor z Auditního logu nemůže mazat akce.

---

# 4. Nefunkční požadavky

## NF-01 Bezpečnost

Komunikace mezi klientem a serverem musí probíhat prostřednictvím HTTPS.

## NF-02 Ověření identity

Přihlášení musí využívat Microsoft OAuth.

## NF-03 Ochrana osobních údajů

IP adresy nesmí být veřejně zobrazovány.

## NF-04 Dostupnost

Systém by měl být dostupný 24 hodin denně, 7 dní v týdnu.

## NF-05 Responzivita

Webové rozhraní musí být použitelné na mobilních zařízeních i počítačích.

## NF-06 Výkon

Zobrazení seznamu banů musí proběhnout do 3 sekund při běžném zatížení.

## NF-07 Zálohování

Databáze musí být pravidelně zálohována.

## NF-08 Integrita dat

Systém nesmí umožnit vytvoření více ticketů ke stejnému banu.

## NF-09 Auditovatelnost

Veškeré administrátorské zásahy musí být dohledatelné v auditním logu.

---

# 5. Technologické požadavky

## TP-01 Minecraft plugin

Plugin musí být vytvořen v jazyce Java s využitím Paper API.

## TP-02 Databáze

Systém musí využívat databázi MySQL.

## TP-03 Backend

Serverová část systému bude implementována pomocí frameworku Spring Boot.

## TP-04 Frontend

Webové rozhraní bude implementováno pomocí frameworku React.

## TP-05 API

Komunikace mezi frontendem, backendem a pluginem musí probíhat prostřednictvím REST API.
