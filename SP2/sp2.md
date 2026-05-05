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

<img width="793" height="554" alt="image" src="https://github.com/user-attachments/assets/a3caa9fb-dbda-4e2c-8d3f-c8f5124e3c8e" />

<img width="543" height="312" alt="image" src="https://github.com/user-attachments/assets/f4c9614f-8055-47ca-b660-96868c7fa685" />

Pas 9. Provar la còpia de fitxers dins Dades per veure com actuen les quotes (superar límit)
S'inicia sessió com alumne1 i es copien fitxers grans a D:. Quan s'arriba als 300 MB, Windows bloqueja la còpia i mostra un error d'espai insuficient, demostrant que les quotes funcionen correctament.

<img width="1025" height="755" alt="image" src="https://github.com/user-attachments/assets/70adb627-f4d3-4c39-b35c-51b0cbc8b94f" />

<img width="608" height="369" alt="image" src="https://github.com/user-attachments/assets/60be5025-168d-4747-8d2e-e8b0b15f1eb9" />

<img width="250" height="73" alt="image" src="https://github.com/user-attachments/assets/09201c42-93e8-4b23-8cea-234ccad735f3" />

<img width="197" height="97" alt="image" src="https://github.com/user-attachments/assets/f0bfc697-7f19-44b5-923e-ed5939bdcb58" />

# Etapa 3: Automatització i gestió de rèpliques

## 3.1. Integració d'una unitat de salvaguarda (Backups)

Per tal de preparar l'entorn per a les tasques d'automatització, cal afegir un nou suport d'emmagatzematge seguint aquests passos:

1.  **Configuració de l'hipervisor:** Des de la interfície de **VirtualBox**, s'annexa un tercer disc virtual a la màquina existent.
2.  **Preparació del volum:** Un cop dins del sistema operatiu Windows, es procedeix a la inicialització de la unitat.
3.  **Sistema de fitxers:** Es defineix el format **NTFS** per a aquest disc i se li assigna el nom descriptiu **"Backups"** per identificar-lo fàcilment com a destí de les còpies de seguretat.

<img width="794" height="496" alt="image" src="https://github.com/user-attachments/assets/ae9526f5-6f0a-4e24-a504-279b7927a35f" />

<img width="496" height="395" alt="image" src="https://github.com/user-attachments/assets/c42d1e86-a867-4a72-9c92-d39aa7d8968c" />

## 3.2. Creació del directori de destí "CòpiesUsuaris"

Dins de la nova unitat de salvaguarda (identificada normalment com a **E:**), cal procedir a la creació d'una carpeta específica anomenada `CòpiesUsuaris`. 

Aquest directori actuarà com el repositori centralitzat on s'emmagatzemaran totes les rèpliques de dades generades pels processos d'automatització posteriors.

<img width="550" height="102" alt="image" src="https://github.com/user-attachments/assets/52f59004-61fe-4351-88c5-701e6245e936" />

## 3.3. Desenvolupament de l'script d'automatització (.bat)

Amb l'objectiu de simplificar el procés de salvaguarda, cal programar un fitxer per lots que realitzi la transferència de dades de manera automàtica. L'script ha de seguir aquesta lògica:

* **Origen:** El directori de l'usuari actual mitjançant la variable de sistema `%USERNAME%` (normalment situat a `C:\Users`).
* **Destí:** La ruta específica de seguretat a la unitat de backups: `E:\CòpiesUsuaris\%USERNAME%`.

El codi font ha d'utilitzar comandes de còpia (com `xcopy` o `robocopy`) per assegurar que el contingut de la sessió activa es replica correctament a la carpeta de destí assignada a cada alumne.

xcopy "C:\Users\%USERNAME%" "E:\CòpiesUsuaris\%USERNAME%" /E /I /Y

Això copia tot el perfil de l'usuari al disc de Backups, creant la subcarpeta si no existeix (/I), de forma recursiva (/E) i sobreescrivint (/Y).

<img width="593" height="63" alt="image" src="https://github.com/user-attachments/assets/1fc5349e-6737-4292-98f0-0d6bd337c72a" />

## 3.4. Implementació de la política de grup per a l'execució de l'script

Per aconseguir que el fitxer de salvaguarda s'executi automàticament, cal configurar una directiva local mitjançant els següents passos:

1.  **Accés a l'editor:** Executeu la consola de directives de grup local utilitzant la comanda `gpedit.msc`.
2.  **Ruta de configuració:** Dins de l'arbre de navegació, cal desplaçar-se per la següent ruta:
    * **Configuració de l'usuari**
    * **Configuració de Windows**
    * **Scripts (inici o tancament de sessió)**
3.  **Selecció del disparador:** Feu doble clic sobre l'opció **"Inici de sessió"** per obrir la finestra on vincularem el nostre fitxer per lots.

<img width="687" height="305" alt="image" src="https://github.com/user-attachments/assets/d3e3bf6a-e458-4cce-b90c-3759b3089699" />

## 3.5. Vinculació i automatització del procés de backup

L'últim pas per completar el flux de treball consisteix a assignar el fitxer executable perquè s'activi de forma autònoma. El procediment és el següent:

* **Assignació de l'arxiu:** Mitjançant el botó **"Afegir"**, seleccionarem l'script `.bat` que hem desenvolupat prèviament.
* **Funcionament operatiu:** Un cop configurat, el sistema dispararà el procés de duplicació cada vegada que l'usuari `alumne1` o `alumne2` accedeixi al seu perfil. 

D'aquesta manera, ens assegurem que tota la informació personal es replica a la unitat **Backups** de manera transparent i sense intervenció manual en cada inici de sessió.

<img width="492" height="456" alt="image" src="https://github.com/user-attachments/assets/e13d9e48-e8fb-4065-810e-eead478f4780" />

<img width="388" height="332" alt="image" src="https://github.com/user-attachments/assets/da5752f5-6a43-4f3c-b81a-0f5ab5c34f86" />

# Etapa 4: Validació final i control de qualitat

## 4.1. Proves de funcionament integral del sistema

Per donar per finalitzada la configuració, cal realitzar una bateria de tests des del perfil de l'usuari `alumne1`. Durant aquesta verificació, s'han de confirmar els següents punts clau:

* **Execució de la salvaguarda:** Comprovar que, en entrar al sistema, l'script ha generat correctament el directori `alumne1` dins de la ruta `E:\CòpiesUsuaris`, confirmant la replicació de les dades.
* **Eficàcia de les restriccions:** Validar que el límit d'emmagatzematge continua actiu a la unitat **D:** i que el sistema denega qualsevol intent de sobrepassar el llindar de **300 MB**.
* **Integritat del conjunt:** Inspeccionar que tots els elements configurats (usuaris, grups, particions i automatismes) operen de manera sincronitzada i sense errors.

Aquesta darrera inspecció garanteix que la infraestructura compleix amb tots els requisits d'administració i seguretat establerts.

<img width="849" height="557" alt="image" src="https://github.com/user-attachments/assets/a8ce2b52-5d9b-4e18-9076-2c4d855b07bb" />

# Etapa 5: Administració de serveis i activitat del sistema

## 5.1. Inventari de l'activitat de l'usuari en temps real

Per analitzar quines tasques s'estan executant en segon pla, procedirem a fer una captura de l'estat del sistema sota el perfil d'`alumne1`:

1.  **Accés a la consola:** Un cop dins de la sessió de l'usuari, obrirem el terminal d'ordres (**CMD**).
2.  **Consulta de processos:** Utilitzarem la comanda `tasklist` per visualitzar el llistat complet de programes en execució.
3.  **Exportació de dades:** Per documentar l'estat inicial, bolcarem la informació generada en un fitxer de text mitjançant l'ordre:
    `tasklist > C:\Users\%USERNAME%\processos_inici.txt`
4.  **Anàlisi de resultats:** Identificarem elements essencials de l'entorn de Windows, com ara l'explorador de fitxers (`explorer.exe`), el servei d'indexació (`SearchIndexer.exe`) o el client d'emmagatzematge al núvol (`OneDrive.exe`).

<img width="769" height="675" alt="image" src="https://github.com/user-attachments/assets/febe99d9-c54b-47ba-bb0e-f22552c0df02" />

<img width="664" height="90" alt="image" src="https://github.com/user-attachments/assets/3882fed0-9ef9-48ec-971c-acb3c032a9c6" />

<img width="259" height="510" alt="image" src="https://github.com/user-attachments/assets/cfc7fe42-4f24-442c-a010-7781986bcdde" />

<img width="755" height="199" alt="image" src="https://github.com/user-attachments/assets/5daad51c-242c-4e26-ac7d-7b0fb4d52d10" />

## 5.2. Diagnòstic i selecció de processos no crítics

L'objectiu d'aquest punt és detectar aquelles tasques que consumeixen recursos sense ser vitals per al funcionament bàsic del sistema (com ara `OneDrive.exe`, `Teams.exe` o `SkypeApp.exe`). 

Per organitzar aquesta informació, s'ha de confeccionar una comparativa tècnica detallada:

| Identificador de la tasca | Consum de memòria (RAM) | Motiu de la seva desactivació |
| :--- | :--- | :--- |
| *Nom del procés* | *KB / MB utilitzats* | *Argument tècnic per prescindir-ne* |

Aquesta anàlisi ens permetrà optimitzar el rendiment de la màquina virtual eliminant càrrega de treball innecessària.

<img width="704" height="104" alt="image" src="https://github.com/user-attachments/assets/c9f3ad3b-b3f4-494a-ab60-8ef18fd39aed" />

| Nom del procés | Memòria usada | Justificació per eliminar-lo |
| :--- | :--- | :--- |
| OneDrive.exe | 137.412 KB | Aturar la sincronització d'arxius en segon pla per estalviar amplada de banda d'internet, reduir l'ús de CPU/Disc o alliberar memòria RAM. |

## 5.3. Finalització forçada de tasques i validació de resultats

En aquest apartat, procedirem a tancar manualment els serveis identificats prèviament com a secundaris per alliberar càrrega del sistema. El procediment a seguir és:

1. **Aturada del procés:** Des de la línia de comandes, s'utilitzarà la instrucció `taskkill` amb els paràmetres d'imatge (`/IM`) i forçat (`/F`). Per exemple:
   `taskkill /IM OneDrive.exe /F`

2. **Auditoria de l'acció:** Cal executar novament la comanda `tasklist` per corroborar que el procés ha estat efectivament expulsat de la memòria activa.

> **Nota de documentació:** Per demostrar l'èxit de l'operació, és necessari incloure evidències gràfiques (captures de pantalla) que mostrin l'estat del llistat de tasques abans i després de l'execució de l'ordre.

<img width="730" height="46" alt="image" src="https://github.com/user-attachments/assets/f79e5b66-c6db-4f24-95b3-63ee4bae386e" />

<img width="573" height="125" alt="image" src="https://github.com/user-attachments/assets/db5eaf9c-1cd0-43a4-8628-f318aebe476d" />

<img width="600" height="93" alt="image" src="https://github.com/user-attachments/assets/92223b68-5a94-4259-acba-4235fa45d76e" />

<img width="467" height="38" alt="image" src="https://github.com/user-attachments/assets/7b2f0154-0f8c-4801-8c8f-4c3b831fafcf" />

# Etapa 5: Anàlisi de resultats i optimització del rendiment

## 5.5. Reflexió sobre la gestió de processos

Després d'executar les tasques de l'inventari (`processos_inici.txt`) i d'haver realitzat la neteja de serveis prescindibles com `OneDrive.exe` o `Discord.exe`, cal entendre l'impacte de les nostres accions sobre el nucli del sistema.

### L'impacte de finalitzar l'explorador (`explorer.exe`)
Tot i que pugui semblar una acció crítica, l'aturada del procés `explorer.exe` només afecta la capa d'interfície d'usuari:

* **Simptomatologia:** Desapareixen visualment la barra de tasques, el menú d'inici i les icones. El sistema sembla "buit", però segueix operatiu.
* **Resiliència:** Les aplicacions obertes continuen funcionant. Es pot recuperar l'entorn fàcilment des del **Gestor de Tasques** (`Ctrl+Shift+Esc`) executant una "Nova tasca" amb el nom del procés.
* **Seguretat:** A diferència dels processos del kernel (`csrss.exe` o `wininit.exe`), que provocarien un error de sistema crític (BSOD), el tancament de l'explorador és una prova segura i reversible.

### Beneficis de l'optimització proactiva
L'administració de processos, especialment mitjançant l'automatització configurada al **Pas 22**, aporta avantatges directes:

1.  **Eficiència de memòria:** Alliberar RAM és vital en màquines virtuals amb recursos ajustats.
2.  **Alliberament de CPU i I/O:** Evitem que serveis de sincronització o indexació consumeixin cicles de processador i operacions de lectura/escriptura constants al disc.
3.  **Agilitat operativa:** Reduïm el temps d'espera des que l'usuari inicia sessió fins que el sistema és realment productiu.

---

# Etapa 6: Seguretat i control d'accés (ACLs)

## 6.1. Fonaments de les Llistes de Control d'Accés

A Windows, la seguretat dels fitxers descansa sobre el sistema **NTFS** i les seves **ACL** (*Access Control Lists*). Aquestes llistes permeten definir amb precisió qui té dret a interactuar amb cada recurs.

### Conceptes clau del model de seguretat
Per gestionar correctament els permisos, cal diferenciar els següents elements:

| Concepte | Descripció |
| :--- | :--- |
| **DACL** | Llista que determina quins usuaris tenen accés i amb quins permisos. |
| **SACL** | Llista dedicada a l'auditoria (registra qui ha intentat accedir a un fitxer). |
| **ACE** | Cada entrada individual d'una ACL que vincula un usuari amb una acció (Permetre/Denegar). |

### Nivells de granularitat i herència
Windows ofereix una jerarquia de permisos que facilita l'administració a gran escala:

* **Permisos Estàndard:** Els rols comuns com Lectura (R), Modificació (M) o Control Total (F).
* **Permisos Avançats:** Un conjunt de 14 permisos detallats (com "Prendre possessió" o "Travessar carpetes").
* **L'Herència:** Els objectes fills adopten per defecte les regles del directori pare, tot i que aquesta propietat es pot tallar per crear excepcions específiques.

### Ordre de prioritats (Lògica de Windows)
És fonamental recordar que Windows avalua les regles seguint un ordre jeràrquic estricte:

> **Denegar (Deny) > Permetre (Allow)**

Una denegació explícita sempre s'imposarà a qualsevol permís heretat o de grup. Si no hi ha cap regla que autoritzi l'accés, el sistema el bloqueja per defecte.

## 6.2. Eines de gestió de permisos

Depenent de l'entorn (gràfic o línia de comandes), utilitzarem diferents eines:

* **Interfície Gràfica:** Pestanya "Seguretat" i opcions "Avançades" dins de les Propietats de la carpeta.
* **Consola (CMD):** L'ordre `icacls` és l'estàndard modern per consultar i modificar permisos de forma massiva.
* **PowerShell:** Comandes `Get-Acl` i `Set-Acl` per a scripts complexos.

> [!IMPORTANT]
> **Nota Final:** Prioritzarem sempre les **ACLs** sobre els permisos de compartició, ja que les primeres protegeixen el recurs tant si s'hi accedeix de forma local com remota.

### Pas 24. Implementació del directori de treball

Per iniciar la configuració dels controls d'accés, cal establir l'estructura de fitxers des d'un compte amb privilegis elevats:

1.  **Accés de gestió:** Iniciem sessió al sistema amb el rol d'**Administrador**.
2.  **Creació del recurs:** Generem una nova carpeta anomenada `Projectes` directament a l'arrel de la unitat **D:**.

Aquest directori servirà com a laboratori per a les proves de permisos granulars i herència que realitzarem a continuació.

<img width="406" height="177" alt="image" src="https://github.com/user-attachments/assets/fb4fb14a-4e23-4b00-a340-a39e08d5ab83" />

### Pas 25. Configuració de privilegis del grup i ruptura d'herència

Per garantir que només els usuaris autoritzats puguin gestionar el contingut de `D:\Projectes`, procedirem a restringir l'accés i definir permisos explícits:

1.  **Gestió de seguretat:** Accedim a les propietats de la carpeta `Projectes` i ens situem a la pestanya **Seguretat**.
2.  **Ruptura de l'herència:** Dins de les "Opcions avançades", desactivarem l'herència de permisos. Seleccionarem l'opció de **convertir/conservar els permisos existents** per mantenir el control manual sobre el recurs.
3.  **Purga d'accessos genèrics:** Eliminarem qualsevol entrada d'usuaris genèrics com `Users` o `Everyone` (Tothom) per evitar accessos no desitjats.
4.  **Assignació de control:** Afegirem el grup **Limitats** a la llista i li concedirem el nivell de **Control total**. 

Finalment, aplicarem els canvis per assegurar que aquestes regles es propaguin correctament a tot el directori.

<img width="370" height="486" alt="image" src="https://github.com/user-attachments/assets/3f1d657e-1ef1-45db-9120-9ec69d07a2d2" />

<img width="776" height="521" alt="image" src="https://github.com/user-attachments/assets/052d0e12-d968-42dd-a2e2-eca59a73eda1" />

<img width="715" height="383" alt="image" src="https://github.com/user-attachments/assets/52aca178-fc06-49ef-8189-74788bdc3c77" />

<img width="596" height="125" alt="image" src="https://github.com/user-attachments/assets/7d6f6776-eb38-486d-81e6-4a51a979a724" />










