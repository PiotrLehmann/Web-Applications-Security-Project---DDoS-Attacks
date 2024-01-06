# Ataki DDoS i Obrona Przed Nimi
## Zalecane przed Labem (przygotowanie środowiska)
1. Potrzebne bedą `dwie maszyny wirtualne` (`atakujący` i `broniący` serwera), polecamy do tego `VMWare` lub `VirtualBox`
   $ [Jak zainstalować VMWare?](https://www.youtube.com/watch?v=PoNPBdKLZdk)
   $ [Jak zainstalować Ubuntu na maszynie wirtualnej?](https://www.youtube.com/watch?v=NhlhJFKmzpk&t=261s)
     
2. Na obydwu maszynach wskazane jest wykonanie poniższych komend
   $ `sudo apt-get update`
   $ `sudo apt install net-tools`

## Zadanie 1
To zadanie będzie polegało na skonfigurowaniu i uruchomieniu web-serwera Apache2, który będzie serwował swoją podstawową stronę, pod adresem naszej wirtualnej maszyny. Efektem wykonania zadania powinna być możliwość otworzenia strony 'pod adresem naszej maszyny wirtualnej' na windowsie. To właśnie na windowsie będziemy obserwować efekt ataku DoS, oraz czy obrona przed nim zadziałała.

