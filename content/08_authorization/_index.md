+++
title = "Zabezpieczamy aplikację"
description = ""
weight = 30
+++

1. W tym punkcie chcemy zabezpieczyć endpointy przed niepowołowanym dostępem. Potrafimy już identyfikować użytkowników, czas z tego skorzystać.
1. Dodaj w tym celu kolejną klasę ```JWTAuthorizationFilter``` do pakietu ```security```:
    ```java
    package com.company.enroller.security;

    import com.auth0.jwt.JWT;
    import com.auth0.jwt.JWTVerifier;
    import com.auth0.jwt.algorithms.Algorithm;
    import org.springframework.security.authentication.AuthenticationManager;
    import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
    import org.springframework.security.core.Authentication;
    import org.springframework.security.core.context.SecurityContextHolder;
    import org.springframework.security.web.authentication.www.BasicAuthenticationFilter;

    import javax.servlet.FilterChain;
    import javax.servlet.ServletException;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;
    import java.io.IOException;
    import java.io.UnsupportedEncodingException;
    import java.util.ArrayList;

    public class JWTAuthorizationFilter extends BasicAuthenticationFilter {
        private static final String HEADER_NAME = "Authorization";
        private static final String TOKEN_PREFIX = "Bearer ";
        private final JWTVerifier verifier;

        public JWTAuthorizationFilter(AuthenticationManager authManager, String secret) throws UnsupportedEncodingException {
            super(authManager);
            verifier = JWT.require(Algorithm.HMAC256(secret)).acceptExpiresAt(0).build();
        }

        @Override
        protected void doFilterInternal(HttpServletRequest req, HttpServletResponse res, FilterChain chain) throws IOException, ServletException {
            String header = req.getHeader(HEADER_NAME);
            if (header != null && header.startsWith(TOKEN_PREFIX)) {
                String token = header.substring(TOKEN_PREFIX.length());
                Authentication authentication = extractUserFromToken(token);
                SecurityContextHolder.getContext().setAuthentication(authentication);
            }
            chain.doFilter(req, res);
        }

        private Authentication extractUserFromToken(String token) {
            String username = verifier.verify(token).getSubject();
            if (username != null) {
                return new UsernamePasswordAuthenticationToken(username, null, new ArrayList<>());
            }
            return null;
        }
    }
    ```
1. Zarejestruj ten filtr w konfiguracji zabezpieczeń:
    ```java
    .addFilter(new JWTAuthorizationFilter(authenticationManager(), secret))
    ```
1. Skonfiguruj mechanizm zabezpieczeń (klasa WebSecurity) tak, by nie pozwalał na korzystanie z aplikacji bez zalogowania się (poza endpointem wystawiającym token oraz rejestrującym użytkownika):
    ```java
    .authorizeRequests()
    .antMatchers(HttpMethod.POST, "/participants").permitAll()
    .antMatchers("/tokens").permitAll()
    .antMatchers("/**").authenticated()
    ```
1. Sprawdź, czy nadal możesz założyć konto i czy dostęp do ```/meetings``` rzeczywiście został zablokowany, jeśli użytkownik nie jest zalogowany. Zalogowanie to pozyskanie tokena, a następnie jego użycie w każdym zapytaniu do endpointa. W tym celu musisz go przesłać w nagłówku zapytania. Możesz to zrobić w Postmanie tak jak na załączonym obrazku:
    ![Authorization](/authorization.png)
1. Gotowe! **Commit, push, deploy!**
