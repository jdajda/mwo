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
1. Czas na uruchomienie aplikacji na zdalnym serwerze. Spróbujmy wykorzystać darmowe środowisko oferowane przez https://render.com.
1. Wejdź na stronę https://dashboard.render.com/login i zaloguj się swoim kontem Github
![Deployment Step 1](/render-step1.png)
1. Potwierdź konto klikając w link, który przyszedł na emaila. **Ankietę, którą zobaczysz możesz pominąć**
1. Wybierz plan *Hobby*
![Deployment Step 2](/render-step2.png)
1. Wybierz opcję New->Web Service
![Deployment Step 3](/render-step3.png)
1. Znajdź i wybierz (Select) odpowiednie repozytorium na Githubie
![Deployment Step 4](/render-step4.png)
![Deployment Step 5](/render-step5.png)
![Deployment Step 6](/render-step6.png)
1. Wybierz nowo dodane repozytorium z listy
![Deployment Step 7](/render-step7.png)
1. Przejdziesz do ekranu konfiguracji **You are deploying a Web Service**. Przewiń niżej i wybierz darmowy plan dla projektów hobbystycznych.
![Deployment Step 8](/render-step8.png)
1. Przewiń na sam dół i kliknij **Deploy Web Service**
1. Po tym kroku nastąpi pobranie Twojego kodu z repozytorium, pobranie obrazów na podstawie pliku Dockerfile, uruchomienie ich, pobranie zależności Mavena, zbudowanie produktu i uruchomienie aplikacji. Wszystko to zobaczysz w logach, które odświeżane są na żywo.
    ![Deployment Step 9](/render-step9.png)
1. Pożądanym efektem jest komunikat następującej treści:
    ```
    ==> Your service is live 🎉
    ```
    ![Deployment Step 10](/render-step10.png)
1. Po jego zobaczeniu możesz przejść do swojej aplikacji, której link znajdziesz u góry.
![Deployment Step 11](/render-step11.png)
1. Przejdź do przeglądarki lub IntelliJ i sprawdź w nim działanie swojej upublicznionej aplikacji.
