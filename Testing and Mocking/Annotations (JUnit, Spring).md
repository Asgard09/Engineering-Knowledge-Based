# 1. The Big Picture
When writing tests in a Spring Boot project, you deal with **three layers** of annotations:
```
┌────────────────────────────────────────────────────────────┐
│  Layer 1: JUnit 5 (Test Runner)                            │
│  @Test, @ParameterizedTest, @BeforeEach, @DisplayName      │
│  → Controls WHAT runs, WHEN, and HOW                       │
├────────────────────────────────────────────────────────────┤
│  Layer 2: Mockito (Test Doubles)                           │
│  @Mock, @Spy, @InjectMocks, @Captor                        │
│  → Controls DEPENDENCIES — replace real objects with fakes │
├────────────────────────────────────────────────────────────┤
│  Layer 3: Spring Boot Test (Application Context)           │
│  @SpringBootTest, @WebMvcTest, @DataJpaTest, @MockitoBean  │
│  → Controls SPRING CONTEXT — how much of the app loads     │
└────────────────────────────────────────────────────────────┘
```
# 2. Layer 1 : JUnit 5 Core Annotations
## 2.1. Lifecycle Annotations
```Java
class OrderServiceTest {

    @BeforeAll   // Runs ONCE before all tests (must be static)
    static void setupOnce() {
        // Expensive one-time setup: start Testcontainers, load CSV, etc.
    }

    @BeforeEach  // Runs BEFORE each test method
    void setup() {
        // Reset mocks, prepare test data, clean state
    }

    @Test
    void shouldCalculateTotal() { /* test logic */ }

    @AfterEach   // Runs AFTER each test method
    void tearDown() {
        // Close resources, clear caches
    }

    @AfterAll    // Runs ONCE after all tests (must be static)
    static void cleanupOnce() {
        // Stop containers, delete temp files
    }
}
```

```
Execution order for 2 test methods:

  @BeforeAll ──────────────────────────────────────────
       │
       ├── @BeforeEach → @Test (test1) → @AfterEach
       │
       ├── @BeforeEach → @Test (test2) → @AfterEach
       │
  @AfterAll ───────────────────────────────────────────
```
# 3. Layer 2 : Mockito Annotations
|Annotation|What It Creates|When To Use|
|---|---|---|
|`@Mock`|A mock (all methods return defaults)|Replace a dependency you don't want to call|
|`@Spy`|A spy wrapping a real object|Need real behavior + override one method|
|`@InjectMocks`|Real object with mocks injected|Auto-wire `@Mock`/`@Spy` into the class under test|
|`@Captor`|An `ArgumentCaptor`|Capture arguments passed to a mock for detailed assertions|
## 3.1. Common Mockito References
```Java
// ── Stubbing ──────────────────────────────────────────────
when(mock.method(arg)).thenReturn(value);          // return value
when(mock.method(arg)).thenThrow(new RuntimeException());  // throw
when(mock.method(arg)).thenAnswer(invocation -> {  // dynamic
    String arg0 = invocation.getArgument(0);
    return arg0.toUpperCase();
});

// ── Verification ──────────────────────────────────────────
verify(mock).method(arg);                     // called exactly once
verify(mock, times(3)).method(arg);           // called exactly 3 times
verify(mock, never()).method(arg);            // never called
verify(mock, atLeastOnce()).method(any());    // at least once
verifyNoMoreInteractions(mock);               // no other calls
verifyNoInteractions(mock);                   // zero calls total

// ── Argument Matchers ─────────────────────────────────────
when(repo.findById(any())).thenReturn(...);   // any argument
when(repo.findByName(eq("alice"))).thenReturn(...); // exact value
when(repo.findByAge(anyInt())).thenReturn(...);
when(repo.findByName(argThat(name -> name.startsWith("A")))).thenReturn(...);
```

>We need when because Mockito creates a fake version of that service. It inherits the method names, but **all the actual code inside those methods is deleted**.
# 4. Layer 3 : Spring Boot Test Annotation
## 4.1. @DataJpaTest - Repository Layer Only
```Java
@DataJpaTest     // auto-configures: JPA, in-memory H2, @Entity scanning
@AutoConfigureTestDatabase(replace = Replace.NONE)  // use Testcontainers instead
class OrderRepositoryTest {

    @Autowired private OrderRepository orderRepo;
    @Autowired private TestEntityManager em;

    @Test
    void findByStatus_returnsMatchingOrders() {
        em.persist(new Order("PROD-1", 2, "PENDING"));
        em.persist(new Order("PROD-2", 1, "SHIPPED"));
        em.persist(new Order("PROD-3", 3, "PENDING"));
        em.flush();

        List<Order> pending = orderRepo.findByStatus("PENDING");

        assertEquals(2, pending.size());
        assertTrue(pending.stream().allMatch(o -> o.getStatus().equals("PENDING")));
    }

    @Test
    void customQuery_calculatesRevenueByCategory() {
        // Test your @Query methods against real SQL
        em.persist(new Order("laptop", 1, "COMPLETED", new BigDecimal("1299")));
        em.persist(new Order("laptop", 1, "COMPLETED", new BigDecimal("999")));
        em.flush();

        BigDecimal revenue = orderRepo.calculateRevenueByCategory("laptop");
        assertEquals(new BigDecimal("2298"), revenue);
    }
}
```

> When defining a constructor, JUnit is responsible for creating the test class instance. In my case, the constructor requires a `UserRepository`, but JUnit doesn't know how to create or provide that dependency. When using `@Autowired`, Spring injects the `UserRepository` into the test class, allowing the test to run successfully.

