
# Spring Boot Security
## 설정
- dependency 에 spring-boot-starter-security, spring-security-test 추가 

## 구현
- Spring Boot Security 설정 파일 생성 - 예제는 Spring Boot 2.x 이므로 현재와 다름
- 설정 파일
	```java
	
	@EnableWebSecurity
	public class SecurityConfig extends WebSecurityConfigurerAdapter {
		@Override
		protected void configure(HttpSecurity security) throws Exception {
			// url에 대한 권한 설정
			security.authorizeRequests().antMathers("/").permitAll;
			security.authorizeRequests().antMatchers("/member/**").authenticated();
			security.authorizedRequests().antMatchers("/manager/**").hasRole("MANAGER");
			security.authorizedRequests().antMatchers("/admin/**").hasRole("ADMIN");
			
			security.csrd().disable();
			// AuthoirzedUrl은 HttpSecurity.authorizedRequest()의 리턴 객체 ExpressionInterceptUrlRegistry의 antMatchers() 메소드를 통해 얻어냄
			// AuthorizedUrl은 builder 패턴을 사용. 아래와 같이 축약해서 사용 가능
			//security.authorizeRequests()
			//    .antMatcher("/").permitAll()
			//    .antMatchers("/member/**").authenticated()
			//    .antMatchers("/manager/**").hasRole("MANAGER")
			//    .antMatchers("/admin/**").hasRole("ADMIN");
			
			// 사용자 화면 제공
			security.formLogin();
			// 사용자 정의 로그인 화면 접근 시 loginPage() 로 지정.
			// 로그인 성공시 화면 defaultSuccessUrl()  로 지정
			// security.formLogin().loginPage("/login").defaultSuccessUrl("/loginSuccess", true);
			
			// 권한 없는 사용자에게 보여줄 URL
			security.exceptionHandling().accessDeniedPage("/accessDenied");
			
			// logout 관련 설정
			security.logout().invalidateHttpSession(true).logoutSuccessUrl("/login");
		}
		
		
		/**
		 * 사용자 정보를 메모리에 저장
		 */
		@Autowired
		public void authenticate(AuthenticationManagereBuilder auth) throws Exception {
			auth.inMemoryAuthentication()
			   .withUser("manager").password("{noop}manager123")
			   .roles("MANAGER");
			
			auth.inMemoryAuthentication()
			   .withUser("admin").password("{noop}admin123")
			   .roles("ADMIN");
		}
	}
	```
- 사용자 정보를 메모리에 저장하고 사용하려면 authenticate() 메서드에 정의(임시용)
- AuthenticationManagerBuilder 객체를 @Autowired를 통해 authenticate 메서드에 주입함.
- AuthenticationManagerBuilder는 메모리, jdbc, ldap을 인증 방식 지원 

## Spring Security 동작 원리
- Security Filter를 통해 동작
- 여러 개의 security filter 를 filter chain 으로 제공 
- 주요 filter

| 필터                                   | 기능                                                                                            |
| ------------------------------------ | --------------------------------------------------------------------------------------------- |
| SecurityContextPersistenceFilter     | SecurityContextRepository에서 SecurityContext객체를 로딩하여 SecurityContextHolder에 저장하고 요청 처리가 끝나면 제거 |
| LogoutFilter                         | 지정한 경로의 요청이 들어오면 로그아웃하고 지정한 페이지로 이동                                                           |
| UsernamePasswordAuthenticationFilter | 로그인 요청이 들어오면 아이디/비밀번호 기반의 인증으 수행                                                              |
| DefaultLoginPageGeneratingFilter     | 로그인 요청이 들어오면 기본으로 제공되는 로그인 화면을 출력하고 이후 필터 진행 안함                                               |
| AnonymousAuthenticationFilter        | 임의 사용자에 해당하는 Authentication객체를 생성하여 Security Context에 설정                                      |
| SessionManagementFilter              | 세션 타임 아웃, 동시 접근 제어, 세션 고정 공격 등을 처리                                                            |
| ExceptionTranslationFilter           | FilterSecurityInterceptor에서 발생한 예외를 웹에 맞는 응답으로 변환                                             |
| FilterSecurityInterceptor            | 현재 사용자가 지정한 경로에 접근할 수 있는지 여부를 검사                                                              |
- 각 필터틑 등록된 순서대로 동작
- 중요한 필터
	- UsernamePasswordAuthenticationFilter가 가장 중요하고 인증 처리
	- FilterSecurityInterceptor 는 인증에 성공한 사용자가 해당 리소스에 접근할 권한이 있는지 검증

### 인증 정보 DB  연동
- Security 설정에서 authenticate() 를 기존 메모리에서 DB 연동으로 수정
	```java
	@EnableWebSecurity
	public class SecurityConfig extends WebSecurityConfigurerAdapter {
		@Autowired
		private DataSource dataSource;
		
		@Autowired
		public void authenticate(AuthenticationManagerBuilder auth) throws Exception {
				// 사용자 정보 조회
			String query1 = "select id, username, concat('{noop}', password) 
			password, true enabled from member where id = ?";
			
			// 권한 정보 조회 
			String query2 = "select id, role from member where id = ?";
			
			auth.jdbcAuthentication()
				.dataSource(dataSource);
				.userbyUsernameQuery(query1)
				.authoritiesByUsernameQuery(query2);
		}
	}
	```
- 조회한 사용자 정보를 org.springframework.security.core.userdetails.User 객체에 자동 매핑

#### JPA 방식으로 인증 구현
- 앞서 security 설정에서 authenticate() 메서드를 수정하여 DB에서 사용자 정보 조회하는 것외에 JPA를 사용해서 인증 구현하는 예제
1. Role 정보 enum 으로 구현
	```java
	public enum Role {
		ROle_ADMIN, ROLE_MANAGER, ROLE_MEMBER
	}
	```
2. Role을 사용하는 회원 엔티티 구현
	```java
	@Getter
	@Setter
	@ToString
	@Entity
	public class Member {
		@Id
		private String id;
		private String password;
		private String name;
		
		// EnumType.STRING 으로 설정해서 권한에 해당하는 값이 문자열로 저장됨
		@Enumerated(EnumType.STRING)
		private Role role;
		private boolean enabled;
	}
	```
3. JPA Repository 구현
	```java
	public interface MemberRepository extends CrudRepository<Member, String> {
	}
	```
4. 사용자 정의 UserDetailsSevice 구현
	- 인증 관리 필터를 통과하려면 사용자 정보가 저장된 UserDetails 객체 필요
	- UserDetailsService가 UserDetails 객체에 사용자 정보를 저장
	- Spring Boot는 UserDetailsService를 구현한 클래스를 기본적으로 제공 
	- 사용자 정의 UserDetailService를 제공하고 싶으면 상속받아 작성
	```java
	@Service
	public class BoardUserDetailsService implements UserDetailsService {
		@Autowired
		private MemberRepository memberRepo;
		
		// loadUserByUsername 메서드 재정의 필요 
		@Override
		public UserDetails loadUserByUsername(String username) throws UsernameNotFoundExcption {
			// MemberRepository로 화면 정보를 조회하여 UserDetails 타입 객체 리턴
			
			return null;
		}
	}
	```
5. 인증 관리 필터에서는 UserDetils 객체가 필요함. but memberRepo는 Member 객체를 리턴함. Member를 UserDetails 를 상속받는 객체로 재선언하거나, User 객체를 상속받으면 처리할 내용이 많음
	```java
	
	// UserDetails에서 상속받는 메서드들을 override 정의
	public class Member implemnts UserDetails {
		@Override
		public Collection<? extends GrantedAuthority> getAuthorities() {
			return null;
		}
		
		@Override
		public String getUsername(){
			return null;
		}
		
		@OVerride
		public boolean isAccountNonExpired() {
			return false;
		}
		
		@Override
		public boolean isAccountNonLocked() {
			return false;
		}
		
		@Override
		public boolean isCredentialsNonExpired() {
			return false;
		}
		
		@Override
		public boolean isEnabled() {
			return false;
		}
	}
	```
	-  User 객체를 상속받는 별개의 클래스를 선언해서 Member  에서 해당 값을 복사하는 방식으로 구현
	```java
	import org.springframework.security.core.userdetails.User;
	
	public class SecurityUser extends User {
		private static final long serialVersionUID = 1L;
		
		public SecurityUser(Member member) {
			super(member.getId(), "{noop}" + member.getPassword(),
			    AuthorityUtils.createAuthorityList(member.getRole().toString()));
		}
	}
	```
	- SecurityUser를 사용하도록 UserDetailsService 수정
	```java
	@Service
	public class BoardUserDetailsService implements UserDetailsService {
		@Autowired
		private MemberRepository memberRepo;
		
		@Override
		public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
			Optional<Member> optional = memberRepo.findById(username);
			if(!optional.isPresent()) {
				throw new UsernameNotFoundException(username + " 사용자 없음");
			} else {
				Member member = optional.get();
				return new SecurityUser(member);
			}
		}
	}
	```

1. Security 설정에 UserDetailsService 적용
	```java
	@EnableWebSecurity
	public class SecurityConfig extends WebSecurityConfigurerAdapter {
		@Autowired
		private BoardUserDetailsService boardUserDetailsService;
		
		@Override
		protected void configure(HttpSecurity security) throws Exception {
			
			security.exceptionHandling().accessDeniedPage("/accessDenied");
			security.logout().invalidateHttpSession(true).logoutSuccessUrl("/login");
			
			security.userDetailsService(boardUserDetailsService);
		}
		
		// authenticate() 메서드 삭제 
		
		// 비밀번호 암호화 추가
		@Bean
		public PasswordEncode passwordEncoder() {
			return PasswordEncodeFactories.createDelegationPasswordEncoder();
		}
	}
	```