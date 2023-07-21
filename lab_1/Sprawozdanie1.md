Alicja Orłowicz
# SPRAWOZDANIE LABORATORIUM 1: WPROWADZENIE , GIT, GAŁĘZIE, SSH

[1] Zainstalowano klienta Git i obsługę kluczy SSH.

![gitInstall](pictures/1.png))

[2] Sklonowano repozytorium  za pomocą HTTPS.

![gitHTTPS](pictures/2.png)

[3] Utworzono dwa klucze SSH, inne niż RSA, w tym co najmniej jeden zabezpieczony hasłem.

![key](pictures/3.png)

![keyPassword](pictures/4.png)

[4] Skonfigurowano klucz SSH jako metodę dostępu do GitHuba

![gitSSH](pictures/5.png)

[5]Sklonowano repozytorium z wykorzystaniem protokołu SSH

niestety tego kroku nie uwieczniłam na ss

[6]Przełączono się na gałąź main, a następnie gałąź swojej grupy – GCL2.
Utworzono gałąź o nazwie AO407699 i przełączono się na nią. Następnie rozpoczęto pracę na nowej gałęzi.

![branch/checkout](pictures/6.png) 

![branch/checkout](pictures/7.png)

[7]Rozpoczęto pracę nad Git Hookiem -utworzono nowy plik i zedytowano. Napisanie Git Hook'a polegało na tym, by weryfikował każdy commit message -  czy zaczyna sie od moich inicjałów oraz numeru indeksu, czyli AO407699. Następnie plik skopiowano do odpowiedniego katalogu.

![gitHook1](pictures/8.png)

![gitHook2](pictures/9.png)

![gitHook3](pictures/10.png)

[8] Przetestowano poprawność działania Git-Hooka – można zauwazyć na zrzucie ekranu, że działa poprawnie.

![gitHook4](pictures/11.png)

[9] Dodano sprawozdanie oraz screeny do katalogu.
![ss](pictures/12.png)

[10] Wyciągnieto gałąź do gałęzi grupowej.
![branch](pictures/13.png)

[11] Zmodyfikowano sprawozdanie o dalsze kroki oraz ponowne wypchnięto gałąź.

![gitPush](pictures/14.png)


# SPRAWOZDANIE LABORATORIUM 2: GIT, DOCKER 

[1] Zainstalowano dockera oraz sprawdzono jego wersję.

![Docker1](pictures/15.png)

![Docker2](pictures/16.png)

[2] Zarejestrowano się w Docker Hub i zapoznano z sugerowanymi obrazami.

![Docker3](pictures/17.png)

[3] Następnie pobrałam hello-world, busybox, ubuntu i mysql.
![hello-world/busybox/mysql](18.png)

![ubuntu](pictures/19.png)

[4] Po pobraniu hello-world sprawdzono czy instalacja się powiodła. Mozna zauważyć na zrzucie ekranu, że tak.

![runHello](pictures/20.png)

[5]Wyświetlono dostępne zainstalowane przez nas obrazy.

![dockerImages](pictures/21.png)

[6] Kolejnym krokiem było uruchomienie busyboxa, interaktywne podłączenie się do niego, a takze jego wersja

![busybox](pictures/22.png)

[7] Uruchomiono Ubuntu, sprawdzono PID1 i procesy dockera oraz zaaktualizowano pakiety.

![ubuntu](pictures/23.png)

[8] Stworzono własnoręcznie, zbudowano i uruchomiono prosty plik Dockerfile bazujący na wybranym systemie.

![Dockerfile1](pictures/24.png)

![Dockerfile2](pictures/25.png)

![Dockerfile3](pictures/26.png)

[9] Zmieniono nazwę w celu ułatwienia pracy.

![name](pictures/27.png)

[10] Upewniono się, czy sklonowano repozytorium.

![repo](pictures/28.png)

[11] Następnie w drugim terminalu sprawdzono uruchomione kontenery.

![dockerps](pictures/29.png)

[12] Usunięto uruchomione kontenery poleceniem. Aby się upewnić, czy poprawnie usunęłam, listowałam uruchamiane kontenery.

![removeContener](pictures/30.png)

[13] Wyczyszczono wszystkie obrazy.

![images](pictures/31.png)















