# Idempotency & Duplicate Prevention

## 1. Overview

**Idempotency** is a concept used in software systems to prevent the same operation from producing duplicate effects when a request is retried.

This is important when a system sends a request but does not receive a clear response. The operation may have succeeded even though the system does not know its result.

The goal is to make retries safer and reduce the risk of duplicate records or repeated actions.

## 2. What Is Idempotency?

Idempotency means that performing the same operation multiple times produces the same final effect as performing it once.

For example, if a customer registration request is sent multiple times, the system should avoid creating multiple records for the same registration.

- First request: The customer is saved.
- Second request: The system recognizes the repeated operation.
- Third request: No additional duplicate record is created.

**The key idea:** Retrying a request should not repeat its effect unnecessarily.

## 3. Why Can an Operation Be Repeated?

A request may be repeated when the system does not receive a clear response from the service it contacted.

For example:

1. The system sends a request to save customer data.
2. The destination saves the data successfully.
3. The response is lost because of a network problem.
4. The system assumes the operation may have failed.
5. The system retries the request.
6. Without duplicate prevention, another record may be created.

The main problem is that **a missing response does not necessarily mean that the operation failed**.

The system must distinguish between a failed operation and a successful operation whose response was not received.

## 4. What Is the Difference Between Retrying a Request and Creating a New Record?

Retrying a request means attempting to complete the same operation again.

Creating a new record means adding another entry to the database or spreadsheet.

Without an appropriate mechanism, a retry may be treated as a new operation, resulting in duplicate records.

With an idempotency mechanism, the system can recognize a repeated request and avoid performing the same operation twice.

A genuinely new registration, however, should still be allowed.

## 5. How Can a Unique Identifier Help?

An **Idempotency Key** is a unique identifier associated with a particular operation.

The system generates the key before sending the request and reuses the same key when retrying that operation.

For example:

- First request: `REG-8F21`
- Retry after a timeout: `REG-8F21`
- Another retry: `REG-8F21`

The system can use this identifier to determine whether the operation has already been processed.

If the operation has already succeeded, the system should avoid creating another record and can return or retrieve the previous result.

A new registration should have a different operation identifier.

**Important:** A unique identifier alone is not enough. The system must store or track identifiers and check them reliably before creating records. Concurrent requests must also be handled to prevent duplicate writes.

## 6. Research Question

**If the system does not know whether a data-saving operation succeeded and retries the request, how can it reduce the risk of creating a duplicate record?**

The system can reduce this risk by:

1. Generating a unique identifier for the operation.
2. Reusing the same identifier for every retry of that operation.
3. Checking whether the operation has already been processed.
4. Avoiding a second record when the same operation has already succeeded.
5. Tracking operation status so that uncertain outcomes can be checked instead of automatically treated as failures.
6. Handling simultaneous requests carefully to prevent duplicate records.

This approach makes retries safer without requiring the system to assume that every missing response means failure.

## 7. Connection to the Project: Customer Registration in Google Sheets

Consider the following scenario:

1. A customer confirms their information.
2. The agent sends a request to save the customer in Google Sheets.
3. Google Sheets saves the data, but the agent does not receive a clear response.
4. The agent retries the request.
5. The same customer may be registered twice if duplicate prevention is not implemented.

### Possible Solution

A possible design would work as follows:

1. Generate a unique identifier for the customer registration operation.
2. Send the identifier along with the customer data.
3. If the request must be retried, reuse the same identifier.
4. Check whether that registration operation has already been saved.
5. If it has already succeeded, do not add another row.
6. If it has not been saved, proceed with the registration while ensuring that concurrent requests cannot create duplicate records.

This is a proposed design for understanding the problem. It is not an implementation of duplicate prevention.

### Important Consideration for Google Sheets

Google Sheets does not automatically guarantee idempotency just because a request includes a unique identifier.

For example, checking whether an identifier exists and then appending a row can still cause duplicates if two requests perform the check at the same time.

A reliable solution must account for operation tracking, retries, and concurrent requests.

## 8. Conclusion

Idempotency is an important concept for building reliable systems that may need to retry requests.

The main lesson is that a repeated request should not automatically create a new record. By using a stable operation identifier and a reliable duplicate-checking mechanism, a system can reduce the risk of duplicate customer registrations when the result of a previous request is uncertain.

In this project, the concept helps explain how an agent could safely retry a request to save customer information in Google Sheets without registering the same operation twice.

## 9. Reference

- [Stripe — Idempotent Requests](https://docs.stripe.com/api/idempotent_requests)

This official Stripe documentation explains how idempotency keys can help make request retries safe and prevent repeated operations.

## Scope of This Research

This document focuses on understanding the problem, the underlying concept, and a possible solution design. **No implementation of the duplicate-prevention mechanism is included.**
