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
Apache2 powinno włączyć się odrazu i serwować podstawową stronę wyglądającą tak:
![apache_photo]([https://www.google.com/url?sa=i&url=https%3A%2F%2Fhelp.nextcloud.com%2Ft%2Fexisting-snap-install-now-just-shows-apache-default-page%2F32221&psig=AOvVaw1xk93HtCjpiLiURl_PmmEJ&ust=1704644758496000&source=images&cd=vfe&opi=89978449&ved=0CBIQjRxqFwoTCODEhd2WyYMDFQAAAAAdAAAAABAD](https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.linux.com%2Faudience%2Fdevops%2Fapache-ubuntu-linux-beginners%2F&psig=AOvVaw1xk93HtCjpiLiURl_PmmEJ&ust=1704644758496000&source=images&cd=vfe&opi=89978449&ved=0CBIQjRxqFwoTCODEhd2WyYMDFQAAAAAdAAAAABAI)https://www.google.com/url?sa=i&url=https%3A%2F%2Fwww.linux.com%2Faudience%2Fdevops%2Fapache-ubuntu-linux-beginners%2F&psig=AOvVaw1xk93HtCjpiLiURl_PmmEJ&ust=1704644758496000&source=images&cd=vfe&opi=89978449&ved=0CBIQjRxqFwoTCODEhd2WyYMDFQAAAAAdAAAAABAI)
