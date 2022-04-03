+++
title = "Obsługa endpointa dla uczestników"
description = ""
weight = 12
+++

1. Do tej pory możemy tylko listować uczestników. Dodaj endpoint który pozwoli na pobranie jednego wskazanego uczestnika. W tym celu zaimplementuj metodę `ParticipantService.findByLogin(String login)` oraz odpowiedniego endpointa w klasie `ParticipantRestController`. Poniżej kod takiego endpointa:
    ```java
    @RequestMapping(value = "/{id}", method = RequestMethod.GET)
    public ResponseEntity<?> getParticipant(@PathVariable("id") String login) {
     Participant participant = participantService.findByLogin(login);
     if (participant == null) {
         return new ResponseEntity(HttpStatus.NOT_FOUND);
     }
     return new ResponseEntity<Participant>(participant, HttpStatus.OK);
    }
    ```
1. Jeśli chcesz żeby za każdym razem nie trzeba było restartować serwera, dodaj nową zależność do swojego ```pom.xml```:
    ```xml
    <dependency>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-devtools</artifactId>
	</dependency>
    ```
    {{% notice tip %}}
    Nie zapomnij przeładować całego projektu w IDE, tak aby wprowadzone zmiany zostały zastosowane.
    {{% /notice %}}
    
1. Zaimplementuj dodawanie uczestników. Metoda powinna zostać zadeklarowana w sposób następujący:
    ```java
   @RequestMapping(value = "", method = RequestMethod.POST)
   public ResponseEntity<?> registerParticipant(@RequestBody Participant participant)
    ```
1. Obsłuż sytuację kiedy dany uczestnik już istnieje. W tym celu należy wykorzystać funkcję `findByLogin`. W przypadku istnienia danego użytkownika należy wyjść z funkcji w następujący sposób:
    ```java
   return new ResponseEntity("Unable to create. A participant with login " + participant.getLogin() + " already exist.", HttpStatus.CONFLICT);
    ```
1. Żeby sprawdzić działanie wykorzystaj Postmana albo następującą komendę:
    ```java
   curl -H "Content-Type: application/json" -d '{"login":"somelogin", "password": "some password"}' localhost:8080/participants
    ```
1. Analogicznie zaimplementuj samodzielnie usuwanie uczestników oraz ich aktualizację.