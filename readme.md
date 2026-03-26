# Opis charakteru danych
W ramach pewnego portalu internetowego zrzeszającego pasjonatów fotografii 
rejestrowane są zdjęcia jego użytkowników.

W strumieniu pojawiają się zdarzenia zgodne ze schematem `PhotoEvent`.

```
create json schema PhotoEvent(camera string, genre string, iso int, 
       width int, height int, ets string, its string);
```

Każde zdarzenie związane z jest z faktem wykonania i rejestracji 
przez użytkownika pojedynczej fotografii. 

Każda fotografia zawiera kilka podstawowych informacji, takich jak użyty 
do jej wykonania sprzęt, parametr ISO oraz rozmiar.

Ponadto, aby ułatwić wyszukiwanie zdjęć o tematyce, której poszukuje 
użytkownik, każdy obraz przypisany ma temat przewodni.

Dane uzupełnione są o dwie etykiety czasowe. 
* Pierwsza (`ets`) związana jest z momentem wykonania zdjęcia. 
  Etykieta ta może się losowo spóźniać w stosunku do czasu systemowego, 
  maksymalnie do 45 sekund.
* Druga (`its`) związana jest z momentem rejestracji zdarzenia w systemie.

# Opis atrybutów
- `camera` - marka oraz model użytego aparatu do zrobienia zdjęcia
- `genre` - rodzaj fotografii, temat przewodni, tematyka zdjęcia
- `iso` - wrażliwość na światło (im mniejsza wartość, tym mniejsza 
   wrażliwość na światło i tym samym ciemniejsze zdjęcie)
- `width` - szerokość zdjęcia, w pikselach
- `height` - wysokość zdjęcia, w pikselach
- `ets` - data i czas wykonania zdjęcia
- `its` - data i czas rejestracji zdarzenia

# Zadania
Opracuj rozwiązania poniższych zadań. 
* Opieraj się strumieniu zdarzeń zgodnych ze schematem `PhotoEvent`
* W każdym rozwiązaniu możesz skorzystać z jednego lub kilku poleceń EPL.
* Ostatnie polecenie będące ostatecznym rozwiązaniem zadania musi 
  * być poleceniem `select` 
  * posiadającym etykietę `answer`, przykładowo:
  ```aidl
    @name('answer') SELECT camera, genre, iso, width, height, ets, its
    from PhotoEvent#ext_timed(java.sql.Timestamp.valueOf(its).getTime(), 3 sec);
  ```

## Zadanie 1
Dla każdego gatunku (tematu przewodniego) fotografii znajdź medianę wartości 
ISO z ostatniej minuty rejestracji.

Wyniki powinny zawierać następujące kolumny:
- `genre` - gatunek zdjęcia oraz 
- `median_iso` - mediana wartości ISO.

## Zadanie 2
Wykrywaj zdjęcia o "niestandardowych" wymiarach. Za niestandardowy wymiar 
uznajemy sytuację, gdy:
- wysokość fotografii stanowi co najwyżej 10% jej szerokości, lub
- szerokość fotografii stanowi co najwyżej 10% jej wysokości

Wyniki powinny zawierać wszystkie kolumny zdarzenia:
- `camera` - marka oraz model użytego aparatu do zrobienia zdjęcia
- `genre` - rodzaj fotografii, temat przewodni, tematyka zdjęcia
- `iso` - wrażliwość na światło (im mniejsza wartość, tym mniejsza
  wrażliwość na światło i tym samym ciemniejsze zdjęcie)
- `width` - szerokość zdjęcia, w pikselach
- `height` - wysokość zdjęcia, w pikselach
- `ets` - data i czas wykonania zdjęcia
- `its` - data i czas rejestracji zdarzenia

## Zadanie 3
Wykrywaj zdjęcia, dla których wartość ISO mocno różni się od mediany dla 
danego gatunku (tematu) zdjęcia z ostatnich 5 minut rejestracji,
tzn. jest co najmniej 2 razy większa lub 2 razy mniejsza od mediany.

Wyniki powinny zawierać wszystkie kolumny zdarzenia:
- `camera` - marka oraz model użytego aparatu do zrobienia zdjęcia
- `genre` - rodzaj fotografii, temat przewodni, tematyka zdjęcia
- `iso` - wrażliwość na światło (im mniejsza wartość, tym mniejsza
  wrażliwość na światło i tym samym ciemniejsze zdjęcie)
- `width` - szerokość zdjęcia, w pikselach
- `height` - wysokość zdjęcia, w pikselach
- `ets` - data i czas wykonania zdjęcia
- `its` - data i czas rejestracji zdarzenia

## Zadanie 4
Porównuj ze sobą średnią wartość ISO między 5 ostatnio zarejestrowanymi 
zdjęciami dla tematyki weselnej (`Wedding`) oraz 5 ostatnio zarejestrowanymi 
zdjęciami dla tematyki `Beauty`.
Porównanie wyrażaj za pomocą stosunku średnich dla tematyki weselnej (`Wedding`) 
do tematyki `Beauty`.
Pomijaj przypadki, dla których ilorazu nie da się obliczyć (z powodu np. 
braku zdjęć o którejś tematyce).

Wyniki powinny zawierać następujące kolumny:

- `iloraz` - stosunek średniej wartości ISO dla tematyki 
   weselnej do średniej wartości ISO dla tematyki beauty

## Zadanie 5
Ograniczając się do analizy zdjęć o tematyce `Beauty`, wyszukuj 
informacje na temat serii zdjęć o długości co najmniej 2, w czasie 
której szerokość zdjęcia nie przekroczyła 5000. Dla każdej takiej serii 
wypisz datę rejestracji pierwszej fotografii, a także wartości 
ISO dwóch pierwszych elementów.

Wyniki powinny zawierać, następujące kolumny:

- `its_first` - data rejestracji pierwszego zdjęcia z serii
- `iso_first` - wartość ISO dla pierwszego zdjęcia z serii
- `iso_second` - wartość ISO dla drugiego zdjęcia z serii


## Zadanie 6
Poszukujemy średniej wartości ISO dla trzech kolejno (nie koniecznie 
bezpośrednio) zarejestrowanych zdjęć, dla których:
- zdjęcie poprzedzające pozostałe ma ISO większe od 1000
- zdjęcie kolejne ma wysokość większą od 200 pikseli
- zdjęcie trzecie ma ISO większe od 1000, a w międzyczasie
  (od drugiego do trzeciego zdjęcia) nie pojawia się żadna bardzo 
  ciemna fotografia (z ISO mniejszym od 100).

Wyniki powinny zawierać następujące kolumny:

- `mean` - średnia wartość ISO wszystkich trzech zdjęć
- `its_first` - czas rejestracji pierwszego zdjęcia z serii
- `its_second` - czas rejestracji drugiego zdjęcia z serii
- `its_third` - czas rejestracji trzeciego zdjęcia z serii

## Zadanie 7
Wykrywaj serie co najmniej dwóch rejestracji zdjęć, 
w przypadku których każda kolejna rejestracja
w serii ma coraz mniejszą wartość ISO. Dla każdej serii wypisz 
czas pierwszej i ostatniej rejestracji.

Wyniki powinny zawierać następujące kolumny:

- `its_start` - czas rejestracji pierwszego zdjęcia z serii
- `its_end` - czas rejestracji ostatniego zdjęcia z serii
