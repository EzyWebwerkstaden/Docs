# Myget
Istället för att använda våran nuget.ezyserver.se, Så har vi signat upp för myget,
Fördelen är att vi inte behöver under hålla feed servern. + att vi kan även ha egna nuget,symbols,discovery,npm,bower,vsix feeds. Men största fördelen är att vi får lite vettig authensiering.

## Hur hittar man sin "pre-authade" url.
När ni blivit inbjudna får ni ett mail där ni får signa upp för ett konto. Man kana använda social loginsen om man vill, sen när man kommit in så kan man kolla på denna sidan:
![Hitta pre-authade keyn](https://dl.dropboxusercontent.com/u/286578618/myfeed.jpg)
Och sen leta upp "Your pre-authenticated feed" , Får då slipper man skriva in anv/lösenord i VS hela tiden.

## Hur lägger man till MyGet i VS

- Klistra in "pre-authade" url i fönstret _Tools > Library Package Manager > Package Manager Settings_ 

![Package Manager Settings](http://docs.myget.org/docs/walkthrough/Images/faq_register_myget_feed.png)

- Öppna package manage console, ändra "Package source" till **EzyMyGet** och kör kommando för att installera MyGet paket som finns på MyGet, se t.ex. [Ezy.Security](https://www.myget.org/feed/ezy/package/nuget/Ezy.Security)

Läs mer här: http://docs.myget.org/docs/walkthrough/getting-started-with-nuget