 ## Where was the difficulty?
The CVM normally does not accept values. If you simply change the values with NCS, as e.g. in GM5 or LSZ, you get the following error: "COAPI-2060: Coding error (general)". Why does this happen?
The CVM calculates a checksum (Check_Block_x) for each block. This must be correct at the end again, that it accepts the coding. But checksums are not standardized, so there is not THE method to calculate them.

## NCS Basics
To understand this more deeply, you need to look into NCS a bit more. Most people know the possibility to code with strings. So e.g. set "SIEDEMARKER_US_MIT_FRA_V" from "not_active" to "active". But what actually happens there?
NCS is based on so-called DATEN files, which in turn translate from your continuous text into 0 and 1 (bits & bytes). With "nicht_aktiv" the bit is usually set to 0, with "aktiv" to 1. But now the bit can take values from 00 to FF (0 to 255). Thus with many values e.g. several options can be coded. Tip flashing can be coded e.g. to "wert_01" = 1x, "wert_02" = 3x, and so on. What actually happens here is that in the DATEN it says: If in the .MAN file "wert_02" is written, then at byte XY the value is to be set to 03. This way you can also teach the DATEN files new values (e.g. typing flash 7x), because you just have to tell the DATEN file which value to set in the bit.

So far, so simple.
But now our controllers are quite old and we are talking about a time when whole video games still fit on 1MB cassettes for the Gameboy. So we had to program efficiently and use the memory. So bit masking was used. In short, several encodings share one byte. With the help of the mask they are linked together to calculate faster and save memory. This is where our bitwise operations come into play again. So for example with "aktiv" the entry has the value 01, but actually set is 08. That is important to know.

## Bitwise WHAT?
In short, it's a bit like written addition. A sequence of 0 and 1 are combined to get a new sequence. Booleans like AND, OR or even XOR can be used.
XOR means that the second number tells whether the 0 or 1 in the same place of the first number should be inverted.
As an example: 0101 XOR 1001:
Position 1: 0 XOR 1 -> The 0 is inverted
Position 2: 1 XOR 0 -> The 1 is not inverted
Position 3: 0 XOR 0 -> The 0 is not inverted
Position 4: 1 XOR 1 -> The 1 is inverted
This is very simplified now and bits are actually read from right to left, but I hope the principle became clear.

## Back to the checksum
That was a big excursion into computer science. How does this help us with the ECU?
We have to XOR the masked values for one block at a time. So how do we do that?

1. First, we look at the masked values in NCSDummy. To do this, we select Series and Module, then Module Functions at the bottom and Export and save it as a text file. 
2. Now we see several columns. Interesting for us is "Function Keyword", "Data", "Masked". Here we know which function we want to have, which value we need and how it's masked. e.g. "GESCHW_VERDECK_AKTIV wert_01" | "0A" | "0A". So we know that this value has a masked value of "0A".
3. We transfer all this into a table and add up the respective masked values by XOR and finally invert them again (XOR FF). Then we should get the Check_Block value.
4. Now we can change a value e.g. from "0A" to "32" (10 to 50). We have to see how the Masked Value changes. Either you have a feeling for it or you insert the parameter at NCS (1st tab, right click, Add Parameter, Export -> Update Module) and export the Functions again and then look at the value again in the TXT. As a rule of thumb you can say, if the value has the length FF, then Masked Value = Data Value.
5. Next, can calculate the checksum again like in step 3 and get a value.
6. We insert this value again into NCS Dummy as parameter. You can find the instructions here at point 3.1.4: https://www.bimmerforums.com/forum/showthread.php?1553779-NCS-Dummy-Taking-the-expert-out-of-NCS-Expert.
7. Now you can code the newly added values normally via .MAN file in NCS.

For calculation you can use the table "Calc_Checksum" which I have attached.

 --- 

## Wo lag die Schwierigkeit?
Das CVM nimmt normalerweise keine Werte an. Ändert man mit NCS einfach die Werte, wie z.B. im GM5 oder LSZ, dann bekommt man folgenden Fehler: "COAPI-2060: Codierung Fehlerhaft (allgemein)". Warum passiert das?
Das CVM berechnet pro Block eine Checksumme (Check_Block_x). Diese muss am Ende wieder stimmen, dass er die Kodierung annimmt. Nun sind Checksummen aber nicht genormt, weshalb es dort nicht DAS Verfahren gibt, um sie zu berechnen. 

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
6. Diesen Wert fügen wir wieder in NCS Dummy als Parameter ein. Die Anleitung dazu findet ihr hier unter Punkt 3.1.4:https://www.bimmerforums.com/forum/showthread.php?1553779-NCS-Dummy-Taking-the-expert-out-of-NCS-Expert
7. Nun kann man die neu hinzugefügten Werte ganz normal per .MAN Datei in NCS kodieren

Zum Berechnen könnt ihr die Tabelle "Calc_Checksum" verwenden, die ich angehangen habe.


