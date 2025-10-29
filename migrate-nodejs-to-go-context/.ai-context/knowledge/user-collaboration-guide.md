# User Collaboration Guide

This guide helps users effectively collaborate with agents during Node.js to Go migration.

## 🎯 Your Critical Role as User

### You Are Essential to Success
The migration agent **cannot guess** your requirements, preferences, or constraints. Your active participation is **mandatory** for successful migration.

### What Agents Cannot Determine Alone
- **Architecture preferences** (microservices vs monolith vs serverless)
- **Performance requirements** (latency targets, throughput needs)
- **Deployment constraints** (infrastructure limitations, compliance requirements)
- **Business priorities** (which components are most critical)
- **Existing integrations** (external services, databases, APIs)
- **Team preferences** (coding standards, tooling choices)

## 📋 Information You Must Provide

### Before Migration Starts
1. **Target Architecture Pattern**
   - Microservices, monolithic, serverless, or hybrid?
   - Preferred Go frameworks or libraries?
   - Deployment target (containers, bare metal, cloud functions)?

2. **Performance Requirements**
   - Current performance bottlenecks in Node.js version
   - Target performance improvements (latency, throughput, memory)
   - Load patterns and scaling requirements

3. **Constraints and Dependencies**
   - External services that must remain unchanged
   - Database schemas that cannot be modified
   - Authentication/authorization systems in use
   - Compliance or security requirements

4. **Business Context**
   - Which components are most business-critical?
   - What's the migration timeline and priorities?
   - Are there any features that can be simplified or removed?

### During Migration Process
1. **Architecture Decisions**
   - Review and approve proposed Go project structure
   - Confirm migration sequence and priorities
   - Validate architectural patterns before implementation

2. **Implementation Feedback**
   - Review migrated components for business logic accuracy
   - Confirm API contracts and data formats are correct
   - Validate performance improvements meet expectations

## ⚠️ Critical Information - Do Not Omit

### Database and Data Layer
- **Schema details**: Table structures, relationships, constraints
- **Data access patterns**: Query patterns, transaction requirements
- **Migration constraints**: Can schema change? Data migration needs?

### External Integrations  
- **Third-party APIs**: Authentication methods, rate limits, data formats
- **Message queues**: Protocols, message formats, delivery guarantees
- **File systems**: Storage patterns, access permissions, backup requirements

### Security and Authentication
- **Authentication flows**: OAuth, JWT, session management patterns
- **Authorization rules**: Role-based access, permission systems
- **Security requirements**: Encryption, audit trails, compliance needs

### Infrastructure and Deployment
- **Current deployment process**: CI/CD pipelines, environment management
- **Infrastructure constraints**: Container orchestration, networking, monitoring
- **Scaling requirements**: Auto-scaling rules, resource limits

## 🤝 How to Collaborate Effectively

### Be Proactive
- **Anticipate questions**: Think about what information the agent might need
- **Provide context**: Don't just answer "yes/no" - explain the reasoning
- **Share examples**: Show current code patterns, configurations, or documentation

### Stay Engaged
- **Review regularly**: Check migration progress and provide feedback
- **Ask questions**: If something doesn't look right, speak up immediately
- **Test early**: Validate migrated components as soon as they're available

### Document Decisions
- **Record rationale**: Why certain architectural choices were made
- **Note exceptions**: Any deviations from standard patterns and why
- **Track changes**: Keep log of modifications and their business impact

## 🚀 Understanding the Migration Process

### Phase 1: Analysis (You're Essential)
- Agent analyzes your codebase but **needs your input** on business logic
- **Your role**: Explain complex business rules, integration requirements
- **Don't assume**: Agent understands your domain - provide context

### Phase 2: Architecture Planning (Your Approval Required)
- Agent proposes migration strategy based on coupling analysis
- **Your role**: Review and approve architectural decisions
- **Validate**: Ensure proposed approach fits your operational requirements

### Phase 3: Component Migration (Stay Involved)
- Agent migrates components following minimal coupling approach
- **Your role**: Test each component, validate business logic correctness
- **Provide feedback**: Report issues immediately, don't wait for completion

### Phase 4: Integration and Testing (Your Validation Needed)
- Agent integrates migrated components and runs tests
- **Your role**: Perform user acceptance testing, validate end-to-end flows
- **Sign off**: Confirm each phase completion before moving to next

## ❌ Common Collaboration Pitfalls

### What NOT to Do
- **Don't assume agent knows your business domain**
- **Don't wait until the end to provide feedback**
- **Don't omit "obvious" information - it might not be obvious to agent**
- **Don't approve architectural decisions you don't understand**
- **Don't skip testing intermediate results**

### Red Flags - Stop and Clarify
- Agent makes assumptions about your requirements
- Migration proceeds without your input on business logic
- Architectural decisions are made without your approval
- Performance or functionality seems degraded
- You don't understand what's being implemented

## ✅ Success Indicators

### Good Collaboration Signs
- Agent asks detailed questions about your requirements
- You're actively reviewing and approving each phase
- Migration follows a clear, agreed-upon plan
- Performance improvements are measured and validated
- You understand what's being built and why

Remember: **You are not just a stakeholder - you are a critical participant in the migration process.**