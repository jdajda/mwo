+++
title = "Deployment"
description = ""
weight = 30
+++

1. Zbuduj aplikację za pomocą Mavena i fazy 'Package'. Zbudowaną paczkę uruchom z poziomu konsoli:
    ```java
    java -jar <path-to-your-project>/agh-mwo-enroller/target/enroller-0.0.1-SNAPSHOT.jar
    ```
1. Powinieneś zobaczyć, że aplikacja się uruchamia. Zakończ działanie aplikacji.
1. Spróbuj wywołać fazę Clean - katalog ```target``` powinien zostać usunięty.
1. Przyjrzyj się plikowi ```Dockerfile```. To plik konfiguracyjny, który pozwoli nam uruchomić naszą aplikację w wirtualnym środowisku za pomocą środowiska [Docker](https://www.docker.com) Docker umożliwia tworzenie, uruchamianie i zarządzanie aplikacjami w kontenerach. Kontenery to izolowane środowiska uruchomieniowe, które pozwalają na pakowanie aplikacji i ich zależności w spójny sposób. Dzięki temu aplikacja może działać w izolowanym środowisku bez konieczności instalacji dodatkowych zależności na maszynie hosta.
![Docker Logo](/docker-logo.png)
Docker umożliwia łatwe przenoszenie aplikacji między różnymi środowiskami, co ułatwia zarządzanie wdrażaniem aplikacji oraz zapewnia spójność między różnymi środowiskami, na których aplikacja jest uruchamiana. Dzięki temu Docker jest obecnie szeroko stosowany i znacznie ułatwia zarządzanie infrastrukturą i wdrażanie aplikacji.
1. Dockerfile w projekcie prezentuje przykład wieloetapowych buildów (Multi-stage builds). W pierwszej części pobiera on obraz Linuxowy (dystrybucja Amazon Linux) z zainstalowanym Mavenem po czym następuje budowanie projektu do postaci paczki JAR (mvn package). W drugiej części następujace skopiowanie paczki i jej uruchomienie. Jednocześnie w pliku deklarujemy, że maszyna udostępnia port 8080.
1. Czas na uruchomienie aplikacji na zdalnym serwerze. Spróbujmy wykorzystać darmowe środowisko oferowane przez https://www.back4app.com.
1. Zarejestruj się swoim kontem Github
![Deployment Step 1](/b4a-step1.png)
1. Dodaj nową aplikację, pomiając ankietę (SKIP)
![Deployment Step 2](/b4a-step2.png)
1. Wybierz opcję ```Containers as a Service```
![Deployment Step 3](/b4a-step3.png)
1. Znajdź i wybierz (Select) odpowiednie repozytorium na Githubie - najlepiej dać dostęp tylko do tego repozytorium
![Deployment Step 4](/b4a-step4.png)
1. Podaj nazwę i utwórz aplikację (Create App)
![Deployment Step 5](/b4a-step5.png)
1. Po tym kroku nastąpi pobranie Twojego kodu z repozytorium, pobranie obrazów na podstawie pliku Dockerfile, uruchomienie ich, pobranie zależności Mavena, zbudowanie produktu i uruchomienie aplikacji. Wszystko to zobaczysz w logach, które odświeżane są na żywo. Pożądanym efektem jest komunikat następującej treści:
    ```
    2023-04-14T23:26:36.552Z # VERIFYING QUEUE...
    2023-04-14T23:26:36.577Z # RELEASING DEPLOYMENT...
    2023-04-14T23:26:36.630Z # DEPLOYMENT READY
    ```
    Po jego zobaczeniu możesz przejść do swojej aplikacji:
    ![Deployment Step 6](/b4a-step6.png)
1. Przejdź do Postmana i sprawdź w nim działanie swojej upublicznionej aplikacji.
