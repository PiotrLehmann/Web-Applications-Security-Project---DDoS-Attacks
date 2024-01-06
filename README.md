# Ataki DDoS i Obrona Przed Nimi
## Zalecane przed Labem (przygotowanie środowiska)
1. Potrzebne bedą `dwie maszyny wirtualne` (`atakujący` i `broniący` serwera), polecamy do tego `VMWare` lub `VirtualBox`
   * [Jak zainstalować VMWare?](https://www.youtube.com/watch?v=PoNPBdKLZdk)
   * [Jak zainstalować Ubuntu na maszynie wirtualnej?](https://www.youtube.com/watch?v=NhlhJFKmzpk&t=261s)
     
2. Na obydwu maszynach wskazane jest wykonanie poniższych komend
   * `sudo apt-get update`
   * `sudo apt install net-tools`

## Zadanie 1
To zadanie będzie polegało na skonfigurowaniu i uruchomieniu web-serwera Apache2, który będzie serwował swoją podstawową stronę, pod adresem naszej wirtualnej maszyny. Efektem wykonania zadania powinna być możliwość otworzenia strony 'pod adresem naszej maszyny wirtualnej' na windowsie. To właśnie na windowsie będziemy obserwować efekt ataku DoS, oraz czy obrona przed nim zadziałała.

### Instalacja Webmin
Webmin to panel kontrolny do obsługi między innymi naszego web-serwer apache2. Pozwoli nam on w łatwy i przyjemny sposób nie tylko zainstalować potrzebne narzędzia, ale i np. obserwować ilość zapytań przychodzących do naszego serwera. Będziemy się tutaj posiłkować zewnętrznym repozytorium, a intrukcja instalacji znajduje się poniżej:
  * Otwórz plik sources.list w dowolnym edytorze `sudo nano /etc/apt/sources.list`
  * Przejdź na sam dół pliku i po Enterze dodaj kolejną linijkę, która będzie źródłem      
  repozytorium `deb http://download.webmin.com/download/repository sarge contrib`
Następnie przechodzimy do pobrania zasobów
  * `sudo apt-key add jcameron-key.asc`
  * `sudo apt-get update`
  * `sudo apt-get install webmin`
Po wykonaniu powyższych instrukcji, wystarczy sprawdzić swój adres IP
  * komendą `ifconfig`
Pod tym adresem, na porcie `10000` będzie teraz działać nasz `webmin`. Wejdź na stronę i zaloguj się swoim loginem i hasłem do linuxa.
  * Jeżeli twoje login i hasło nie działają, użyj koemndy `sudo -i`, a następnie `sudo      passwd` i zmień swoje hasło.
Po zalogowaniu powinieneś zobaczyć dashboard ze zużyciem zasobów serwera (twojej maszyny wirtualnej).
### Utworzenie web-serwera Apache2
Używając hamburger-menu przystępujemy do kolejnych kroków, żeby móc wreszcie hostować podstawową stronę na Apache2.
  * Wejdź do zakładki `System`, wybierz `Software Package Updates` a następnie zaktualizuj wszystkie paczki przyciskiem `Update`. Pominięcie tego kroku może powodować komplikacje w ćwiczeniach!
  * W zakładce `Unused Modules` wybieramy `Apache WebServer` i przechodzimy do instalacji
Apache2 powinno włączyć się odrazu i serwować podstawową stronę. Można na nią przejść na swoim głownym systemie, `wpisując adres maszyny wirtualnej w pasek przeglądarki`. Jeśli jednak byłby problem z jej wyświetleniem, polecamy wykonać komendę `service apache2 restart`, lub `stop` a potem `start`. W tym momencie maszyna wirtualna broniącego się jest już gotowa. Możesz teraz włączyć drugą maszynę wirtualną (polecamy podzielić ekran na pół). Pozostaw także windowsową przeglądarkę uruchomioną ze stroną pod adresem maszyny broniącego.
### Konfiguracja maszyny atakującego
Tutaj już zostaje nam niewiele do zrobienia. Musimy na maszynie atakującego wykonać kilka komend
  * `sudo apt-get update`
  * `sudo apt install net-tools`
  * `sudo apt install hping3`
### Przetestuj łącznośc pomiędzy maszynami
Użyj komend `ifconfig`, na maszynie broniącego. Następnie na maszynie atakującego wykonaj ping pod adres maszyny broniącego. Jeżeli wykonałeś wszystko poprawnie, ping powinien przechodzić z sukcesem.

## Zadanie 2.
A tym zadaniu przechodzimy już do ataku i obrony przed nim. Nasza prezentacja opowiada o DDoS, jednakże nie będziemy wykonywać ataku z kilku komputerów naraz. Dla zachowania idei i prostoty, wykonamy rodzaj ataku DoS, którym jest SYN flood. W trakcie prezentacji był on szczegółowo opisany, dlatego skoro wiesz już, jak on działa, możesz wykonać go za pomocą hping3.
  * wykonaj komendę `sudo hping3 -S --flood -V -p 80 <adres_maszyny_broniącego>`
Flaga `-S` odpowiada za rodzaj ataku (SYN flood), `--flood` powoduje wysyłanie pakietów tak szybko, jak to możliwe (zalewając nimi nasz cel). `-V` pozwala na "Verbose output" odpowiedzi na wysyłane zapytania. Atakujemy adres na porcie `80`, za co odpowiada flaga `-p`. Jest to port http, dzięki temu zobaczymy efekt niełądującej się strony. `UWAGA`, widoczność efektów ataku jest zależna od wielu czynników, należy chwilę poczekać, zanim serwer przestanie odpowiadać. Gdyby ktoś chciał pokombinować z flagami komendy hping3, odsyłamy do jej oficjalnej dokumentacji [tutaj](https://linux.die.net/man/8/hping3).
