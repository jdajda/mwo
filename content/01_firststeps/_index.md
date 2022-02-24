+++
title = "Pierwsze kroki"
description = ""
weight = 11
+++

1. Sklonuj projekt: https://github.com/jdajda/agh-mwo-enroller-rest
1. Zaimportuj do Eclipse
1. Uruchom projekt (główna klasa: ```com.company.enroller.App```). Powinieneś zobaczyć w ostatnich linijkach konsoli mniej więcej taką linijkę: 
   ```java
   INFO 11920 --- [    main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
   ```
1. Zobacz czy serwis działa: [http://localhost:8080/participants](http://localhost:8080/participants). Powinieneś zobaczyć:
   ```java
   [{"login":"user2","password":"password"},{"login":"user3","password":"password"},{"login":"user4","password":"password"},{"login":"user5","password":"password"}]
   ```
1. Zobaczmy to samo z poziomu konsoli. Uruchom terminal (Git Bash) i wpisz
curl -s [http://localhost:8080/participants](http://localhost:8080/participants)
1. Sprawdź jaki kod odpowiedzi serwer zwraca adresu dla [http://localhost:8080](http://localhost:8080)
1. Pobierz i zainstaluj aplikację Postman https://www.getpostman.com
1. Zapoznaj się z aplikacją wywołując powyższe zapytania