# Sprawozdanie 2 

Alicja Orłowicz

Laboratorium 3 : Docker files, kontener jako definicja etapu

Pierwszym krokiem było znalezienie repozytorium z kodem dowolnego oprogramowania, które dysponuje otwartą licencją oraz zawiera Makefile i testy. Wybrano kalkulator.

* Sklonowano wybrane repozytorium

![1](pictures/3.1.png)

* Doinstalowano wymagane zależności(doinstalowanie ncurses)

![2](pictures/3.2.png)

* Zbudowano program 

![3](pictures/3.3.png)

* Uruchomiono testy, a następnie program

![4](pictures3.4.png)

![5](pictures/3.5.png)

Kolejnym krokiem było uruchomienie Dockera i sprawdzenie dostepnych obrazów

![6](pictures/3.6.png)

Uruchomiono kontener i podłączono się do niego celem rozpoczęcia interaktywnej pracy. Zainstalowano również gita.

![7](pictures/3.7.png)

Sklonowano repozytorium

![8](pictures/3.8.png)

Następnie uruchomiono build programu poleceniem make oraz uruchomiono testy za pomocą bash ./run-tests.sh

![9](pictures/3.9.png)

W drugim zadaniu należało stworzyć dwa pliki Dockerfile automatyzujące wcześniej wykonywane kroki. Pierwszy kontener przeprowadza wszystkie kroki aż do builda: 

![10](pictures/3.10.png)

Utworzono obraz z pliku DockerFile1 

![11](pictures/3.11.png)

![12](pictures/3.12.png)

Utworzono drugi plik Dockerfile2, w którym wybierany jest aktualny obraz utworzony wcześniej, następnie uruchamiane są testy.

![13](pictures/3.13.png)

Utworzenie obrazu z pliku DockerFile2:

![14](pictures/3.14.png)

![15](pictures/3.15.png)

![16](pictures/3.16.png)

Na końcu sprawdzono historię 

![17](pictures/3.17.png)


Laboratorium 4 : Dodatkowa terminologia w konteneryzacji, instancja Jenkins

**ZACHOWANIE STANU**

Przygotowano woluminy wejściowy i wyjściowy o nazwach `volin` oraz `volout`. Wykorzystano komendę `sudo docker volume create`.

![1](pictures/4.1.png)

 Następnie uruchomiono kontener z woluminami za pomocą `sudo docker run` i wyświetlono odpowiednie katalogi `vin` i `vout`

 ![2](pictures/4.2.png)

 Wyświetlono szczegóły o obu woluminach oraz ścieżkę, która będzie potrzebna w następnym kroku. Wykorzystano komendę `sudo docker volume inspect`

 ![3](pictures/4.3.png)

 Z poziomu roota skopiowano repozytorium na wolumin wejściowy `volin` z wykorzystaniem ścieżki z poprzedniego kroku oraz polecenia `git clone`

 ![4](pictures/4.4.png)

 W kontenerze widoczne jest sklonowane repozytorium

 ![5](pictures/4.5.png)

 Do wykonania builda w kontenerze konieczne było doinstalowanie `make`, `gcc` oraz biblioteki `ncurses`

 ![6](pictures/4.6.png)

 ![7](pictures/4.7.png)

 ![8](pictures/4.8.png)

 Zapisano pliki w woluminie wyjściowym

 ![9](pictures/4.9.png)

 **EKSPORTOWANIE PORTU**

 W następnym zadaniu pierwszym krokiem było pobranie i uruchomienie odpowiedniego obrazu dockerowego

 ![10](pictures/4.10.png)

 ![11](pictures/4.11.png)

 Sprawdzono za pomocą `sudo netstat` nasłuchiwanie serwera

 ![12](pictures/4.12.png)

 ![13](pictures/4.13.png)

 Sprawdzono IP serwera

 ![14](pictures/4.14.png)

 Połączono się z serwerem z drugiego kontenera, którego działanie określono jako client

 ![15](pictures/4.15.png)

 W celu połączenia się spoza kontenera (z i spoza hosta) najpierw zainstalowano iperf3

 ![16](pictures/4.16.png)

 Połączenie z hosta

 ![17](pictures/4.17.png)

 Połączenie spoza hosta (komputer z systemem Windows 11). Poczatkowo pobrałam Iperf3

 ![18](pictures/4.18.png)

 ![19](pictures/4.19.png)

 ![20](pictures/4.20.png)

 Na koniec wyciągnięto logi z kontenera aby przedstawić przepustowość komunikacji

 ![21](pictures/4.21.png)

 ![22](pictures/4.22.png)

 ![23](pictures/4.23.png)

 Uzyskane wyniki wskazują na to, że największą przepustowość uzyskano między kontenerami. Podobny rezultat otrzymano dla transferu między hostem a kontenerem. Wyraźnie najniższą przepustowość uzyskano dla transferu spoza hosta.

 **INSTALACJA JENKINS**

 Utworzono nową sieć dockerową oraz pobrano i uruchomiono obraz DIND kontenera

 ![24](pictures/4.24.png)

 Utworzono Dockerfile

 ![25](pictures/4.25.png)

 Zbudowano nowy obraz dockera z pliku Dockerfile określając za pomocą `-t` nazwę

 ![26](pictures/4.26.png)

 ![27](pictures/4.27.png)

 Uruchomiono stworzony obraz

 ![28](pictures/4.28.png)

 Sprawdzono adres localhost:8080 i odblokowano za pomocą hasła

 ![29](pictures/4.29.png)

* Aby odczytać hasło uruchomiono najpierw interaktywny terminal za pomocą `sudo docker exec`, polecenie `cat` pozwoliło na odczytanie już samego hasła

 ![30](pictures/4.30.png)

 Zainstalowano zalecane wtyczki

 ![31](pictures/4.31.png)

 Za pomocą polecenia `sudo docker ps` wykazano działanie kontenerów

 ![32](pictures/4.32.png)

 Ekran logowania 

 ![33](pictures/4.33.png)

 ![34](pictures/4.34.png)





