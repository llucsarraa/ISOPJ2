---
layout: default
title: "Sprint 2: Instal·lació, configuració de programari de base i gestió de fitxers"
---

# Etapa 1: Configuració prèvia de l'entorn

## 1.1. Incorporació d'una unitat d'emmagatzematge addicional

El primer pas consisteix a accedir al menú de configuració de **VirtualBox** per integrar un nou suport de disc virtual (format **VDI**) a la nostra estació de treball. Durant aquest procés, cal definir les següents especificacions:

* **Tipus de reserva:** S'ha d'optar per l'assignació **dinàmica** per optimitzar l'espai físic del host.
* **Capacitat:** Cal seleccionar el volum de gigabytes necessari segons els requeriments del projecte.

<img width="787" height="436" alt="image" src="https://github.com/user-attachments/assets/874712d7-127e-4e40-a761-5c28a2fe292e" />

## 1.2. Execució de Windows i accés a l'Administrador de discs

Un cop engegada la màquina virtual amb Windows, cal accedir a la utilitat de gestió d'emmagatzematge. Per fer-ho, podem fer servir la drecera `Win + X` o escriure directament `diskmgmt.msc` a la consola d'execució.

En entrar-hi, observarem que la unitat que acabem de crear es mostra amb l'estat **"Sense inicialitzar"**, indicant que el sistema encara no l'ha preparat per al seu ús.

<img width="1027" height="622" alt="image" src="https://github.com/user-attachments/assets/802f542a-6d6f-48e3-832e-3984d925520d" />

## 1.3. Configuració de la unitat i creació de volums

Per començar a utilitzar el dispositiu, cal fer clic dret sobre la unitat marcada i seleccionar l'opció **"Inicialitzar disc"**, assegurant-nos d'escollir l'estil de partició **GPT**. Un cop el disc estigui operatiu, procedirem a dividir-lo en dues seccions diferenciades:

1.  **Volum "Dades":** Es configurarà amb el sistema de fitxers **NTFS**, ja que aquest format és imprescindible per a la implementació de quotes de disc i el control d'accés mitjançant llistes (ACL).
2.  **Volum "Portable":** Es formatarà en **FAT32**, garantint així la màxima compatibilitat amb diversos entorns i sistemes operatius.

<img width="659" height="350" alt="image" src="https://github.com/user-attachments/assets/9acb0539-b121-49e5-b5d2-0bdd38135eb5" />

<img width="497" height="395" alt="image" src="https://github.com/user-attachments/assets/a569c63b-e10d-47b2-82ab-1548924e03bf" />

<img width="502" height="395" alt="image" src="https://github.com/user-attachments/assets/a07c9d8f-e1fb-473f-87ae-bacd9719383d" />

<img width="495" height="394" alt="image" src="https://github.com/user-attachments/assets/b0ab648b-d96f-4baa-ae5a-dfcb9d598f9c" />

<img width="675" height="221" alt="image" src="https://github.com/user-attachments/assets/3552e623-d355-4c3a-9eb1-25e96c1b8802" />

<img width="500" height="393" alt="image" src="https://github.com/user-attachments/assets/4670e165-4394-48e4-9057-3653c0432763" />

<img width="500" height="402" alt="image" src="https://github.com/user-attachments/assets/a80bae12-370b-427d-87b7-27b23566dcc3" />

<img width="627" height="114" alt="image" src="https://github.com/user-attachments/assets/6134b773-4e83-4814-a827-699f67bf129d" />

## 1.4. Identificació d'unitats i validació mitjançant Diskpart

Un cop definides les particions, s'han de vincular a una lletra d'unitat específica (per exemple, assignarem la **D:** per al volum de Dades i la **E:** per al Portable). 

Per assegurar-nos que tot s'ha configurat correctament, realitzarem una comprovació tècnica seguint aquests passos:
1. Executar el terminal (**CMD**) amb permisos de superusuari (Administrador).
2. Accedir a l'eina de gestió de línia de ordres teclejant `diskpart`.
3. Utilitzar la comanda `list volume` per inspeccionar el llistat final i verificar que les lletres, les etiquetes i els formats (NTFS/FAT32) coincideixen amb el previst.

<img width="756" height="504" alt="image" src="https://github.com/user-attachments/assets/ab056533-9aaa-486f-9965-736346c10bd7" />

<img width="689" height="244" alt="image" src="https://github.com/user-attachments/assets/25aa6e6d-6439-4b50-b93d-854d82f3e1ff" />

# Etapa 2: Gestió d'usuaris i restriccions d'emmagatzematge

## 2.1. Habilitació de quotes de disc al volum Dades

Per controlar l'espai utilitzat a la partició NTFS (unitat D:), cal configurar els límits d'ús seguint aquest procediment:

1.  **Accés a la configuració:** Entreu a les "Propietats" de la unitat i dirigiu-vos a la secció de **"Quota"**.
2.  **Activació del servei:** Seleccioneu l'opció per posar en marxa l'administració de quotes.
3.  **Control de restriccions:** Cal activar la funció que impedeix l'escriptura de dades addicionals a aquells usuaris que hagin sobrepassat el llindar de capacitat establert.

<img width="783" height="593" alt="image" src="https://github.com/user-attachments/assets/fa41c95c-d93e-4c70-af50-96dc3c3bf836" />

<img width="764" height="534" alt="image" src="https://github.com/user-attachments/assets/8b441fd8-ce9a-4d0f-9db3-0f57b174179a" />

<img width="367" height="447" alt="image" src="https://github.com/user-attachments/assets/61276007-0e57-4660-9e0f-71921330c745" />

## 2.2. Definició de llindars i avisos de capacitat

Dins del mateix panell de control de quotes, procedirem a parametritzar les restriccions d'espai per a cada compte d'usuari:

* **Sostre d'emmagatzematge:** Es fixa un topall màxim de **300 MB**. Un cop assolit, el sistema bloquejarà qualsevol intent de desar més informació a la unitat D:.
* **Sistema de preavís:** Es configurarà un nivell d'advertència previ perquè l'usuari rebi una notificació abans d'arribar al límit crític.

D'aquesta manera, garantim que cap membre de l'equip pugui monopolitzar l'espai de la partició destinada a dades.

<img width="360" height="410" alt="image" src="https://github.com/user-attachments/assets/7dbe60c8-8fc4-4f00-a4ff-5cd75295e793" />

## 2.3. Alta de comptes locals: alumne1 i alumne2

El següent procediment consisteix a registrar nous perfils d'accés al sistema per comprovar el funcionament de les restriccions:

* **Creació de comptes:** Cal donar d'alta dos usuaris nous en l'àmbit local del sistema operatiu.
* **Identificadors:** Els noms escollits per a aquests perfils seran `alumne1` i `alumne2`.

Aquesta acció ens permetrà validar posteriorment que les quotes de disc s'apliquen de manera individualitzada a cada un d'ells.

<img width="812" height="595" alt="image" src="https://github.com/user-attachments/assets/e920edfa-548d-45da-816f-e5ed28d1f061" />

<img width="476" height="286" alt="image" src="https://github.com/user-attachments/assets/9a3948a0-b3df-4a65-9499-34a1b459d696" />

<img width="659" height="637" alt="image" src="https://github.com/user-attachments/assets/38bab6f3-b64c-4ead-98aa-26c554f3288a" />

<img width="634" height="622" alt="image" src="https://github.com/user-attachments/assets/6f20a790-7455-4f7a-8e22-d124be2425c1" />

<img width="651" height="633" alt="image" src="https://github.com/user-attachments/assets/1c48453b-1299-4726-9311-ab6f1f49dd49" />



























