# 1. Types of Tests
## 1.1. The Test Pyramid
```
                    ┌───────────┐
                    │   E2E     │  ← Few: slow, expensive, fragile
                    │  Tests    │     (Selenium, Cypress, Playwright)
                    ├───────────┤
                  ┌─┤Integration├─┐  ← Some: test wiring between components
                  │ │  Tests    │ │     (DB, HTTP, Kafka, Spring context)
                  │ ├───────────┤ │
              ┌───┤ │   Unit    │ ├───┐  ← Many: fast, focused, isolated
              │   │ │  Tests    │ │   │     (Mockito, plain JUnit)
              │   │ └───────────┘ │   │
              └───┴───────────────┴───┘
```

|Level|What It Tests|Speed|Dependencies|Confidence|
|---|---|---|---|---|
|**Unit**|Single class/method in isolation|⚡ ~ms|None (mocked)|Logic correctness|
|**Integration**|Multiple components wired together|🐢 ~seconds|Real DB, HTTP, queues|Wiring correctness|
|**E2E**|Full user flow through the entire system|🐌 ~minutes|Everything real|System works end-to-end|
## 1.2. Integration Testing
Integration tests verify that **multiple components work together correctly** — the wiring between your code and real infrastructure (database, HTTP endpoints, message queues).
```Java
// Integration test — starts real Spring context + in-memory DB
@SpringBootTest
@AutoConfigureMockMvc
@Transactional   // rolls back after each test — clean state
class OrderControllerIntegrationTest {

    @Autowired private MockMvc mockMvc;
    @Autowired private OrderRepository orderRepo;

    @Test
    void createOrder_savesToDatabase_andReturns201() throws Exception {
        String requestBody = """
            {
                "productId": "PROD-1",
                "quantity": 2,
                "customerId": "CUST-100"
            }
            """;

        mockMvc.perform(post("/api/orders")
                .contentType(MediaType.APPLICATION_JSON)
                .content(requestBody))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.orderId").exists())
            .andExpect(jsonPath("$.status").value("PENDING"));

        // Verify side effect — data actually persisted
        assertEquals(1, orderRepo.count());
        Order saved = orderRepo.findAll().get(0);
        assertEquals("PROD-1", saved.getProductId());
        assertEquals(2, saved.getQuantity());
    }

    @Test
    void createOrder_invalidPayload_returns400() throws Exception {
        mockMvc.perform(post("/api/orders")
                .contentType(MediaType.APPLICATION_JSON)
                .content("{}"))  // missing required fields
            .andExpect(status().isBadRequest())
            .andExpect(jsonPath("$.errors").isNotEmpty());
    }
}
```