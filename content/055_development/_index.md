+++
title = "Dalszy rozwój"
description = ""
weight = 35
+++

1. Czas na dalszy rozwój aplikacji! Dodaj dodatkowe endpointy umożliwiające:
    - dodawanie uczestnika do spotkania (uczestnik musi być wcześniej zarejestrowany w systemie)
    - usuwanie uczestnika ze spotkania
    - pobieranie uczestników zarejestrowanych w spotkaniu 
1. Wszystkie niezbędne serwisy powinny być już dostępne
1. Przykłady użycia endpointów:
    - ```GET meetings/{id}/participants``` - pobiera zarejestrowanych uczestników spotkania
    - ```POST meetings/{id}/participants``` - dodaje uczestnika do spotkania
    - ```DELETE meetings/{id}/participants/{login}``` - usuwa uczestnika ze spotkania

