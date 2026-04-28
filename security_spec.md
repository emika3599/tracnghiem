# Security Specification: QuizGen AI

## Data Invariants
1. A quiz history item must belong to the user who completed the quiz.
2. A saved quiz must belong to the user who saved it.
3. A folder must belong to the user who created it.
4. Timestamps (`timestamp`) must be validated against `request.time` where applicable.
5. All IDs must follow a strict pattern (alphanumeric, length limits).
6. Users can only read, create, update, or delete their own data.
7. Only verified emails can perform write operations (if specified).

## The "Dirty Dozen" Payloads

1. **Identity Spoofing**: Attempt to create a document for another user's ID.
2. **Resource Poisoning**: Use a massive string (1KB+) as a document ID.
3. **Shadow Update**: Add extra fields (e.g., `role: 'admin'`) to a document.
4. **Timestamp Fraud**: Provide a past or future timestamp from the client.
5. **Score Manipulation**: Set a quiz score > 10 (or outside valid range).
6. **Orphaned Writes**: Create a history item without required quiz data.
7. **Cross-User Leak**: Attempt to list another user's folders.
8. **Privilege Escalation**: Attempt to update a user role via user profile (if it existed).
9. **Terminal State Bypass**: Attempt to update an immutable history record.
10. **ID Injection**: Use special characters in document IDs to break path logic.
11. **Type Mismatch**: Send a number where a string is expected.
12. **Unverified Write**: Attempt to write from an unverified email account.

## The Test Runner (firestore.rules.test.ts)
(Will be implemented as a separate file)
