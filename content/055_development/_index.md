+++
title = "Dalszy rozwój"
description = ""
weight = 35
+++

1. Czas na dalszy rozwój aplikacji! Dodaj dodatkowe endpointy umożliwiające:
    - dodawanie uczestników do spotkania
    - usuwanie uczestników ze spotkania
    - pobieranie uczestników w ramach spotkania. Uczestnicy muszą być wcześniej zarejestrowani w systemie
1. Wszystkie niezbędne serwisy powinny być już dostępne
1. Przykłady użycia endpointów:
    - ```GET meetings/{id}/participants``` - pobiera zarejestrowanych uczestników spotkania
    - ```POST meetings/{id}/participants``` - dodaje uczestnika/uczestników spotkania
    - ```DELETE meetings/{id}/participants/{login}``` - usuwa uczestnika ze spotkania

