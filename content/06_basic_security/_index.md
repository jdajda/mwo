+++
title = "Zabezpieczamy aplikację"
description = ""
weight = 40
+++

1. Czas zabezpieczyć aplikację. Po pierwsze wysyłamy hasła razem z loginami. I do tego są one w plain-text. Po drugie każdy może wyświetlić zawartość naszej bazy.
1. Na początek zabezpiecz aplikację przed wysyłaniem haseł. Wystarczy dodać odpowiednią adnotację w odpowiednim miejscu :-)
    ```java
    @JsonProperty(access = JsonProperty.Access.WRITE_ONLY)
    ```
1. Sprawdź efekt działania. Jak?
1. Dodajmy bibliotekę odpowiedzialną za bezpieczeństwo dla frameworka Spring: ```spring-boot-starter-security```. Jak to zrobić? Powinieneś to już wiedzieć ;-)
1. Następnie sprawdź efekt działania. Co zauważyłeś? Czy aplikacja działa? 
1. Czas rozbudować ochronę i zacząć szyfrować hasła. Wcześniej jednak musimy być w stanie cokolwiek w aplikacji zrobić, bo obecnie niewiele możemy. W tym celu pakiet ```Security``` a w nim klasę ```WebSecurity``` odpowiedzialną za kontrolę bezpieczeństwa:
    ```java
    package com.company.enroller.security;

    import org.springframework.security.config.annotation.web.builders.HttpSecurity;
    import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
    import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
    import org.springframework.security.config.http.SessionCreationPolicy;

    @EnableWebSecurity
    public class WebSecurity extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.csrf().disable()
            .authorizeRequests()
            .anyRequest().permitAll()
            .and()
            .sessionManagement().sessionCreationPolicy(SessionCreationPolicy.STATELESS);
        }
    }
    ```
1. Sprawdź teraz efekt działania. Co zauważyłeś?
1. Teraz dodaj funkcję która nam zakoduje/zashasuje hasło do głównej klasy ```App```:
    ```java
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
    ```
1. A następnie ją wykorzystaj tam gdzie dodajesz nowego użytkownika:
    ```java
    String hashedPassword = passwordEncoder.encode(participant.getPassword());
	participant.setPassword(hashedPassword);
    ```
1. Sprawdź efekt działania. Dodaj nowego użytkowika i zobacz czy jego hasło będzie zaszyfrowane w bazie danych. **Zapamiętaj lub zapisz podane hasło!**
1. Na koniec: **Commit, push, deploy!**
