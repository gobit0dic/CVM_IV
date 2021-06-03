# CVM_IV E85 - Verdecköffnung während der Fahrt

## Einleitung
Hier findet ihr DATEN Files für das CVM IV des E85, um die Verdecköffnung während der Fahrt zu codieren. ACHTUNG: Bitte denkt an die Stabilität eures Verdecks und nutzt die Funktion nur nach sorgfältigem Nachdenken. Ich nutze die Verdecköffnung nur bis 30km/h. Aufgrund mehrerer Nachfragen sind jedoch Werte bis 50km/h hinterlegt. 
Das Verdeck öffnet auch weiter, wenn ich die Geschwindigkeit erhöht. Lediglich beim Zeitpunkt des Tastendrucks muss die Geschwindigkeit unterschritten sein.

## Anleitung
1. Schaut euch euren aktuellen Codierindex des CVM an. Diesen findet ihr z.B. über die Informationswerte in INPA.
2. Ladet euch die entsprechenden Datei/Ordner für euer CVM CI herunter. Entpackt die DATEN File nach C:/NCSEXPER/DATEN/E85
3. Nun könnt ihr folgende Codieroptionen in der .MAN Datei nutzen

## Codieroptionen
#### Verdecköffnung bis 20 km/h
GESCHW_VERDECK_AKTIV

kmh_20

GESCHW_PRUEFUNG

nicht_aktiv

CHECK_BLOCK_4

kmh_20


#### Verdecköffnung bis 36 km/h
GESCHW_VERDECK_AKTIV

kmh_36

GESCHW_PRUEFUNG

nicht_aktiv

CHECK_BLOCK_4

kmh_36


#### Verdecköffnung bis 40 km/h
GESCHW_VERDECK_AKTIV

kmh_40

GESCHW_PRUEFUNG

nicht_aktiv

CHECK_BLOCK_4

kmh_40


#### Verdecköffnung bis 50 km/h
GESCHW_VERDECK_AKTIV

kmh_50

GESCHW_PRUEFUNG

nicht_aktiv

CHECK_BLOCK_4

kmh_50


## Weitere Infos
Infos zum Aufbau der Codierung und möglichen Optionen findet ihr im Development Ordner. Dazu einige hilfreiche Dateien.
Weitere Diskussion: https://www.zroadster.com/forum/threads/cvm-iv-verdeckoeffnen-waehrend-der-fahrt-ohne-zusatzmodul-ncs-exkurs.142475


# CVM_IV E85 - Convertible top opening while driving

## Introduction
Here you can find DATEN files for the CVM IV of the E85 to code the top opening while driving. ATTENTION: Please think about the stability of your convertible top and use this function only after careful consideration. I use the top opening only up to 30km/h. However, due to several requests, values up to 50km/h are stored. 
The soft top also continues to open when you increase the speed. Only at the time of the keystroke the speed must be under the limit set.

## Instruction
1. Look at your current coding index of the CVM. You can find it e.g. via the information tab in INPA.
2. Download the corresponding file/folder for your CVM CI. Unpack the DATEN file to C:/NCSEXPER/DATEN/E85.
3. Now you can use the following coding options in your .MAN file

## Coding options
#### Opening up to 20 km/h
GESCHW_VERDECK_AKTIV
kmh_20

GESCHW_PRUEFUNG
nicht_aktiv

CHECK_BLOCK_4
kmh_20

#### Opening up to 36 km/h
GESCHW_VERDECK_AKTIV
kmh_36

GESCHW_PRUEFUNG
nicht_aktiv

CHECK_BLOCK_4
kmh_36

#### Opening up to 40 km/h
GESCHW_VERDECK_AKTIV
kmh_40

GESCHW_PRUEFUNG
nicht_aktiv

CHECK_BLOCK_4
kmh_40

#### Opening up to 50 km/h
GESCHW_VERDECK_AKTIV
kmh_50

GESCHW_PRUEFUNG
nicht_aktiv

CHECK_BLOCK_4
kmh_50

## Further info
Info about the structure of the coding and possible options can be found in the development folder. In addition some helpful files.
Further discussion (in german): https://www.zroadster.com/forum/threads/cvm-iv-verdeckoeffnen-waehrend-der-fahrt-ohne-zusatzmodul-ncs-exkurs.142475

