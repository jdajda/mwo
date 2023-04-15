+++
title = "Endpointy dla spotkań"
description = ""
weight = 20
+++

Analogicznie do endpoint'ów dla uczestników zaimplementuj poniższe endpointy dla spotkań:
1. Pobieranie listy wszystkich spotkań
1. Pobieranie listy pojedyncznego spotkania
1. **Commit & Push** !

{{% notice tip %}}
1. Pamiętaj, że w tym celu musisz dodać nowy kontroller:
    ```java
    @RestController
    @RequestMapping("/meetings")
    public class MeetingRestController
    ```
1. Nie zapomnij również o odpowiednim serwisie:
    ```java
	@Autowired
	MeetingService meetingService;
    ```
{{% /notice %}}