+++
title = "Dodatkowe funkcje dla uczestników"
description = ""
weight = 13
+++

1. Rozszerz swoją implementację o moliwość sortowania uczestników po loginie. Dodatkowo chcemy mieć możliwość wyboru kolejności sortowania ***ASC*** lub ***DESC*** wg przykładów poniżej:
    1. Sortowanie listy wyników po loginie w kolejności malejącej ```http://localhost:8080/participants?sortBy=login&sortOrder=DESC```
    1. Sortowanie listy wyników po loginie w kolejności rosnącej ```http://localhost:8080/participants?sortBy=login&sortOrder=ASC```
    1. Sortowanie listy wyników w domyślnej kolejności (parametr ```sortOrder``` nie jest obowiązkowy): ```http://localhost:8080/participants?sortBy=login```

    Endpoint powinnien być odporny na podawanie błędnych/nie obsługiwanych wartości.

1. Dodaj parametr filtrowania listy wyników po loginie, zgodnie z poniższymi przykładami:
    1. ```http://localhost:8080/participants?key=login``` - pokaże tylko uczestników, które loginy zawierają słowo kluczowe ```login```
    1. ```http://localhost:8080/participants?key=og``` - pokaże tylko uczestników, które loginy zawierają słowo kluczowe ```og```
  
1. Skommituj kod i wyślij do swojego repozytorium: **Commit & Push** !

{{% notice tip %}}
1. By zmapować parametr z URI na parametr wywołania funkcji użyj adnotacji ```@RequestParam```. Poniżej przykład:
    ```java
    public ResponseEntity<?> someFunction(@RequestParam(value = "parametr_name", defaultValue = "") String param)		
    ```
1. Zadanie obróbki wyników zapytania przekaż do klas odpowiedzialnych za komunikację z bazą danych.
{{% /notice %}}

