## Sprawozdanie 4

Alicja Orłowicz, 407699

Zajęcia 08

## Instalacja zarządcy Ansible
 
 Laboratorium rozpoczęłam początkowo od utworzenia drugiej maszyny wirtualnej o jak najmniejszym zbiorze zainstalowanego oprogramowania.W moim przypadku była to Fedora.

Poniżej przedstawiono kroki instalacji 

![1](pictures/1.png)

![2](pictures/2.png)

![3](pictures/3.png)

![4](pictures/4.png)

Po ponownym uruchomieniu maszyny, mogłam zalogować się przy pomocy stworzonego przeze mnie użytkownika

![5](pictures/5.png)

Następnym krokiem było zainstalowanie oprogramowania Ansible zgodnie z dokumentacją dla systemu Fedora: https://docs.ansible.com/ansible/2.9/installation_guide/intro_installation.html#installing-ansible-on-rhel-centos-or-fedora

![6](pictures/6.png)



Kolejnym etapem była inwentaryzacja systemów.
Ustawiłam przewidywalne nazwy komputerów przy pomocy sudo hostnamectl set-hostname , gdzie hostnamectl to narzędzie wiersza poleceń w Linux OS, które umożliwia zarządzanie nazwą hosta systemu - dzięki niemu można ustawić nazwę hosta.

![8](pictures/8.png)

![7](pictures/7.png)

Aby umożliwić wywoływanie komputerów za pomocą nazw zamiast adresów IP, wprowadziłam nazwy DNS dla maszyn wirtualnych, korzystając z systemd-resolved. W tym celu dokonałam edycji dwóch plików /etc/hosts oraz /etc/systemd/resolved.conf. Musiałam użyć polecenia "sudo" w celu uzyskania uprawnień administratora, inaczej pojawiał się błąd związany z brakiem uprawnień do edycji pliku.

![9](pictures/9.png)

![10](pictures/10.png)

Następnie zrestartowałam usługę systemd-resolved przy użyciu polecenia sudo systemctl restart systemd-resolved.
Zweryfikowałam łączność za pomocą komendy ping i nazwy hosta. Weryfikacja łączności przebiegła pomyślnie.

![11](pictures/11.png)

![12](pictures/12.png)

W celu uzyskania łączności między maszynami wygenerowałam klucz i wymieniłam go pomiędzy maszynami oraz wykonałam próbę logowania przy użyciu SSH - zostałam zalogowana bez użycia hasła, co pokazuje prawidłowość wykonania tego kroku:

![13](pictures/13.png)
![14](pictures/14.png)
![15](pictures/15.png)


Następnie wyświetliłam listę dostępnych hostów.

![16](pictures/16.png)

Wysłałam moduł ping do wszystkich dostępnych hostów:

![17](pictures/17.png)

Wszystko przebiegło poprawnie i zgodnie z przewidywaniem - jedynie ansiblehost jest nieosiągalny, ze względu na to, że moduł ping nie działa tak jak zwykły ping - wymaga wymiany kluczy, a nie wymieniamy swojego klucza z samym sobą

Następnie stworzyłam plik inwentaryzacji. Jego efekt działania powinien być z założenia podobny do uzyskanego poprzednio.

Zawartość pliku :

![18](pictures/18.png)

Zweryfikowałam inventory, zgodnie z dokumentacją https://docs.ansible.com/ansible/latest/getting_started/get_started_inventory.html:

![19](pictures/19.png)

Uruchomiłam moduł ping przy użyciu pliku inventory.yaml.
Jak widzimy, rezultat działania jest taki jak omówiony poprzednio, powyższe kroki zostały wykonane poprawnie.

![20](pictures/20.png)

Zmodyfikowałam mój stary plik inwentaryzacji. Umieściłam w nim sekcje Orchestrators oraz Endpoints. Plik po edycji przedstawia się następująco:

![21](pictures/21.png)


## Zdalne wywoływanie procedur

Teraz pingując wszystkie maszyny, wystarczy, że spinguję moje endpointy (w tej sekcji nie ma obecnego hosta, więc maszyny są dwie i wszystko przebiega poprawnie):

![22](pictures/22.png)


Skopiowanie pliku inwentaryzacji na maszynę endpoint, następnie wykonanie tej samej komendy w celu sprawdzenia czy plik został zmieniony.

![23](pictures/23.png)

![24](pictures/24.png)


Następnie sprawdziłam, czy plik został skopiowany poprawnie na oba endpointy:

![27](pictures/27.png)

![28](pictures/28.png)

Ponowiłam operację - tym razem  zamiast statusu CHANGED uzyskałam SUCCESS.

![25](pictures/25.png)

![26](pictures/26.png)

Próba wykonania powyższej komendy przy wyłączonym internecie oraz usłudze ssh

![29](pictures/29.png)

Sprawdziłam też status sshd, aby upewnić się, że usługa została pomyślnie zdeaktywowana. Następnie wykonałam ping, tym razem endpoint z wyłączonym SSH jest UNREACHABLE:

![30](pictures/30.png)

Aby ping znowu zadziałał poprawnie, pomimo wyłączonej usługi SSH na jednym z endpointów, konieczna była edycja pliku konfiguracyjnego ansible. Znalazłam jego lokalizację za pomocą zapytania o wersję:

![31](pictures/31.png)

Dopisałam do niego następujące linijki:

![32](pictures/32.png)

Teraz ping obu endpointów przebiegł pomyślnie:

![33](pictures/33.png)



## Zarządzanie kontenerem

Stworzyłam plik playbook.yaml zawierający kroki budujące kontenera. 

```bash
- name: Deploy application in Docker container 
  hosts: endpoints 
  become: yes 
  become _method: sudo 
  tasks:
    - name: Install docker 
      become: yes 
      pip:
        name:
           - websocket-client==0.59.0
           - requests==2.28.2
           - urllib3==1.26.15
           - docker
           - docker-compose 
        state: present
    - name: First test
      shell: docker ps 
    - name: Build application 
      docker_image:
        source: build 
        build:
          path: /home/alaor/dockerfiles 
        name: app
    - name: Run container  
      docker_container:
        name: app_container 
        image: app
```
Wersja z obrazem z DockerHub:

```bash
- name: Deploy application in Docker container 
  hosts: endpoints 
  become: yes 
  become _method: sudo 
  tasks:
    - name: Install docker 
      become: yes 
      pip:
        name:
           - websocket-client==0.59.0
           - requests==2.28.2
           - urllib3==1.26.15
           - docker
           - docker-compose 
        state: present
    - name: First test
      shell: docker ps 
    - name: Build application 
      docker_image:
        source: pull
        name: alaor/moja_aplikacja
    - name: Volume
        name: volume
    - name: Run docker container  
      docker_container:
        name: app
        image: alaor/moja_aplikacja
        intercative: true
        ports:
          - "8081:8081"
        volumes:
          - /volume
```

Niestety ze względu na problemy systemowe nie byłam w stanie uzyskać poprawnych rezultatów tego kroku. 

![34](pictures/34.png)


 ## Instalacja nienadzorowana systemu

Aby przeprowadzić instalację nienadzorowaną, musiałam skopiować plik odpowiedzi o nazwie /root/anaconda-ks.cfg. Ten plik był potrzebny do skonfigurowania instalacji. Jednak, z powodu ograniczeń dotyczących uprawnień, nie mogłam bezpośrednio skopiować pliku z folderu /root. W celu rozwiązania tego problemu, skorzystałam z wcześniej utworzonej maszyny. Z tej maszyny mogłam uzyskać dostęp do pliku odpowiedzi. Następnie skopiowałam ten plik z maszyny do docelowej lokalizacji na innym urządzeniu, aby umożliwić instalację nienadzorowaną. W rezultacie, mogłam pomyślnie skopiować plik odpowiedzi /root/anaconda-ks.cfg.

![35](pictures/35.png)

![36](pictures/36.1.png)
![36](pictures/36.2.png)

Dzięki temu kopiowanie przebiegło już w sposób zgodny z przewidywaniem,a plik wrzuciłam do mojego folderu na repozytorium Github. Jego zawartość przedstawia się następująco:

```bash
# Generated by Anaconda 37.12.6
# Generated by pykickstart v3.41
#version=DEVEL
# Use graphical install
graphical

# Keyboard layouts
keyboard --vckeymap=pl --xlayouts='pl'
# System language
lang pl_PL.UTF-8

%packages
@^minimal-environment
@editors
@headless-management
@network-server

%end

# Run the Setup Agent on first boot
firstboot --enable

# Generated using Blivet version 3.5.0
ignoredisk --only-use=sda
autopart
# Partition clearing information
clearpart --none --initlabel

timesource --ntp-disable
# System timezone
timezone America/New_York --utc

# Root password
rootpw --iscrypted --allow-ssh $y$j9T$49G7rjzpQf91/eBmO3NJJsrg$hTWbkq65.cT9leUTsUdO3o6OgDD0NIizbf7FQffeXy.
user --groups=wheel --name=ansible_alaor --password=$y$j9T$HiwFt4cyn/64AzGKPExp3y9G$/Z528z3jwAUTvE6Jf6IbRN98vibYpsnbFG.TN5PqD69 --iscrypted --gecos="ansible_alaor"
```
dodanie do pliku anaconda-ks.cfg fragmentu :

```bash
url --mirrorlist=http://mirrors.fedoraproject.org/mirrorlist?repo=fedora-37&arch=x86_64
repo --name=updates --mirrorlist=http://mirrors.fedoraproject.org/mirrorlist?repo=updates-released-f37&arch=x86_64
```
Utworzyłam nową maszynę wirtualną z systemem Fedora.

![37](pictures/37.png)
![38](pictures/38.png)

Aby potwierdzić, czy instalacja przebiegła pomyślnie na nowo utworzonej maszynie sprawdzam działające kontenery


![39](pictures/39.png)

Wnioski :
Ansible okazał się być przydatnym narzędziem, które znacząco ułatwia operacje między dwoma komputerami.Dzięki Ansible możemy zdefiniować zestaw instrukcji i skryptów w pliku konfiguracyjnym, który automatycznie wykonuje wszystkie niezbędne operacje.Ansible umożliwia replikację i skalowanie zadań na wiele maszyn jednocześnie, co czyni go niezwykle przydatnym narzędziem w środowiskach, w których konieczne jest zarządzanie infrastrukturą wielu serwerów.
