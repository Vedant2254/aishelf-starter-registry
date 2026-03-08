---
name: feature-research-brainstorm
description: Research and brainstorm solutions for new feature implementation
---

# Feature Research & Brainstorming Workflow

This workflow helps you research, analyze, and brainstorm solutions when implementing new features. Use this when you need to understand how others have solved similar problems, compare different approaches, and make informed decisions about libraries and implementation strategies.

## When to Use This Workflow

- Implementing a new feature with unknown best practices
- Choosing between multiple libraries or frameworks
- Designing complex functionality requiring research
- Needing to understand industry standards and patterns
- Comparing different architectural approaches
- Evaluating trade-offs between solutions

## Workflow Steps

### 1. Define Feature Requirements

**Clarify the Problem:**
- What specific functionality needs to be implemented?
- What are the core requirements and constraints?
- What are the success criteria?
- Are there any technical limitations or dependencies?

**Document Context:**
```markdown
## Feature: [Feature Name]

### Requirements:
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

### Constraints:
- [Technical constraint 1]
- [Performance requirement 1]
- [Compatibility requirement 1]

### Success Criteria:
- [Measurable outcome 1]
- [Measurable outcome 2]
```

### 2. Research Existing Solutions

**Research Strategy:**
- Search for industry best practices
- Look for open-source implementations
- Study documentation and tutorials
- Review community discussions and case studies

**Search Patterns:**
```
"[feature] best practices"
"[feature] implementation guide"
"[feature] architecture patterns"
"[feature] tutorial examples"
"[language] [feature] libraries comparison"
```

**Research Sources:**
- Official documentation and guides
- GitHub repositories with similar implementations
- Stack Overflow discussions and solutions
- Blog posts and technical articles
- Conference talks and presentations
- Library/framework documentation

### 3. Identify Potential Approaches

**Categorize Solutions:**
- **Library-based**: Using existing libraries/frameworks
- **Custom Implementation**: Building from scratch
- **Hybrid Approach**: Combining libraries with custom code
- **Service Integration**: Using external APIs/services

**Document Each Approach:**
```markdown
## Approach: [Approach Name]

### Description:
[Brief description of the approach]

### Libraries/Tools:
- [Library 1] - [Purpose]
- [Library 2] - [Purpose]

### Pros:
- [Advantage 1]
- [Advantage 2]

### Cons:
- [Disadvantage 1]
- [Disadvantage 2]

### Complexity:
[Low/Medium/High]

### Learning Curve:
[Low/Medium/High]
```

### 4. Compare Libraries and Tools

**Evaluation Criteria:**
- **Functionality**: Does it meet all requirements?
- **Performance**: How efficient is it?
- **Maintenance**: Is it actively maintained?
- **Community**: How large is the community?
- **Documentation**: Quality of documentation
- **Learning Curve**: Ease of adoption
- **Compatibility**: Works with existing stack?

**Library Comparison Template:**
```markdown
## Library Comparison: [Feature Area]

| Library | Functionality | Performance | Maintenance | Community | Docs | Learning | Compatible |
|---------|--------------|-------------|-------------|-----------|------|----------|------------|
| [Lib 1] | ✅✅✅ | ✅✅ | ✅✅✅ | ✅✅✅ | ✅✅✅ | ✅✅ | ✅✅✅ |
| [Lib 2] | ✅✅ | ✅✅✅ | ✅✅ | ✅✅ | ✅✅ | ✅✅✅ | ✅✅ |
| [Lib 3] | ✅✅✅ | ✅✅ | ✅✅✅ | ✅✅ | ✅✅ | ✅ | ✅✅ |

**Legend:**
✅✅✅ Excellent | ✅✅ Good | ✅ Fair | ❌ Poor
```

### 5. Brainstorm Implementation Approaches

**Approach Brainstorming:**
For each identified approach, brainstorm specific implementation strategies:

**Strategy 1: [Strategy Name]**
- **Core Idea**: [Brief description]
- **Key Components**: [List of main components]
- **Data Flow**: [How data moves through the system]
- **State Management**: [How state is handled]
- **Error Handling**: [Error management strategy]
- **Testing Approach**: [How to test this approach]

**Strategy 2: [Strategy Name]**
- **Core Idea**: [Brief description]
- **Key Components**: [List of main components]
- **Data Flow**: [How data moves through the system]
- **State Management**: [How state is handled]
- **Error Handling**: [Error management strategy]
- **Testing Approach**: [How to test this approach]

### 6. Evaluate Trade-offs

**Trade-off Analysis:**
For each approach, evaluate the pros and cons:

```markdown
## Trade-off Analysis: [Approach Name]

### Advantages:
- **Performance**: [Specific performance benefits]
- **Maintainability**: [Code maintenance benefits]
- **Scalability**: [How well it scales]
- **Developer Experience**: [Ease of development]
- **User Experience**: [Benefits for end users]

### Disadvantages:
- **Complexity**: [Implementation complexity]
- **Learning Curve**: [Time to learn/adopt]
- **Dependencies**: [Additional dependencies required]
- **Limitations**: [Known limitations]
- **Risks**: [Potential risks]

### Mitigation Strategies:
- [How to address disadvantages]
- [Alternative approaches if issues arise]
```

### 7. Make Recommendation with Confidence

**Confidence Assessment:**
- **High Confidence (80-100%)**: Well-documented, widely used, proven solution
- **Medium Confidence (50-79%)**: Good solution but some uncertainty or trade-offs
- **Low Confidence (20-49%)**: Experimental or limited evidence
- **Very Low Confidence (0-19%)**: High risk, requires prototyping

**Recommendation Template:**
```markdown
## Recommended Solution

**Primary Recommendation**: [Approach/Library Name]

**Confidence Level**: [High/Medium/Low] ([XX]%)

**Reasoning**:
1. **Best Fit for Requirements**: [Why it meets requirements best]
2. **Proven Track Record**: [Evidence of successful usage]
3. **Community Support**: [Community and maintenance status]
4. **Implementation Feasibility**: [Ease of implementation]
5. **Future-Proofing**: [Long-term viability]

**Alternative Options**:
1. **Alternative 1**: [Name] - [Brief reason to consider]
2. **Alternative 2**: [Name] - [Brief reason to consider]

**Implementation Strategy**:
1. **Phase 1**: [Initial implementation steps]
2. **Phase 2**: [Additional features]
3. **Phase 3**: [Optimization and refinement]

**Risk Mitigation**:
- [Risk 1]: [Mitigation strategy]
- [Risk 2]: [Mitigation strategy]
```

### 8. Create Implementation Plan

**Detailed Implementation Steps:**
```markdown
## Implementation Plan

### Phase 1: Foundation ([Timeframe])
- [ ] [Specific task 1]
- [ ] [Specific task 2]
- [ ] [Specific task 3]

### Phase 2: Core Features ([Timeframe])
- [ ] [Specific task 1]
- [ ] [Specific task 2]
- [ ] [Specific task 3]

### Phase 3: Integration & Testing ([Timeframe])
- [ ] [Specific task 1]
- [ ] [Specific task 2]
- [ ] [Specific task 3]

### Dependencies:
- [Dependency 1]: [Details]
- [Dependency 2]: [Details]

### Success Metrics:
- [Metric 1]: [Target]
- [Metric 2]: [Target]
```

## Best Practices

### Research Quality
- **Multiple Sources**: Cross-reference information from different sources
- **Recent Information**: Prioritize recent documentation and discussions
- **Community Validation**: Look for community adoption and feedback
- **Official Sources**: Always check official documentation first

### Evaluation Criteria
- **Requirements Fit**: How well does it solve the specific problem?
- **Future Viability**: Will this solution still be relevant in 2-3 years?
- **Team Skills**: Does the team have the skills to implement and maintain?
- **Project Context**: How does it fit with existing architecture?

### Decision Making
- **Transparent Trade-offs**: Clearly document pros and cons
- **Confidence Levels**: Be honest about uncertainty
- **Risk Assessment**: Identify and mitigate potential risks
- **Alternative Plans**: Have backup options ready

## Example: Real-time Chat Feature

**Scenario**: Implement real-time chat functionality in a web application

### Step 1: Requirements
```markdown
## Feature: Real-time Chat

### Requirements:
- Real-time message delivery
- User presence indicators
- Message history
- Typing indicators
- File sharing support

### Constraints:
- Must scale to 1000+ concurrent users
- <100ms message latency
- Mobile-friendly
- Works with existing React app

### Success Criteria:
- Messages deliver in <100ms
- 99.9% uptime
- No message loss
- Smooth UX on mobile
```

### Step 2: Research Results
**Libraries Found:**
- Socket.IO (WebSocket-based)
- Pusher (managed service)
- Ably (managed service)
- Firebase Realtime Database
- Custom WebSocket implementation

### Step 3: Library Comparison
| Library | Performance | Scalability | Cost | Maintenance | Learning |
|---------|-------------|-------------|------|-------------|----------|
| Socket.IO | ✅✅✅ | ✅✅ | ✅✅✅ | ✅✅✅ | ✅✅ |
| Pusher | ✅✅✅ | ✅✅✅ | ✅✅ | ✅✅✅ | ✅✅✅ |
| Ably | ✅✅✅ | ✅✅✅ | ✅✅ | ✅✅✅ | ✅✅✅ |
| Firebase | ✅✅ | ✅✅✅ | ✅✅ | ✅✅✅ | ✅✅✅ |
| Custom | ✅✅ | ✅ | ✅✅✅ | ✅ | ❌ |

### Step 4: Recommendation
```markdown
## Recommended Solution

**Primary Recommendation**: Socket.IO

**Confidence Level**: High (85%)

**Reasoning**:
1. **Best Performance**: Direct WebSocket control, <50ms latency
2. **Cost Effective**: No ongoing service costs
3. **Full Control**: Custom implementation possible
4. **Large Community**: Well-documented, many examples
5. **React Integration**: Excellent React library support

**Alternative**: Pusher
- Use if development time is critical
- Managed service reduces operational burden
- Higher cost but faster implementation

**Implementation Strategy**:
1. Phase 1: Basic Socket.IO setup with message sending
2. Phase 2: Add presence indicators and typing status
3. Phase 3: Implement file sharing and message history
```

## Usage Guidelines

### When to Use This Workflow
- **Complex Features**: Features requiring significant research
- **Technology Decisions**: Choosing between libraries/frameworks
- **Architecture Design**: Designing system architecture
- **Proof of Concepts**: Evaluating feasibility of approaches

### Expected Outcomes
- **Informed Decisions**: Evidence-based technology choices
- **Reduced Risk**: Identified and mitigated potential issues
- **Clear Roadmap**: Detailed implementation plan
- **Team Alignment**: Shared understanding of approach

### Success Indicators
- Clear requirements documented
- Multiple viable approaches identified
- Trade-offs analyzed and documented
- Recommendation with confidence level
- Actionable implementation plan
