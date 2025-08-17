# APIs

Purpose: Single source of truth for external services (OpenAPI/Swagger).

- Formats: `.yaml` or `.json` OpenAPI specs.
- Usage (Agent):
  1) Parse the spec to generate strong types.
  2) Scaffold adapters/clients from templates.
  3) Derive tests and example requests from the spec.
- Constraint: Do not invent endpoints. Trust the spec.
