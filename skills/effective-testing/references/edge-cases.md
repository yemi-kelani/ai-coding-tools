# Edge Case Patterns

Systematic edge case coverage by data type and context. Scan this list when writing tests.

## Numeric Values

| Category | Test Values | What Breaks |
|----------|-------------|-------------|
| Zero | `0`, `-0`, `0.0` | Division, array indexing, loop conditions |
| Boundaries | `MIN_INT`, `MAX_INT` | Overflow, type coercion |
| Off-by-one | `min-1`, `max+1` | Fence-post errors, range checks |
| Signs | positive, negative, mixed | Absolute value assumptions |
| Precision | `0.1 + 0.2`, `1e-10`, `1e308` | Float comparison, rounding |
| Special | `NaN`, `Infinity`, `-Infinity` | Propagation, comparison |

## Strings

| Category | Test Values | What Breaks |
|----------|-------------|-------------|
| Empty | `""`, `null`, `undefined` | Length checks, first char access |
| Whitespace | `" "`, `"\t"`, `"\n"`, leading/trailing | Trimming assumptions, validation |
| Length | 1 char, max length, max+1 | Buffer overflow, truncation |
| Encoding | unicode (`日本語`), emoji (`🎉`), RTL | Display, sorting, length calculation |
| Special | quotes, backslashes, `\0` | Escaping, parsing, SQL injection |
| Case | UPPER, lower, MiXeD | Case-sensitive comparison |

## Collections

| Category | Test Values | What Breaks |
|----------|-------------|-------------|
| Size | empty, 1 element, 2 elements, 1000+ | Off-by-one, first/last handling |
| Duplicates | all same, some dupes, no dupes | Deduplication, counting |
| Order | sorted, reverse, random | Sort stability, binary search |
| Content | contains null, nested empty | Null propagation, iteration |
| Index | 0, -1, length-1, length | Bounds checking |

## Objects/Maps

| Category | Test Values | What Breaks |
|----------|-------------|-------------|
| Empty | `{}`, no keys | Property access, iteration |
| Keys | missing required, extra keys, null key | Validation, destructuring |
| Values | null values, undefined, wrong type | Type assumptions |
| Depth | flat, deeply nested, circular | Recursion, serialization |
| Prototype | inherited properties | hasOwnProperty checks |

## Date/Time

| Category | Test Values | What Breaks |
|----------|-------------|-------------|
| Boundaries | epoch, far future (9999), far past | Overflow, display |
| Calendar | Feb 29, Dec 31, Jan 1 | Leap year, year boundary |
| Timezone | UTC, local, +14:00, -12:00, DST | Conversion, storage |
| Invalid | Feb 30, month 13, hour 25 | Validation |
| Format | ISO, Unix timestamp, locale | Parsing |

## Files/IO

| Category | Test Values | What Breaks |
|----------|-------------|-------------|
| Size | 0 bytes, 1 byte, >4GB | Memory, streaming |
| Names | spaces, unicode, `.`, `..` | Path resolution |
| Paths | relative, absolute, symlinks | Security, resolution |
| Permissions | read-only, no access, locked | Error handling |
| Content | binary, UTF-8, UTF-16 BOM | Encoding detection |

## State/Lifecycle

| Category | Test Scenarios | What Breaks |
|----------|----------------|-------------|
| Init | uninitialized, double init | State corruption |
| Order | out-of-order calls, missing steps | Precondition violations |
| Concurrency | simultaneous calls, race conditions | Data races |
| Cleanup | use after dispose, double dispose | Resource leaks |
| Idempotency | same operation twice | Side effect duplication |

## Network/API

| Category | Test Scenarios | What Breaks |
|----------|----------------|-------------|
| Availability | timeout, refused, DNS failure | Error handling |
| Response | empty body, malformed JSON, partial | Parsing |
| Status | 200, 400, 401, 403, 404, 500, 503 | Error mapping |
| Latency | immediate, slow (>30s), variable | Timeout handling |
| Retry | success on retry, persistent failure | Retry logic |

## User Input (Security)

| Category | Test Values | What Breaks |
|----------|-------------|-------------|
| Injection | `'; DROP TABLE`, `<script>`, `$(cmd)` | SQL, XSS, command injection |
| Format | wrong format, partial, extra fields | Validation bypass |
| Size | too short, too long, exactly limit | Buffer issues |
| Encoding | URL encoded, base64, double-encoded | Validation bypass |

## Business Logic Checklist

When testing domain logic, always consider:
- **Minimum/maximum** allowed values for each field
- **Required vs optional** field combinations
- **State transitions** - what sequences are valid?
- **Permissions** - admin vs user vs anonymous
- **Temporal** - expired, not-yet-valid, exactly-at-boundary
- **Monetary** - rounding, currency conversion, negative amounts
