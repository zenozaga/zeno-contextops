# Template Quality Checklist

Use this checklist when creating or reviewing templates to ensure quality and completeness.

## ✅ Structure Validation

### Required Files
- [ ] `.ai-context/` folder exists
- [ ] `.ai-context/.aicontext.yaml` with complete configuration  
- [ ] `.ai-context/rules.md` with specific, actionable rules
- [ ] `.ai-context/knowledge/README.md` with domain knowledge
- [ ] `.ai-context/tasks/README.md` with task instructions
- [ ] `.ai-context/templates/README.md` with template descriptions  
- [ ] `.ai-context/goals/README.md` with objective definitions
- [ ] `.ai-context/apis/README.md` with API specifications
- [ ] Root `README.md` with comprehensive usage guide

### Folder Structure
- [ ] Follows kebab-case naming ending in `-context`
- [ ] All subfolders have README.md files
- [ ] File names use consistent naming conventions
- [ ] No empty folders without explanation

## 📝 Content Quality

### Documentation
- [ ] All content written in clear, professional English
- [ ] Purpose and use cases clearly defined
- [ ] Prerequisites explicitly stated  
- [ ] Step-by-step instructions provided
- [ ] Expected outcomes described
- [ ] Examples included where helpful

### Rules & Standards
- [ ] Rules are specific and actionable (not generic advice)
- [ ] Coding standards appropriate for target technology
- [ ] Quality gates and validation criteria defined
- [ ] Process guidelines included
- [ ] Best practices documented with reasoning

### Knowledge Base
- [ ] Domain-specific concepts explained
- [ ] Common patterns and anti-patterns documented
- [ ] External resources and references included
- [ ] Troubleshooting guides provided
- [ ] Background context for decision-making

## 🎯 Functionality Testing

### Task Execution
- [ ] All tasks can be executed successfully
- [ ] Task dependencies clearly defined
- [ ] Error handling and recovery documented
- [ ] Tasks produce expected outputs
- [ ] Workflow sequence logical and complete

### Template Files  
- [ ] Code templates are syntactically correct
- [ ] Placeholders clearly marked and documented
- [ ] Templates follow established conventions
- [ ] Generated code compiles/runs without errors
- [ ] Templates include necessary imports/dependencies

### Goals & Objectives
- [ ] Goals have measurable success criteria
- [ ] Objectives are realistic and achievable  
- [ ] Progress tracking mechanisms defined
- [ ] Milestone checkpoints included
- [ ] Completion validation provided

## 🔧 Technical Validation

### Configuration
- [ ] `.aicontext.yaml` schema valid
- [ ] Version information included
- [ ] Template metadata complete
- [ ] Dependencies properly specified
- [ ] Environment requirements documented

### Integration
- [ ] Template works in target environments
- [ ] No conflicts with common project structures
- [ ] Graceful handling of existing configurations
- [ ] Clear upgrade/migration paths
- [ ] Rollback procedures documented

## 📊 User Experience

### Accessibility  
- [ ] Instructions clear for beginners
- [ ] Advanced options available for experts
- [ ] Common questions anticipated and answered
- [ ] Multiple usage scenarios covered
- [ ] Troubleshooting section comprehensive

### Consistency
- [ ] Follows repository template patterns
- [ ] Matches established naming conventions
- [ ] Consistent formatting and style
- [ ] Standard section ordering maintained
- [ ] Cross-references accurate and helpful

## 🚀 Index Integration

### README.md Updates
- [ ] Template added to appropriate category
- [ ] Description follows standard format  
- [ ] Purpose clearly communicated
- [ ] Use cases relevant and specific
- [ ] Quick start instructions accurate
- [ ] Alphabetical order maintained within category

## ✨ Final Review

### Completeness
- [ ] All planned features implemented
- [ ] No TODO comments or placeholder content
- [ ] All references and links functional
- [ ] Version information updated
- [ ] Changelog entry created

### Quality Assurance  
- [ ] Peer review completed (if applicable)
- [ ] Real-world testing performed
- [ ] User feedback incorporated
- [ ] Performance implications considered
- [ ] Security implications reviewed