# Graphiti MCP Enhancement for Code-Focused Memory

## Overview
Transform the Graphiti MCP server into a flexible code knowledge management system that tracks code evolution, architecture decisions, and development insights using a minimal, adaptable entity design.

## Flexible Entity Design (v2)

### Design Philosophy
- **Flexibility over Rigidity**: Use fewer, more adaptable entity types
- **Emergence over Prescription**: Let structure emerge from relationships
- **Compliance**: Follow ALL Graphiti best practices
- **Extensibility**: Use metadata dicts for unlimited extension

### Core Entities (3 Total)

#### 1. Resource Entity
```python
from pydantic import BaseModel, Field
from datetime import datetime
from typing import Optional

class Resource(BaseModel):
    """A Resource represents any artifact in the development ecosystem - code files, documentation, configurations, schemas, or data files.
    
    This flexible entity can represent files, modules, documents, PRDs, configurations, 
    or any other resource that exists as a discrete unit in the project.
    
    Instructions for identifying and extracting resources:
    1. Look for mentions of files, documents, or modules ("the authentication.js file", "in the README", "the config.yaml")
    2. Identify code modules and their purposes ("the auth module handles JWT tokens")
    3. Extract documentation references ("according to the API docs", "the design document states")
    4. Capture configuration files and their roles ("the webpack.config.js defines bundling")
    5. Note data files and schemas ("the users.json schema", "the database migration files")
    6. Track dependencies between resources ("this component imports utils", "requires the crypto module")
    7. Record version information when mentioned ("using v2.1 of the API", "updated to latest")
    8. Preserve file paths relative to project root when possible
    9. Add specific typed fields as needed for domain-specific attributes
    """
    resource_type: Optional[str] = Field(
        None, 
        description="Type of resource: code, doc, config, prd, data, api, schema, etc."
    )
    path: Optional[str] = Field(
        None, 
        description="File path relative to project root or URL"
    )
    purpose: Optional[str] = Field(
        None, 
        description="What this resource does or why it exists"
    )
    format: Optional[str] = Field(
        None, 
        description="File format or content type: python, typescript, json, markdown, etc."
    )
    last_modified: Optional[datetime] = Field(
        None, 
        description="When this resource was last modified"
    )
    dependencies: Optional[list[str]] = Field(
        default_factory=list, 
        description="Other resources this depends on"
    )
    exports: Optional[list[str]] = Field(
        default_factory=list, 
        description="What this resource exports or provides"
    )
    version: Optional[str] = Field(
        None, 
        description="Version or revision identifier"
    )
```

#### 2. Knowledge Entity
```python
class Knowledge(BaseModel):
    """A Knowledge entity represents insights, patterns, decisions, problems, solutions, or learnings discovered during development.
    
    Captures all forms of knowledge gained during development work, from technical patterns 
    to architectural decisions, from bug root causes to performance optimizations.
    
    Instructions for identifying and extracting knowledge:
    1. Look for discovered patterns or approaches ("using useMemo reduced re-renders by 60%", "singleton pattern works well here")
    2. Identify architectural or technical decisions ("we chose PostgreSQL over MongoDB because...", "decided to use microservices")
    3. Extract problems and their solutions ("the memory leak was caused by...", "fixed by implementing connection pooling")
    4. Capture learnings and insights ("learned that React.memo is not always beneficial", "discovered that caching improves response time")
    5. Note performance findings ("database queries are the bottleneck", "lazy loading reduced initial bundle by 40%")
    6. Record security discoveries ("found XSS vulnerability in user input", "API keys were exposed in client code")
    7. Track what didn't work and why ("tried WebSockets but latency was too high", "recursive approach caused stack overflow")
    8. Preserve confidence levels when uncertainty is expressed ("might be", "probably", "definitely")
    9. Link knowledge to affected resources when mentioned ("applies to all API endpoints", "specific to the Dashboard component")
    10. Add specific typed fields for metrics, benchmarks, or domain-specific details
    """
    knowledge_type: Optional[str] = Field(
        None, 
        description="Type: pattern, decision, learning, problem, solution, insight, etc."
    )
    title: Optional[str] = Field(
        None, 
        description="Brief title or identifier (not 'name' - that's protected)"
    )
    description: Optional[str] = Field(
        None, 
        description="Detailed description of the knowledge"
    )
    category: Optional[str] = Field(
        None, 
        description="Domain or category: architecture, performance, security, ux, etc."
    )
    status: Optional[str] = Field(
        None, 
        description="Current status: experimental, validated, deprecated, superseded, etc."
    )
    confidence: Optional[float] = Field(
        None, 
        description="Confidence level (0.0 to 1.0)"
    )
    impact: Optional[str] = Field(
        None, 
        description="Impact level: critical, high, medium, low"
    )
    affected_resources: Optional[list[str]] = Field(
        default_factory=list, 
        description="Resources (files, modules) this knowledge applies to"
    )
    related_knowledge: Optional[list[str]] = Field(
        default_factory=list, 
        description="Related knowledge items"
    )
    evidence: Optional[list[str]] = Field(
        default_factory=list, 
        description="Supporting evidence or examples"
    )
    discovered_at: Optional[datetime] = Field(
        None, 
        description="When this knowledge was discovered"
    )
    validated_at: Optional[datetime] = Field(
        None, 
        description="When this knowledge was validated"
    )
```

#### 3. WorkItem Entity
```python
class WorkItem(BaseModel):
    """A WorkItem represents any unit of development work - features, bugs, tasks, refactors, explorations, or sprints.
    
    Tracks all types of development activities from high-level features to specific bug fixes,
    from exploratory spikes to systematic refactoring efforts.
    
    Instructions for identifying and extracting work items:
    1. Look for feature implementations ("implementing OAuth authentication", "adding dark mode support")
    2. Identify bug fixes and issues ("fixing the login timeout bug", "resolved race condition in checkout")
    3. Extract refactoring efforts ("refactoring the payment module", "cleaning up technical debt in utils")
    4. Capture exploratory work ("exploring GraphQL alternatives", "spike on WebSocket implementation")
    5. Note task descriptions and their outcomes ("migrated database to PostgreSQL", "updated all dependencies")
    6. Track sprint or iteration work ("Sprint 23 focused on performance", "Q1 goals included API redesign")
    7. Record blockers and impediments ("blocked by missing API documentation", "waiting for design approval")
    8. Preserve status transitions ("started Monday", "completed after 3 days", "put on hold due to priorities")
    9. Link to affected resources ("modified 15 files", "touched authentication and user modules")
    10. Capture unexpected discoveries during work ("found unrelated bug while testing", "discovered performance issue")
    11. Note time estimates vs actuals when mentioned ("estimated 2 days, took 5 days")
    12. Add specific typed fields for PR numbers, ticket IDs, or team-specific attributes
    """
    work_type: Optional[str] = Field(
        None, 
        description="Type: feature, bug, task, refactor, exploration, spike, etc."
    )
    title: Optional[str] = Field(
        None, 
        description="Work item title or brief description"
    )
    description: Optional[str] = Field(
        None, 
        description="Detailed description of the work"
    )
    status: Optional[str] = Field(
        None, 
        description="Current status: planned, in_progress, completed, blocked, etc."
    )
    priority: Optional[str] = Field(
        None, 
        description="Priority level: critical, high, medium, low"
    )
    sprint_id: Optional[str] = Field(
        None, 
        description="Associated sprint or iteration"
    )
    parent_id: Optional[str] = Field(
        None, 
        description="Parent work item if this is a subtask"
    )
    started_at: Optional[datetime] = Field(
        None, 
        description="When work began"
    )
    completed_at: Optional[datetime] = Field(
        None, 
        description="When work was completed"
    )
    estimated_hours: Optional[float] = Field(
        None, 
        description="Estimated hours to complete"
    )
    actual_hours: Optional[float] = Field(
        None, 
        description="Actual hours spent"
    )
    outcome: Optional[str] = Field(
        None, 
        description="Result: success, partial, blocked, pivoted, cancelled"
    )
    blockers: Optional[list[str]] = Field(
        default_factory=list, 
        description="Things blocking progress"
    )
    affected_resources: Optional[list[str]] = Field(
        default_factory=list, 
        description="Resources modified or affected"
    )
    discoveries: Optional[list[str]] = Field(
        default_factory=list, 
        description="Unexpected findings during work"
    )
```


## Key Improvements from v1

### 1. **Graphiti Best Practice Compliance**
- ✅ ALL fields are `Optional[type]`
- ✅ No protected attribute names (avoided "name")
- ✅ Proper types (datetime, float, int)
- ✅ Clear descriptions for LLM guidance
- ✅ PascalCase for entity names
- ✅ snake_case for attributes

### 2. **Flexibility Enhancements**
- Reduced from 9 rigid entities to 3 flexible ones
- Added `metadata` dict to every entity for extensibility
- Type fields are strings allowing infinite categorization
- No prescriptive enums - everything is flexible

### 3. **Better Structure**
- Atomic attributes (no compound data)
- Optional validators can be added as needed
- Schema can evolve without breaking existing data
- Relationships create the real intelligence

## Usage Examples

### Storing Code File Information
```python
await add_memory(
    name="Authentication Module",
    episode_body="""
    Resource:
    - resource_type: code
    - path: src/auth/authentication.ts
    - purpose: Handles user authentication and JWT token management
    - dependencies: [src/utils/crypto.ts, src/models/user.ts]
    - exports: [authenticate, validateToken, refreshToken]
    """,
    entity_types={"Resource": Resource}
)
```

### Recording a Learning
```python
await add_memory(
    name="React Performance Pattern",
    episode_body="""
    Knowledge:
    - knowledge_type: pattern
    - title: Memoization for expensive computations
    - description: Using useMemo reduced re-renders by 60% in data tables
    - category: performance
    - status: validated
    - confidence: 0.9
    - affected_resources: [src/components/DataTable.tsx]
    """,
    entity_types={"Knowledge": Knowledge}
)
```

### Tracking Work
```python
await add_memory(
    name="OAuth Implementation",
    episode_body="""
    WorkItem:
    - work_type: feature
    - title: Implement OAuth 2.0 authentication
    - status: completed
    - sprint_id: sprint-2024-Q1-3
    - outcome: success
    - affected_resources: [src/auth/*, src/api/auth.ts]
    - discoveries: ["Need rate limiting on token refresh", "Azure AD requires special scope handling"]
    """,
    entity_types={"WorkItem": WorkItem}
)
```

## Migration from v1

For existing memories using the old 9-entity system:

1. **CodeUnit** → Resource (resource_type: "code")
2. **Feature** → WorkItem (work_type: "feature")
3. **Sprint** → WorkItem (work_type: "sprint")
4. **Problem** → Knowledge (knowledge_type: "problem")
5. **Pattern** → Knowledge (knowledge_type: "pattern")
6. **TechChoice** → Knowledge (knowledge_type: "decision")
7. **DocumentationRef** → Resource (resource_type: "doc")
8. **WorkContext** → WorkItem
9. **ProjectConfig** → ProjectConfig (minimal changes)

Use the `metadata` field to preserve any attributes that don't map directly.

## Benefits of This Approach

1. **Maximum Flexibility**: Can adapt to any development workflow
2. **Future-Proof**: metadata dict allows unlimited extension
3. **Graphiti Compliant**: Follows ALL best practices
4. **Emergence-Friendly**: Structure emerges from relationships
5. **Simple Mental Model**: Just 3 core concepts to remember
6. **Easy Migration**: Old data maps naturally to new structure
7. **Better Retrieval**: Fewer types means more consistent search

## Scope Management Strategy

### V1 Approach: Simple Project-Level Scoping

Use a special "config" scope to store project configurations:

```python
# Store project config in special scope
await add_memory(
    name="Project Configuration",
    episode_body="""
    ProjectConfig:
    - project_identifier: agentic-memory-mcp
    - scope_name: project_agentic_memory_mcp
    - primary_language: python
    """,
    group_id="config"  # Special config scope
)

# Discover and use project scope
config = await search_memory_nodes(
    "ProjectConfig agentic-memory-mcp",
    group_ids=["config"]
)
project_scope = extract_scope(config)

# Use discovered scope for memories
await add_memory(
    name="Bug fix",
    episode_body="Fixed OAuth timeout issue",
    group_id=project_scope
)
```

## Implementation Notes

- Start with generic mode (entity_types={}) to test flexibility
- Gradually introduce the 3 flexible entity types
- Use metadata dict liberally for unexpected attributes
- Let relationships define the structure, not entity types
- Focus on capturing "why" not just "what"
- Link to source files rather than duplicating content

## Future Enhancements

- Hierarchical scopes (global → project → feature)
- Automatic pattern promotion based on usage
- Cross-project knowledge sharing
- Confidence scoring and validation workflows
- Integration with CI/CD for automatic memory updates