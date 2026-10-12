# Good and Bad Tests (Java & Spring Guidelines)

## Good Tests

**Integration-style at the seam**: Test through real interfaces (HTTP Controller seam with `MockMvc` or Service seam with a real test database via Testcontainers), not mocks of internal parts.

```java
// GOOD: Tests observable behavior via Service or HTTP seam
@Test
void shouldCreateSpotWithCoordinatesAndRetrieveIt() {
    CreateSpotRequest request = new CreateSpotRequest(
        "Lenin Plaza",
        SpotType.SKATE,
        new Coordinates(55.7558, 37.6173)
    );

    SpotResponse created = spotService.createSpot(request);

    assertThat(created.id()).isNotNull();
    assertThat(created.title()).isEqualTo("Lenin Plaza");
    
    // Verify retrieval works (observable state)
    SpotResponse retrieved = spotService.getSpotById(created.id());
    assertThat(retrieved).isEqualTo(created);
}
```

Characteristics of Good Tests:
- Tests observable behavior that users/clients care about.
- Uses public APIs, records, DTOs, or HTTP endpoints.
- Survives internal refactorings (e.g. changing repository query logic or internal helper methods).
- Asserts on domain state and output, not on internal execution steps.
- Uses clear AssertJ assertions (`assertThat(...)`).

---

## Bad Tests

**1. Implementation-detail tests**: Coupled to internal Spring components and method calls.

```java
// BAD: Tests implementation details and internal mocks
@Test
void shouldCallRepositorySaveWhenCreatingSpot() {
    SpotRepository mockRepo = mock(SpotRepository.class);
    SpotService service = new SpotServiceImpl(mockRepo);

    service.createSpot(request);

    // Red flag: Verifying internal call count instead of observable behavior
    verify(mockRepo, times(1)).save(any(SpotEntity.class));
}
```

Red flags:
- Mocking repositories or domain entities.
- Testing private methods via reflection or package-private workarounds.
- Overusing `verify(mock, times(1)).doInternalThing()`.
- Test breaks when internal implementation changes even though external behavior is identical.

**2. Bypassing the interface to check database directly**:

```java
// BAD: Bypasses the application interface to inspect SQL table directly
@Test
void shouldSaveSpotDirectlyInDb() {
    spotController.createSpot(request);
    
    // Inspecting raw database table instead of using API/Service
    Integer count = jdbcTemplate.queryForObject("SELECT count(*) FROM spots WHERE name = 'Park'", Integer.class);
    assertThat(count).isEqualTo(1);
}

// GOOD: Verifies through the public interface
@Test
void shouldFindSpotByDisciplineViaApi() {
    spotController.createSpot(new CreateSpotRequest("Park", SpotType.BMX));
    
    List<SpotResponse> bmxSpots = spotController.getSpotsByDiscipline(SpotType.BMX);
    assertThat(bmxSpots).extracting(SpotResponse::title).contains("Park");
}
```

**3. Tautological tests**: Expected value recalculates the logic identically to production code.

```java
// BAD: Recalculates expected value with same formula
@Test
void shouldCalculateDistance() {
    double expected = formula(a, b);
    assertThat(calculator.distance(a, b)).isEqualTo(expected);
}

// GOOD: Expected value is a known, pre-calculated constant or fixture
@Test
void shouldCalculateDistanceBetweenKnownCoordinates() {
    assertThat(calculator.distance(MOSCOW_COORDS, SPB_COORDS)).isEqualTo(634.5);
}
```
