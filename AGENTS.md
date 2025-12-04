# Agent Guidelines for opencode-kubernetes

## Overview
This repository is dedicated to Kubernetes troubleshooting using the Kubernetes MCP server for OpenCode in **read-only mode**. The purpose is to detect, analyze, and provide remediation suggestions for Kubernetes cluster issues without modifying any cluster resources.

## Core Principles

### 1. Ask Intelligent Questions First
- If instructions are **clear and specific**, proceed directly with investigation
- **Only ask questions if the initial request is unclear or lacks necessary context**
- If investigation hits a roadblock or you cannot find the issue, **come back and ask targeted questions**:
  - What error message or symptom are you seeing?
  - Which namespace is affected?
  - What resource type? (Pod, Deployment, Service, etc.)
  - Any Custom Resource Definitions (CRDs) involved?
- **DO NOT** blindly search the entire cluster - use questions to narrow scope efficiently
- Keep questions brief and to the point

### 2. Read-Only Operations
- **NEVER** update, modify, delete, or create any cluster resources
- Use available MCP tools for information gathering only, unless user specify what command you can use
- No `kubectl apply`, `kubectl edit`, `kubectl delete`, or any mutating operations

### 3. Troubleshooting Model: Detect → Analyze → Remediate

#### Detect
- Identify the problem by gathering relevant cluster information
- Use appropriate tools to list resources, check events, view logs, and monitor metrics
- Look for error messages, unusual states, resource pressure, or anomalies

#### Analyze
- Examine collected data to understand the root cause
- Correlate events, logs, and resource states
- Consider multiple potential causes
- Identify patterns and relationships between different cluster components

#### Remediate
- **Focus on immediate fixes first** - provide clear, actionable suggestions to resolve the issue quickly
- Output remediation steps to a file for reference
- Include command examples where applicable
- Long-term improvements can be provided **after** the immediate problem is fixed
- Prioritize speed of detection and resolution over comprehensive analysis

### 4. Confidence Levels
Every remediation suggestion **must** include a confidence level:

- **High (80-100%)**: Root cause clearly identified with strong evidence from logs/events/metrics
- **Medium (50-79%)**: Likely cause identified with supporting evidence, but some uncertainty remains
- **Low (30-49%)**: Possible cause identified based on symptoms, requires further validation
- **Speculative (<30%)**: Multiple potential causes, needs more investigation

**Format**: Always state confidence explicitly, e.g., "Confidence: High (90%)"

### 5. No Hallucination - Evidence-Based Solutions
- All solutions and troubleshooting steps **must** be based on actual Kubernetes concepts and tools
- Reference specific Kubernetes resources, fields, and documented behaviors
- Do not suggest non-existent commands, options, or features
- If uncertain, state the limitation clearly rather than guessing
- Solutions should be verifiable and grounded in Kubernetes documentation

**Note**: Low confidence is acceptable when evidence is limited, but the suggested approach must still be valid and real.

### 6. Step-by-Step Documentation
For every troubleshooting session, document:

1. **Initial Problem Statement**: What issue was reported or detected?
2. **Investigation Steps**: List each action taken in order
   - Which resources were examined
   - What tools/commands were used
   - What was found at each step
3. **Evidence Collected**: Summarize key findings
   - Error messages
   - Resource states
   - Metrics or patterns observed
4. **Root Cause**: What caused the issue
5. **Remediation Suggestions**: Recommended actions with confidence level
6. **Follow-up**: Additional monitoring or validation steps

This documentation serves as:
- A reference for similar issues in the future
- Training data for improving troubleshooting processes
- Audit trail of investigation methodology

## Output Format

All remediation suggestions should be saved to a file in the following format:

```markdown
# Troubleshooting Report: [Issue Description]
**Date**: [Timestamp]
**Cluster**: [Context/Cluster Name]

## Problem Statement
[Description of the issue]

## Remediation Suggestions
**Confidence Level**: [High/Medium/Low] ([percentage]%)

### Immediate Actions
1. [Action 1 with command example if applicable]
2. [Action 2 with command example if applicable]

## Investigation Steps
1. [Step 1 - Tool used and what was checked]
2. [Step 2 - Tool used and what was checked]
...

## Evidence
- [Key finding 1]
- [Key finding 2]
...

## Additional Analysis
### Root Cause Analysis
[Explanation of what caused the issue]

### Long-term Improvements
1. [Suggestion 1]
2. [Suggestion 2]

### Follow-up Monitoring
- [What to monitor after remediation]
- [How to verify the fix worked]
```


## Best Practices

1. **Start broad, then narrow**: Begin with cluster-wide views (events, namespaces) before diving into specific resources
2. **Check recent events**: Often the quickest way to identify issues
3. **Correlate timestamps**: Match events with log entries and metric spikes
4. **Consider the full stack**: Application → Pod → Node → Cluster
5. **Look for patterns**: Repeated errors, crash loops, resource trends
6. **Document as you go**: Don't wait until the end to document findings
7. **Be honest about limitations**: State when more information is needed

