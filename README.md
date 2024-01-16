# Ataki DDoS i Obrona Przed Nimi
## Zalecane przed Labem (przygotowanie środowiska)
1. Potrzebne bedą `dwie maszyny wirtualne` (`atakujący` i `broniący` serwera), polecamy do tego `VMWare` lub `VirtualBox`, z systemem operacyjnym Linux (my użyliśmy ubuntu 22.04)
   * [Jak zainstalować VMWare?](https://www.youtube.com/watch?v=PoNPBdKLZdk)
   * [Jak zainstalować Ubuntu na maszynie wirtualnej?](https://www.youtube.com/watch?v=NhlhJFKmzpk&t=261s)
     
2. Na obydwu maszynach wskazane jest wykonanie poniższych komend
   * `sudo apt-get update`
   * `sudo apt install net-tools`

## Zadanie 1 - konfiguracja maszyny obrońcy
To zadanie będzie polegało na skonfigurowaniu i uruchomieniu web-serwera Apache2, który będzie serwował swoją podstawową stronę, pod adresem naszej wirtualnej maszyny. Efektem wykonania zadania powinna być możliwość otworzenia strony 'pod adresem naszej maszyny wirtualnej' na windowsie. To właśnie na windowsie będziemy obserwować efekt ataku DoS, oraz czy obrona przed nim zadziałała.

### Instalacja Webmin
Webmin to panel kontrolny do obsługi między innymi naszego web-serwer apache2. Pozwoli nam on w łatwy i przyjemny sposób nie tylko zainstalować potrzebne narzędzia, ale i np. obserwować ilość zapytań przychodzących do naszego serwera. Będziemy się tutaj posiłkować zewnętrznym repozytorium, a intrukcja instalacji znajduje się poniżej:
  * Otwórz plik sources.list w dowolnym edytorze `sudo nano /etc/apt/sources.list`
  * Przejdź na sam dół pliku i po Enterze dodaj kolejną linijkę, która będzie źródłem      
  repozytorium `deb http://download.webmin.com/download/repository sarge contrib`
Następnie przechodzimy do pobrania zasobów:
  * `wget http://www.webmin.com/jcameron-key.asc`
  * `sudo apt-key add jcameron-key.asc`
  * `sudo apt-get update`
  * `sudo apt-get install webmin`
Po wykonaniu powyższych instrukcji, wystarczy sprawdzić swój adres IP
  * komendą `ifconfig`
Pod tym adresem, na porcie `10000` będzie teraz działać nasz `webmin`. Wejdź w przeglądarce na stronę `<Twoj adres IP>:10000` i zaloguj się swoim loginem i hasłem do linuxa.
  * Jeżeli twoje login i hasło nie działają, użyj koemndy `sudo -i`, a następnie `sudo passwd` i zmień swoje hasło.
Po zalogowaniu powinieneś zobaczyć dashboard ze zużyciem zasobów serwera (twojej maszyny wirtualnej).
### Utworzenie web-serwera Apache2
Używając hamburger-menu przystępujemy do kolejnych kroków, żeby móc wreszcie hostować podstawową stronę na Apache2.
### Instrukcje z poniższego akapitu wykonaj tylko, jeśli w pewnym momencie później coś nie będzie działać.
W panelu webmin wejdź do zakładki `System`, wybierz `Software Package Updates`. Domyślnie wszystkie paczki powinny być zaznaczone. Nie zmieniaj tego i zaktualizuj wszystkie paczki przyciskiem `Update Selected Packages`. Gdy aktualizacja się zakończy, kliknij `Install Now`. Pominięcie tego kroku może powodować komplikacje w ćwiczeniach, ale trwa bardzo długo. 
### Konfiguracja serwera Apache
W zakładce `Unused Modules` wybieramy `Apache WebServer` i przechodzimy do instalacji. Apache2 powinno włączyć się odrazu i serwować podstawową stronę. Można na nią przejść na swoim głownym systemie, `wpisując adres maszyny wirtualnej w pasek przeglądarki`. Jeśli jednak byłby problem z jej wyświetleniem, polecamy wykonać komendę `service apache2 restart`, lub `stop` a potem `start`. W tym momencie maszyna wirtualna broniącego się jest już gotowa. Możesz teraz włączyć drugą maszynę wirtualną (polecamy podzielić ekran na pół). Pozostaw także windowsową przeglądarkę uruchomioną ze stroną pod adresem maszyny broniącego.
### Konfiguracja maszyny atakującego
Tutaj już zostaje nam niewiele do zrobienia. Musimy na maszynie atakującego wykonać kilka komend
  * `sudo apt-get update`
  * `sudo apt install net-tools`
  * `sudo apt install hping3`
### Przetestuj łącznośc pomiędzy maszynami
Użyj komend `ifconfig`, na maszynie broniącego. Następnie na maszynie atakującego wykonaj ping pod adres maszyny broniącego. Jeżeli wykonałeś wszystko poprawnie, ping powinien przechodzić z sukcesem. Do zaliczenia zadania wyślij screenshot pinga z maszyny atakującego do obrońcy oraz screenshot działającej strony podstawowej Apache.

## Zadanie 2.
A tym zadaniu przechodzimy już do ataku i obrony przed nim. Nasza prezentacja opowiada o DDoS, jednakże nie będziemy wykonywać ataku z kilku komputerów naraz. Dla zachowania idei i prostoty, wykonamy rodzaj ataku DoS, którym jest SYN flood. W trakcie prezentacji był on szczegółowo opisany, dlatego skoro wiesz już, jak on działa, możesz wykonać go za pomocą hping3.
  * wykonaj komendę `sudo hping3 -S --flood -V -p 80 <adres_maszyny_broniącego>`
Flaga `-S` odpowiada za rodzaj ataku (SYN flood), `--flood` powoduje wysyłanie pakietów tak szybko, jak to możliwe (zalewając nimi nasz cel). `-V` pozwala na "Verbose output" odpowiedzi na wysyłane zapytania. Atakujemy adres na porcie `80`, za co odpowiada flaga `-p`. Jest to port http, dzięki temu zobaczymy efekt niełądującej się strony. `UWAGA`, widoczność efektów ataku jest zależna od wielu czynników, należy chwilę poczekać, zanim serwer przestanie odpowiadać. Gdyby ktoś chciał pokombinować z flagami komendy hping3, odsyłamy do jej oficjalnej dokumentacji [tutaj](https://linux.die.net/man/8/hping3). Z jakiegoś powodu, u nas atak zadziałał dopiero za 3 razem, polecamy użyć kilku terminali, lub odczekać dłuższą chwilę. Dłuższe niż 2s ładowanie statycznej strony można również traktować jako sukces, jeżeli nie chcecie nadwyrężać swojego sprzętu, który już i tak ma uruchomione dwie maszyny wirtualne. **Odświeżaj cały czas swoją stronę z apache2, żeby zobaczyć efekty.** Zapisz zrzut ekranu z ładującą się stroną oraz działającą komendą hping i wyślij jako odpowiedź na zadanie 2.
  * Przechodzimy teraz do odnalezienia źródła problemu (adresu atakującego). Z tego względu użyjemy komendy `netstat -an | grep :80 | sort`. Flaga `-a` odpowiada za pokazywanie ruchu ze wszystkich socketów, `-n` odpowiada, za pokazanie przy wynikach adresów źródłowych. Szukamy oczywiście na porcie 80 i sortujemy. W ten sposób znajdziemy adres atakującego - sprawdz, czy ten adres należy do twojej drugiej maszyny wirtualnej.
  * Mając już konkretny adres, skopiuj go. Użyjemy teraz popularnego narzędzia `iptables`, które pozwala filtrować, blokować, odrzucać ruch przychodzący i wychodzący pod nasz adres. Ma też wiele innych funkcjonalności, ale nie użyjemy ich na naszym labie. Żeby zablokować adres atakującego, użyj komendy `sudo iptables -I INPUT -s <adres_atakującego> -j REJECT`. Po wykonaniu tej komendy nie będzie widać efektu odrazu, nasz serwer dalej ma przepełnione bufory, wcześniej wysłanymi zapytaniami. Musimy go zatem zrestarować poleceniami `service apache2 stop`, a następnie `service apache2 start`. **Efektem po odświeżeniu strony, powinno być powrotne się jej załadowanie, mimo ciągle przeprowadzanego ataku**. Sprawdz również, czy możesz wykonać ping w z maszyny atakującego.. Powinien być on odrzucany. Brawo właśnie obroniłeś/aś się przed bardzo podstawowym atakiem DoS.

## Zadanie 3.
Nasze poprzednie akcje powstrzymują bardzo podstawowy atak wykonany przez niezbyt kompetentą osobę. Narzędzi do ataków DoS jest bardzo dużo, im droższe tym najczęściej bardziej skuteczne. Hping3 oferuje dużo więcej możliwości, niż pokazaliśmy w powyższym ćwiczeniu. Zacznijmy choćby od flagi `--rand-source`, która spowoduje wyświetlenie w naszym netstat losowych adresów IP, zamiast faktycznego adresu atakującego. Wykonanie takiego ataku z kilku terminali (w naszym symulowanym środowisku), lub z wielu zainfekowanych przez atakującego komputerów, skończyłoby się całkowitym zablokowaniem dostępu do naszej strony i iptables zdałoby się na niewiele, w zależności od tego jak dużo adresów próbowałoby "DDoSować" nasz serwer. Dlatego w 3 zadaniu spróbujemy bronić się przed atakiem w bardziej uniwersalny sposób, który jednak po złej konfiguracji, może znacznie obniżyć jakość usług którą oferujemy na serwerze - mowa tutaj o `evasive mode`. `Jest to mod, który możemy dodać do naszego serwera Apache`, a oferuje kontrolę nad `Rate limiting`, czyli można powiedzieć, stopniem zużycia zasobów naszego serwera. Możem,y tu wprowadzić kilka ustawień, które dadzą nam porządane efekty w kwestii obrony przez atakami DoS i DDoS. Zacznijmy zatem konfigurację, potrzebujemy najpierw uruchomić terminal na maszynie broniącego i pobrać evasive_mode na nasz serwer apache2.
  * `sudo apt-get update`
  * `sudo apt-get install apache2-utils`
  * `sudo apt-get install libapache2-mod-evasive`
  * `sudo nano /etc/apache2/mods-enabled/evasive.conf`
Powinnien wyświetlić się wam plik konfiguracyjny z następującymi opcjami do ustawienia.
```
DOSHashTableSize 3097
DOSPageCount 2
DOSSiteCount 50
DOSPageInterval 1
DOSSiteInterval 1
DOSBlockingPeriod 10
DOSEmailNotify mail@yourdomain.com
DOSLogDir "/var/log/apache2/"
```
  * `DOSHashTableSize` musi być to liczba pierwsza, definiuje jak duża może być tablica dostępu do naszegej strony. Gdybyśmy ustawili zbyt mały numer, nie będzie możliwy dostęp dla wielu normalnych użytkowników w jedny momencie.
  * `DOSPageCount ` to liczba identycznych requestów do identycznego URI, jakie użytkownik może zrobić podczas `DOSPageInterval`. Ustawiając wartość 2, dla `DOSPageInterval` = 10s, ustalimy zasadę, która pozwoli tylko raz odświeżyć stronę użytkownikowi. Należałoby zatem ustawić tu sensowne wartości
  * `DOSSiteCount` podobne do powyższego ale dotyczy dostępu do całej strony, a nie do konkretnych URI podczas `DOSSiteInterval `
  * `DOSBlockingPeriod` jeżeli użytkownik przekroczy którykolwiek z limitów, zostanie zablokowany na ten właśnie czas. Podczas blokady będziemy dostawać odpowiedź `403 Forbidden Error`
  * `DOSEmailNotify` to bardzo fajna opcja, która pozwala na otrzymanie wiadomości email na określony adres mailowy w razie ataku. Spróbuj ustawić tutaj swój adres mailowy.

Po ustawieniu wszystkich opcji zresetuj serwer komendą `sudo systemctl reload apache2` lub wyłącz go i włącz ponownie. Twórcy evasive_mode wprowadzili z jego plikami test, który przetestuje twoją konfigurację, symulując atak. Uruchom go poprzez wykonanie komendy `perl /usr/share/doc/libapache2-mod-evasive/examples/test.pl`. Output powinien wyglądać tak:
```
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
HTTP/1.1 403 Forbidden
```

Następnie sam przetestuj działanie, zaatakuj serwer komendą `sudo hping3 -S --flood --rand-source -V -p 80 <adres_maszyny_broniącego>`. Możesz to zrobić z wielu terminali naraz, symulując atak DDoS. Sprawdź teraz, czy mail przychodzi na twoją skrzynkę, czy strona dalej działa. Spróbuj wejść na stronę na swopim głownym systemie, odświeżyć ją kilka razy. Sprawdź czy blokowanie działa na ustawiony przez ciebie czas. Jeśli wszystko działa - Gratulacje - twój serwer apache2 jest bezpieczny atakami DoS!
