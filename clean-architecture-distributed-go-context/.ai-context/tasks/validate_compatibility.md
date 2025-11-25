# Validate Configuration Compatibility

**Description:**
Validate the answers to the 6 technical questions. Ensure all dependencies and choices are compatible before generating code.

## Steps
1. Check Outbox Notifier vs Database
2. Check Outbox Notifier vs Cache
3. Check gRPC requirements
4. Warn about performance if using Kafka + Polling
5. Show configuration summary to user for final confirmation

## Outcome
- If all checks pass, proceed to project generation
- If any check fails, explain the issue and suggest alternatives
