+++
title = "Dodajemy autentykację"
description = ""
weight = 50
+++

1. Czas przejść do autentykacji. A może autoryzacji? Czy to jest to samo czy czymś się jednak różni?
{{% notice info %}}
**Uwierzytelnianie** i **autoryzacja** to dwa kluczowe procesy bezpieczeństwa informacji, które administratorzy wykorzystują do ochrony systemów i informacji. Uwierzytelnianie weryfikuje tożsamość użytkownika lub usługi, a autoryzacja określa jego prawa dostępu.
{{% /notice %}}
1. Do weryfikacji tożsamości obecnie najczęściej wykorzystuje się tokeny, a w szczególności tokeny JWT. Tokeny te zawierają komplet informacji o użytkowniku. Serwer nie musi posiadać przetwarzać dodatkowych informacji. Działają więc na zasadzie dowodu tożsamości.
![JWT Diagram](/jwt-diagram.png)
![JWT structure](/jwt.png)
1. Więcej informacji znajdziesz na stronie https://jwt.io.
1. Naszym celem jest więc zaimplementowanie nowego endpointa, który na podstawie przesłanych danych wygeneruje nam odpowiedni token.
1. W tym celu dodaj do projektu bibliotekę dodającą wsparcie dla JWT: ```java-jwt```.
1. Dodaj klasę do pakietu ```security``` pozwalającą na ładowanie użytkowników przez system autentykacji Springa:
```java
package com.company.enroller.security;

import com.company.enroller.model.Participant;
import com.company.enroller.persistence.ParticipantService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.stereotype.Service;

import java.util.Collections;

@Service
public class ParticipantProvider implements UserDetailsService {
   @Autowired
   private ParticipantService participantService;

   @Override
   public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
       Participant participant = participantService.findByLogin(username);
       if (participant == null) {
           throw new UsernameNotFoundException(username);
       }
       return new User(participant.getLogin(), participant.getPassword(), Collections.emptyList());
   }
}
```
1. W klasie WebSecurity wstrzyknij wymagane zależności i skonfiguruj system autentykacji nadpisując odpowiednią metodę:
```java
@Override
public void configure(AuthenticationManagerBuilder auth) throws Exception {
   auth.userDetailsService(participantProvider).passwordEncoder(passwordEncoder);
}
```
1. W katalogu ```src/main/resources``` utwórz plik ```application.properties``` z konfiguracją aplikacji:
```
security.secret = ThisIsNotSoSecretSoChangeIt
security.issuer = https://myenrollerdemo-twoj_login.b4a.run
security.token_expiration_in_seconds = 3600
```
1. Do pakietu ```security``` dodaj nową klasę. Będzie to filtr requestów, który wystawi token JWT gdy klient prześle poprawne dane logowania:
```java
package com.company.enroller.security;

import com.auth0.jwt.JWT;
import com.auth0.jwt.algorithms.Algorithm;
import com.company.enroller.model.Participant;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.springframework.http.HttpMethod;
import org.springframework.security.authentication.AuthenticationManager;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.web.authentication.AbstractAuthenticationProcessingFilter;
import org.springframework.security.web.util.matcher.AntPathRequestMatcher;

import javax.servlet.FilterChain;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.util.ArrayList;
import java.util.Date;

public class JWTAuthenticationFilter extends AbstractAuthenticationProcessingFilter {
   private AuthenticationManager authenticationManager;
   private final String secret;
   private final String issuer;
   private final int tokenExpiration;

   public JWTAuthenticationFilter(AuthenticationManager authenticationManager, String secret, String issuer, int tokenExpiration) {
       super(new AntPathRequestMatcher("/tokens", HttpMethod.POST.name()));
       this.authenticationManager = authenticationManager;
       this.secret = secret;
       this.issuer = issuer;
       this.tokenExpiration = tokenExpiration;
   }

   @Override
   public Authentication attemptAuthentication(HttpServletRequest req, HttpServletResponse res) throws AuthenticationException {
       try {
           Participant participant = new ObjectMapper().readValue(req.getInputStream(), Participant.class);
           Authentication authentication = new UsernamePasswordAuthenticationToken(participant.getLogin(), participant.getPassword(), new ArrayList<>());
           return authenticationManager.authenticate(authentication);
       } catch (IOException e) {
           throw new BadCredentialsException("Invalid login request.", e);
       }
   }

   @Override
   protected void successfulAuthentication(HttpServletRequest req, HttpServletResponse res, FilterChain chain, Authentication auth) throws IOException {
       String login = ((UserDetails) auth.getPrincipal()).getUsername();
       LocalDateTime now = LocalDateTime.now();
       String token = JWT.create()
               .withIssuer(issuer)
               .withSubject(login)
               .withIssuedAt(Date.from(now.atZone(ZoneId.systemDefault()).toInstant()))
               .withExpiresAt(Date.from(now.plusSeconds(tokenExpiration).atZone(ZoneId.systemDefault()).toInstant()))
               .withClaim("role", "participant")
               .sign(Algorithm.HMAC256(secret));
       res.getWriter().write(String.format("{\"token\": \"%s\"}", token));
   }
}
```
1. W klasie WebSecurity w metodzie configure zarejestruj stworzony filtr:
    ```java
    .addFilterBefore(new JWTAuthenticationFilter(authenticationManager(), secret, issuer, tokenExpiration), UsernamePasswordAuthenticationFilter.class)
    ```
1. Potrzebne wartości wstrzyknij z konfiguracji aplikacji za pomocą adnotacji @Value, na zasadzie:
    ```java
    @Value("${security.secret}")
    String secret;
    ```
1. Przetestuj wystawianie tokena za pomocą Postmana. W tym przypomij sobie z poprzedniego kroku dane nowego użytkownika. Następnie wywołaj żądanie utworzenia nowego tokenu podając dane wstawionego użytkownika:
    ```
    curl -s -d '{"login":"janek", "password":"tajemne"}' -H "Content-Type: application/json" -X POST http://localhost:8080/api/tokens
    ```
    ![Token 1](/fetching-token-1.png)
1. Efektem powinien być otrzymany na wyjściu token.
    ![Token 2](/fetching-token-2.png)
1. Możesz go zdekodować na znanej już stronie https://jwt.io
    ![Token 3](/fetching-token-3.png)
1. Sprawdź kiedy token wygaśnie - zdekoduj datę wygaśnięcia np. przy pomocy strony: https://www.epochconverter.com
    ![Token 4](/fetching-token-4.png)
1. **Commit, push, deploy!**