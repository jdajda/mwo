+++
title = "Testy endpointów"
description = ""
weight = 15
+++

1. Przeanalizuj kod testów w klasie `ParticipantRestControllerTest`
1. Uzupełnij testy korzystając z poniższej podpowiedzi:
    ```java
    @Test
    public void addParticipant() throws Exception {
        Participant participant = new Participant();
        participant.setLogin("testlogin");
        participant.setPassword("testpassword");
        String inputJSON = "{\"login\":\"testlogin\", \"password\":\"somepassword\"}";

        given(participantService.findByLogin("testlogin")).willReturn((Participant)null);
        given(participantService.create(participant)).willReturn(participant);
        mvc.perform(post("/participants").content(inputJSON).contentType(MediaType.APPLICATION_JSON)).andExpect(status().isCreated());

        given(participantService.findByLogin("testlogin")).willReturn(participant);
        mvc.perform(post("/participants").content(inputJSON).contentType(MediaType.APPLICATION_JSON)).andExpect(status().isConflict());

        verify(participantService, times(2)).findByLogin("testlogin");
    }
    ```
1. Posługując się analogią, napisz testy dla aktualizacji i usuwania użytkowników.
