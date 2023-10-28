---
title: JWT,SpringSecurity
category: TIL
order: 1
---
### Spring Security 란?

<div class="content-box">
Authenticatio(인증) : 특정 대상이 누구인지 확인하는 절차</br>
Authorization(권한) : 인증된 주체가 특정한 곳에 접근 권한을 확인
</div>

##### SpringSecurity 인증 구조

![Sqrt Arch](https://drive.google.com/uc?id=1_NQ4TNlTgvzcGalSDAlNWCJES3N8hQLY)

- *1.* `Http Request` -> `Server`
- *2.* `AuthenticationFilter` 가 `Http Request` 수신
- *3.* `AuthenticationFilter`가 `Request`의 `Id`, `Password`를 이용해 `AuthenticationToken` 생성
- *4.* `AuthenitcationManager`가 토큰 수신
- *5.* `AuthenticationManager` -> `AuthenticationProvider` 토큰 전달
- *6.* `AuthenticationProvider` -> `UserDetailsService` 토큰의 ID 전달 , `UserDetailsService` 는 DB 의 회원정보 `UserDetails` 객체로 받아 반환
- *7.* `AuthenticationProvider` 가 `UserDetails` 와 실제 사용자 입력정보 비교
- *8.* 사용자 정보를 가진 `Authentication` 객체를 `SecurityContextHolder`에 담은 이후 `AuthenticationSuccessHandle`를 실행. 이후 복귀


### JWT(Json Web Token)

##### JWT 란?
<div class="content-box">
JWT는 일반적으로 클라이언트와 서버 통신 시 권한 인가(Authorization)을 위해 사용하는 토큰<br>
JSON 객체를 사용하여 토큰 자체에 정보를 저장하는 웹토큰</br>
다른 인증 방식들에 비해 가볍고 간편해서 유용한 인증 방식으로 알려져 있음.
</div>

||장점|단점|
|-|-|-|
||중앙의 인증서버, 데이터 스토어에 대한 의존성 없음|Payload의 정보가 많아지면 네트워크 사용량이 증가|
||Base64 URL Safe Encoding을 사용하기 떼문에 URL, Cookie, Header 모두 사용 가능|Token이 클라이언트에 저장되기 때문에 서버에서 클라이언트의 토큰 조작 불가능|

### Spring Security + JWT 동작

![Sqrt Arch](https://drive.google.com/uc?id=1YYiXNdWHMfrcpCIv2FUoLUIOJnuYXf_S)

##### SecurityConfig 예제
```java
@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig {

    private final JwtTokenFilter jwtTokenFilter;

    /* Spring Security 필터 체인 설정 */
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {

        http
                .csrf().disable()
                .cors(Customizer.withDefaults())
                .headers(headers -> headers.frameOptions().disable())
                .addFilterBefore(jwtTokenFilter, UsernamePasswordAuthenticationFilter.class)
                .authorizeRequests(authorize -> authorize
                        .antMatchers("/**")
                        .permitAll()
                        .anyRequest()
                        .authenticated()
                );

        return http.build();

    }

    /* 비밀번호 암호화에 사용할 BcryptPasswordEncoder 빈 등록*/
    @Bean
    public BCryptPasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

}
```
##### JWT TOKEN Filter 예제
```java
@AllArgsConstructor
@Component
public class JwtTokenFilter extends OncePerRequestFilter {

    private JwtTokenProvider jwtTokenProvider;

    // 필터링 White-List (개발 초기 임시로 모두 생략)
    private static final String[] ALL_WHITELIST = {"/**/"};

    /* White-List 대상 판별 (필터링 대상 인지 아닌지 판별) */
    private boolean isFilterCheck(String requestURI) {
        return !PatternMatchUtils.simpleMatch(ALL_WHITELIST, requestURI);
    }

    /* 토큰 가져오기 */
    private String getTokenFromRequest(HttpServletRequest request) {
        String bearerToken = request.getHeader(AUTHORIZATION);
        if (bearerToken != null && bearerToken.startsWith("Bearer ")) {
            // Bearer prefix 지우고 반환
            return bearerToken.substring(7);
        }
        return null;
    }
    @Override
    protected void doFilterInternal(HttpServletRequest request,
                                    HttpServletResponse response,
                                    FilterChain filterChain) throws ServletException, IOException {

        String token = getTokenFromRequest(request);

        try {
            // 화이트리스트에 있는 경우에는 필터링을 건너뛰어서 다음 필터로 진행
            if (isFilterCheck(request.getRequestURI())) {
                // 화이트리스트에 없는 경우에만 검증 처리
                if (token != null) {
                    SecurityContextHolder
                            .getContext()
                            .setAuthentication(jwtTokenProvider.getAuthentication(token));
                }
            }
            filterChain.doFilter(request, response);
        } catch (Exception e) {
            e.printStackTrace();
            response.sendError(HttpServletResponse.SC_UNAUTHORIZED, e.getMessage());
        }

    }
}
```

##### JWT TOKEN Provider 예제

```java
@Slf4j
@RequiredArgsConstructor
@Component
public class JwtTokenProvider {

    private final UserDetailsServiceImpl userDetailsService;

    @Value("${token.key}")
    private String issuer;
    private SecretKey secretKey;

    /* SecretKey 초기화 메서드 (빈 생성 시 1회 실행) */
    @PostConstruct
    public void init() {
        secretKey = Keys.secretKeyFor(SignatureAlgorithm.HS256);
    }

    /* Access Token 생성 메서드 - 클레임에 이메일과 UserRole 삽입 */
    public String generateAccessToken(TokenIssuanceDTO tokenTokenIssuanceDTO) {
        Claims claims = Jwts.claims().setSubject(tokenTokenIssuanceDTO.getId().toString());
        claims.put("email", tokenTokenIssuanceDTO.getEmail());
        claims.put("userRole", tokenTokenIssuanceDTO.getUserRole().getRoleName());

        return Jwts.builder()
                .setClaims(claims)
                .setIssuer(issuer)
                .setIssuedAt(new Date(System.currentTimeMillis()))
                // 유효 기간 1시간
                .setExpiration(new Date(System.currentTimeMillis() + (60 * 60 * 1000)))
                .signWith(secretKey)
                .compact();
    }

    /* Refresh Token 생성 메서드 - 클레임에 이메일 삽입 (추후 엑세스 토큰 재발급 시 사용예정)*/
    public String generateRefreshToken(String email) {
        return Jwts.builder()
                .claim("email", email)
                .setIssuer(issuer)
                .setIssuedAt(new Date(System.currentTimeMillis()))
                // 유효 기간 24시간
                .setExpiration(new Date(System.currentTimeMillis() + (24 * 60 * 60 * 1000)))
                .signWith(secretKey)
                .compact();
    }

    /* 토큰 유효성 검증 */
    public boolean validateToken(String token) {
        try {
            Jwts.parserBuilder()
                    .setSigningKey(secretKey)
                    .build()
                    .parseClaimsJws(token);
            return true;
        } catch (JwtException ex) {
            return false;
        }
    }

    /* 토큰에서 Id 추출 */
    public Long getIdFromToken(String token) {
        if (token.startsWith("Bearer ")) {
            token = token.substring(7);
        }
        return Long.parseLong(Jwts.parserBuilder()
                .setSigningKey(secretKey)
                .build()
                .parseClaimsJws(token)
                .getBody()
                .getSubject());
    }

    /* 토큰에서 이메일 추출 */
    public String getEmailFromToken(String token) {
        if (token.startsWith("Bearer ")) {
            token = token.substring(7);
        }
        return Jwts.parserBuilder()
                .setSigningKey(secretKey)
                .build()
                .parseClaimsJws(token)
                .getBody()
                .get("email", String.class);
    }

    /* 토큰 인증 */
    public Authentication getAuthentication(String token) {

        // "Bearer " 프리픽스 제거
        if (token.startsWith("Bearer ")) {
            token = token.substring(7);
        }

        // 토큰을 파싱하여 클레임 추출
        Claims claims = Jwts.parserBuilder()
                .setSigningKey(secretKey)
                .build()
                .parseClaimsJws(token)
                .getBody();

        // 클레임에서 이메일과 사용자 역할 가져오기
        String email = claims.get("email", String.class);
        UserRole userRole = UserRole.valueOf(claims.get("userRole", String.class));

        // 사용자 정보 로드
        UserDetails userDetails = userDetailsService.loadUserByUsername(email);

        // 사용자 권한과 역할 권한을 병합하여 Authentication 객체 생성
        List<GrantedAuthority> authorities = new ArrayList<>(userDetails.getAuthorities());
        authorities.addAll(userRole.getAuthorities());

        return new UsernamePasswordAuthenticationToken(
                userDetails, "", authorities
        );
    }

}
```


