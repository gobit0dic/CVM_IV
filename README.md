# CVM_IV E85 - Verdecköffnung während der Fahrt

## Einleitung
Hier findet ihr DATEN Files für das CVM IV des E85, um die Verdecköffnung während der Fahrt zu codieren. ACHTUNG: Bitte denkt an die Stabilität eures Verdecks und nutzt die Funktion nur nach sorgfältigem Nachdenken. Ich nutze die Verdecköffnung nur bis 30km/h. Aufgrund mehrerer Nachfragen sind jedoch Werte bis 50km/h hinterlegt. 

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
Weitere Hintergrundinfos: https://www.zroadster.com/forum/threads/cvm-iv-verdeckoeffnen-waehrend-der-fahrt-ohne-zusatzmodul-ncs-exkurs.142475
