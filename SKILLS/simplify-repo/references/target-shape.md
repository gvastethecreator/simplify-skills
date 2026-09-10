# Target Shape

Read during step 5, before writing replacements. Design for the product that exists.

## Shallow Default

1. Public domain interface with meaningful verbs.
2. One implementation per real backend or platform.
3. One persistence module when durable state exists.
4. Thin entry points: CLI, route, UI, job.
5. Protocol logic owned by the backend that speaks it.
6. Tests through the public interface, plus focused protocol tests where wire behavior matters.

If the diagram needs more layers than that, the product should justify each one by name.

## Public Interface

- Expose domain verbs, not operation envelopes. Treat operations as data only when they genuinely are data.
- Accept validated, relevant inputs. Return useful values directly.
- Hide transport and lifecycle detail callers do not control.
- Make errors actionable: enough for the caller or user to decide something, no taxonomy that renames low-level failures.

## Selection And Extension

- Fixed set of backends: a switch.
- Dynamic registry only when third parties register implementations at runtime.
- No capability map for static features.

## Concurrency

- Smallest honest rule. If only one operation may run, reject overlap with a clear busy error.
- Queue only when ordering and backpressure are product requirements.
- Use platform cancellation and timeout APIs, and preserve cancellation reasons where the platform allows.
- Keep transport recovery private to the backend that owns it.

## State And Persistence

- One owner parses, validates, reads, updates, and writes persistent state.
- Preserve atomic writes where interruption corrupts data, and restrictive permissions for credentials or sensitive user data.
- Derive rather than mirror. Two objects holding the same truth is a defect waiting for a session.
- Lock only the resource that genuinely needs it.

## Validation

- One clear trust boundary per input path. Validate there, then stop re-checking downstream.
- Untrusted protocol messages are parsed and validated, always.

## Documentation Of The Shape

The primary architecture doc should state: public interface, module ownership, call paths, supported lifecycle, persistence owner, validation boundaries, error model, backend ownership, intentional single-process or sequential assumptions, deferred features, and the condition that would justify adding deferred machinery.

Use JSDoc or the project's equivalent for public APIs, non-obvious domain operations, invariants, trust boundaries, protocol mappings, persistence guarantees, and subtle lifecycle behavior. Not for restating the line below it.
