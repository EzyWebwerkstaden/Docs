Om det är så att man inte får svar från hotelservice på nån ny stad man har lagt upp, eller om man får svar och dessa sorteras bort, så är det med all sanolikhet fel i mappningen i ezyhotels.

Om staden finns med i ezyhotels, så finns den med i items tabellen. Kan vara fel mappad i det fallet. Eller så är det nedanstående SP som gör så att det blir fel.

Kontrollera detta vid ett sådant fall.
Kolla i items att staden finns och att den ligger i rätt heirarki. Dvs tex. Usa/Nevada/Las vegas.
Om den inte finns där, lägger man till den i items. Kopierar itemid och lägger sedan till den i mappedItems med rätt providerItemCode och rätt providerID

Om den å andra sidan finns i items. Så kopiera itemId och lägg till enligt ovan i mappedItems.
Många fel i hotelservicen beror på denna SP: [GetItemFromIATACity], som är helt galen byggd. Detta behöver byggas om i hotelservicen.
