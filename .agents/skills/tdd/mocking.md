# When to Mock (Java / Spring Guidelines)

Mock at **system boundaries** only:

- External third-party APIs (payment gateways, external SMS/mail, external OAuth providers)
- Object Storage / S3 clients (or prefer MinIO / LocalStack via Testcontainers)
- Keycloak integration (when testing business logic without running full IdP)
- System clock / time / randomness (`Clock` bean)

Don't mock:

- Your own domain classes, entities, records, and value objects
- Internal Spring `@Service` collaborators within the same bounded context
- Database queries where a real schema/SQL is involved (prefer **Testcontainers** with real PostgreSQL)
- Anything you fully control and can run quickly in memory or containers

## Designing for Mockability in Spring

At system boundaries, design clean interfaces and inject them:

**1. Use constructor-based dependency injection**

Inject interfaces rather than instantiating clients directly:

```java
// GOOD: Easy to test and mock
@Service
public class SpotMediaService {
    private final S3StorageClient storageClient;

    public SpotMediaService(S3StorageClient storageClient) {
        this.storageClient = storageClient;
    }

    public String uploadSpotPhoto(UUID spotId, byte[] content, String contentType) {
        return storageClient.putObject("spots/" + spotId, content, contentType);
    }
}

// BAD: Hard to test, hard-coded client creation
@Service
public class SpotMediaService {
    public String uploadSpotPhoto(UUID spotId, byte[] content) {
        S3Client client = S3Client.builder().credentialsProvider(...).build();
        // tightly coupled to AWS SDK directly
    }
}
```

**2. Prefer domain-focused client abstractions over raw SDKs**

Wrap low-level AWS S3 SDK or HTTP clients into domain-specific interfaces:

```java
// GOOD: Domain-level interface, trivial to mock in unit tests
public interface SpotMediaStorage {
    StoredFile upload(String path, InputStream stream, long size, String mimeType);
    void delete(String path);
}
```

The domain abstraction approach means:
- Tests test business logic, not AWS SDK ceremony.
- Unit tests run in milliseconds.
- Swapping MinIO (local dev) for AWS S3 / Cloudflare R2 (production) requires zero changes to business logic.
