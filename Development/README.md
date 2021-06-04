## Wo lag die Schwierigkeit?
Das CVM nimmt normalerweise keine Werte an. Ändert man mit NCS einfach die Werte, wie z.B. im GM5 oder LSZ, dann bekommt man folgenden Fehler: "COAPI-2060: Codierung Fehlerhaft (allgemein)". Warum passiert das?
Das CVM berechnet pro Block eine Checksumme (Check_Block_x). Diese muss am Ende wieder stimmen, dass er die Kodierung annimmt. Nun sind Checksummen aber nicht genormt, weshalb es dort nicht DAS Verfahren gibt, um sie zu berechnen. Wer mehr wissen möchte, hier entlang: Prüfsumme – Wikipedia

## NCS Grundlagen
Um das tiefer zu verstehen, muss man sich mit NCS etwas mehr auseinander setzen. Die meisten kennen die Möglichkeit mit Text zu codieren. Also z.B. "SIEDEMARKER_US_MIT_FRA_V" von "nicht_aktiv" auf "aktiv" setzen. Aber was passiert da eigentlich?
NCS basiert auf sog. DATEN-Dateien, die wiederum übersetzen von eurem Fließtext in 0 und 1 (Bits & Bytes). Bei "nicht_aktiv" wird meist das Bit auf 0 gesetzt, bei "aktiv" auf 1. Nun kann das Bit aber Werte von 00 bis FF (0 bis 255) annehmen. Somit können bei vielen Werten z.B. mehrere Optionen codiert werden. Tippblinken kann man z.B. auf "wert_01" = 1x, "wert_02" = 3x, usw. codieren. Was hier eigentlich passiert ist, dass in den DATEN gesagt wird: Wenn in der .MAN Datei "wert_02" steht, dann soll am Byte XY der Wert auf 03 gesetzt werden. Dadurch kann man den DATEN Dateien auch neue Werte beibringen (z.B. Tippblinken 7x), weil man einfach nur der DATEN Datei sagen muss, welchen Wert sie im Bit setzen soll.

Soweit, so einfach.
Jetzt sind unsere Steuergeräte aber recht alt und wir reden von einer Zeit als ganze Videospiele noch auf 1MB Kassetten für den Gameboy gepasst haben. Deswegen musste effizient programmiert und der Speicher genutzt werden. Also wurde Bit-Masking verwendet. Kurz gesagt teilen sich hier mehrere Codierungen ein Byte. Mithilfe der Maske werden sie miteinander verknüpft um somit schneller zu rechnen und Speicherplatz zu sparen. Da kommen jetzt auch wieder unser Bitwise Operations ins Spiel. Somit hat z.B. bei "aktiv" der Eintrag den Wert 01, aber gesetzt wird eigentlich 08. Das ist wichtig zu wissen.

## Bitwise WAS?
Kurz gesagt ist das ein bisschen wie Schriftliche Addition. Eine Folge von 0 und 1 werden miteinander kombiniert, um eine neue Folge zu erhalten. Dabei kann man Booleans wie AND, OR oder auch XOR verwenden.
XOR bedeutet, dass die zweite Zahl besagt, ob die 0 oder 1 an der gleichen Stelle der ersten Zahl invertiert werden soll.
Als Beispiel: 0101 XOR 1001:
Position 1: 0 XOR 1 -> Die 0 wird invertiert
Position 2: 1 XOR 0 -> Die 1 wird nicht invertiert
Position 3: 0 XOR 0 -> Die 0 wird nicht invertiert
Position 4: 1 XOR 1 -> Die 1 wird invertiert
Das ist jetzt sehr vereinfacht und Bits liest man eigentlich von rechts nach links, aber ich hoffe das Prinzip wurde klar.

## Zurück zur Checksumme
Das war jetzt ein großer Ausflug in die Informatik. Wie hilft uns das beim Steuergerät?
Wir müssen jeweils für einen Block die Masked Values per XOR verrechnen. Wie macht man das also?

1. Zunächst schauen wir uns in NCSDummy die Masked Values an. Dazu gehen wählen wir Baureihe und Modul aus, dann unten auf Module Functions und auf Export und speichern es als Text-Datei.
2. Nun sehen wir mehrere Spalten. Interessant ist für uns "Function Keyword", "Data", "Masked". Hier wissen wir welche Funktion wir haben wollen, welchen Wert wir brauchen und wie er maskiert ist. z.B. "GESCHW_VERDECK_AKTIV wert_01" | "0A" | "0A". Also wissen wir, dass dieser Wert einen Masked Value von "0A" hat.
3. Das übertragen wir uns alles in eine Tabelle und rechnen die jeweiligen Masked Values per XOR zusammen und zuletzt nochmal invertieren (XOR FF). Dann müssten wir auf den Check_Block Wert kommen.
4. Nun können wir einen Wert ändern z.B. von "0A" auf "32" (10 auf 50). Wir müssen schauen, wie der Masked Value sich ändert. Entweder hat man es im Gefühl oder man fügt den Parameter bei NCS ein (1. Tab, Rechtklick, Add Parameter, Export -> Update Module) und exportiert wieder die Functions und schaut sich dann Wert wieder in der TXT an. Als Faustregel kann man sagen, wenn der Wert die Länge FF hat, dann ist Masked Value = Data Value
5. Jetzt können wir uns wieder die Checksumme, wie in Schritt 3, berechnen und erhalten einen Wert.
6. Diesen Wert fügen wir wieder in NCS Dummy als Parameter ein. Die Anleitung dazu findet ihr hier unter Punkt 3.1.4: NCS Dummy - Taking the expert out of NCS Expert
7. Nun kann man die neu hinzugefügten Werte ganz normal per .MAN Datei in NCS kodieren

Zum Berechnen könnt ihr die Tabelle "Calc_Checksum" verwenden, die ich angehangen habe.


