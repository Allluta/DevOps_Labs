## Sprawozdanie 5

Alicja Orłowicz, 407699


## Instalacja klastra Kubernetes

Implementacja stosu k8s:minikube wedle oficjalnej dokumentacji (https://minikube.sigs.k8s.io/docs/start/) wraz z sprawdzeniem statusu klastra lokalnego:

![1](pictures/1.png)

![2](pictures/2.png)

![3](pictures/3.png)

Instalacja kubectl wedle oficjalnej dokumentacji (https://kubernetes.io/docs/tasks/tools/install-kubectl-linux#install-kubectl-binary-with-curl-on-linux):

![5](pictures/5.png)

![6](pictures/6.png)

![7](pictures/7.png)

![8](pictures/8.png)

Kluster minikube postawiony, kubectl można również pobrać używając:

![9](pictures/9.png)

Wylistowanie działajacych kontenerów/workerów działających na lokalnym klastrze - wszystko działa poprawnie : 

![10](pictures/10.png)

Uruchomienie dashboarda w przeglądarce domyślnej systemu, łączność odbywa się na localhoście na porcie 39285:

![11](pictures/11.png)

![12](pictures/12.png)

Utworzenie przykładowych wdrożeń.

![13](pictures/13.png)

![14](pictures/14.png)

![15](pictures/15.png)

Pobranie gotowego obrazu nginx:stable :

![16](pictures/16.png)

Uruchomienie kontenera :

![17](pictures/17.png)

![18](pictures/18.png)

![19](pictures/19.png)

Włączenie obsługi deploymentu kubectl get services hello-minikube. Przekierowanie na port :

![20](pictures/20.png)

![21](pictures/21.png)

![22](pictures/22.png)

![23](pictures/23.png)

### Wdrożenie
 Utworzenie pliku .yaml za pomocą polecenia 'touch nginx-deployment.yaml'
  Zawartość pliku: 

![24](pictures/24.png)

Sprawdzono rezultaty przy pomocy kubectl get pods oraz Dashboarda minikube:

![25](pictures/25.png)

Wszystko działa poprawnie i zgodnie.

## Wdrażanie na zarządzalne kontenery: Kubernetes (2)

Konwersja wdrożenia ręcznego na wdrożenie deklaratywne YAML

Wzbogacono obraz o 4 repliki:

![26](pictures/26.png)

Rozpoczęto wdrożenie za pomocą kubectl apply i zbadano stan za pomocą kubectl rollout status :

![27](pictures/27.png)

![28](pictures/28.png)

![29](pictures/29.png)


Przygotowanie nowego obrazu

Pobranie innej, starszej wersji obrazu nginx :

![30](pictures/30.png)

Zmiany w deploymencie

Zaktualizowanie pliku YAML z wdrożeniem i przeprowadzenie  ponownie po zastosowaniu zmian.
Zmniejszenie liczby replik do 1.

![31](pictures/31.png)

![32](pictures/27.png)

![33](pictures/32.png)

Zmniejszenie liczby replik do 0:

![34](pictures/33.png)

![35](pictures/34.png)

Zastosowanie starszej wersji:

![36](pictures/35.png)

![37](pictures/36.png)

Przywrócenie poprzednicj wersji wdrożeń za pomocą poleceń kubectl rollout history i kubectl rollout undo :

![38](pictures/44.png)
![38.1](pictures/44.1.png)
![40](pictures/39.png)

Kontrola wdrożenia

Napisanie skryptu weryfikującego, czy wdrożenie „zdążyło” się wdrożyć (60 sekund). 

![41](pictures/40.png)

![41.1](pictures/40.1.png)


### Strategie wdrożenia

Recreate kończy dziąłanie nieaktualnych instancji i uruchamia je w nowszej wersji, zapewnia ciągłość odnawiania stanu aplikacji.

![42](pictures/41.png)
![42.2](pictures/41.2.png)

Pody są niszczone.

Rolling Update umożliwia stopniową aktualizację. Liczba podów ze starszą wersją jest zmniejsza a z nową wersją zwiększana. Istotnymi argumetami są: maxSurge – liczba podów, które możemy dodać,  maxUnvailable – liczba podów nieaktywnych

![43](pictures/42.png)

![42.1](pictures/41.1.png)


Canary Deployment workload opiera się na wdrożeniu nowego oprogramowania obok starszych wersji, które są stabilne. Wraz z nowym wdrożeniem umożliwia zwiększenie podów i rezygnację ze starych.

![44](pictures/43.png)

![45](pictures/45.png)

Dobór odpowiedniej strategii wdrażania zależy od konkretnych wymagań i kontekstu projektu. Istnieje kilka czynników, które warto wziąć pod uwagę.

Pierwszym aspektem do rozważenia jest dostępność aplikacji. Jeżeli aplikacja musi być nieprzerwanie dostępna i nie może doświadczać przerw w działaniu, strategia "Rolling Update" może być odpowiednia. Jest to korzystniejszy wybór niż strategia "Recreate", która może prowadzić do tymczasowej niedostępności.

Kolejnym czynnikiem jest poziom ryzyka i potrzebna elastyczność. Jeśli konieczne jest ograniczenie ryzyka związanego z wdrażaniem nowej wersji aplikacji lub częstym cofaniem aktualizacji, strategia "Canary Deployment" może być dobrym rozwiązaniem. Pozwala ona na stopniowe wprowadzanie zmian i monitorowanie jakości.

Trzecim aspektem są wymagania dotyczące czasu. W niektórych projektach priorytetem jest szybkie wdrożenie nowej wersji aplikacji, wtedy strategia "Recreate" może być najbardziej odpowiednia.

Ostateczny wybór strategii powinien wynikać z analizy korzyści i wad każdej z nich. Nie istnieje jednoznaczna odpowiedź, ponieważ zależy to od indywidualnych potrzeb i wymagań projektu. Przeprowadzenie praktycznego laboratorium, które pozwoli zapoznać się z tymi strategiami i ich zastosowaniem, może pomóc dokonać właściwego wyboru.