---
name: code-style-guide
description: Essential development rules for writing maintainable code
---

# Code Style Guide

## 1. Documentation-First Implementation

Never assume library APIs, syntax, or best practices from memory. Always research official documentation before implementing.

**Requirements:**
- Search for latest official documentation before using any external library
- Verify current recommended implementation patterns - APIs change frequently
- Check for deprecation warnings and migration guides
- Use exact syntax from official docs, not from memory
- Research latest dependency versions before adding them

**Examples:**
- React Hooks → Search "React Hooks latest documentation" 
- Express.js → Verify current syntax from expressjs.com
- Python asyncio → Check python.org for latest patterns

## 2. Minimal and Simple Code

Write the least complex code that solves the problem. Avoid over-engineering.

**Requirements:**
- Prefer simple solutions over clever ones
- Avoid deep nesting (max 2-3 levels)
- One function should do one thing
- No unnecessary wrapper classes
- Use built-in functions before writing custom utilities

**Why:** Simple code is easier to debug, test, and maintain.

## 3. Think Before Coding

Do not write code immediately. First brainstorm and validate the approach.

**Requirements:**
- Explain approach in comments or pseudocode first
- Consider at least 2 alternative approaches
- Research idiomatic patterns if unsure
- Ask "Is there a simpler way?" before finalizing

**Process:**
1. Understand what needs to be done
2. Think about how to do it
3. Research if unsure about best practices
4. Explain chosen approach
5. Then write the code

## 4. Modular Code Organization

Keep related code together in modules. Only extract to common when truly shared.

**Requirements:**
- Each module should be self-contained
- Types and utils specific to module stay in module
- Only move to `common/` if used by 3+ modules
- No circular dependencies between modules

**Decision Rule:**
- Used by 3+ modules? → Move to common
- Generic (not domain-specific)? → Move to common
- Otherwise → Keep in the module

## 5. Language-Specific Guidelines

### JavaScript/TypeScript
- Use meaningful variable names
- Follow ESLint configuration
- Prefer `const` and `let` over `var`
- Use TypeScript for type safety

### Python
- Follow PEP 8 style guide
- Use type hints where appropriate
- Prefer list comprehensions over loops
- Use f-strings for string formatting

### Other Languages
- Follow official style guides
- Use language-specific idioms
- Maintain consistency with existing codebase

## Summary Checklist

Before writing any code, verify:

- [ ] Did I search official documentation?
- [ ] Is this the simplest solution?
- [ ] Did I think through the approach?
- [ ] Is this code in the right module?
- [ ] If moving to common/, is it used by 3+ modules?
- StateFlow/SharedFlow → Verify current best practices from Android Developers

**Why:** Library APIs evolve. What worked 6 months ago may be deprecated or have better alternatives now.

---

## 2. Minimal and Simple Code

Write the least complex code that solves the problem. Avoid over-engineering.

**Requirements:**
- Prefer simple solutions over clever ones
- Avoid deep nesting - refactor if more than 2-3 levels deep
- One function should do one thing
- Avoid premature abstraction - don't create interfaces until you have multiple implementations
- No unnecessary wrapper classes
- Use Kotlin's built-in functions before writing custom utilities

**Examples:**
```kotlin
// ❌ Over-engineered
class LocationUpdateHandler {
    interface LocationUpdateCallback {
        fun onUpdate(location: Location)
    }
    
    class LocationUpdateProcessor(private val callback: LocationUpdateCallback) {
        fun process(location: Location) {
            callback.onUpdate(location)
        }
    }
}

// ✅ Simple
fun processLocation(location: Location): Position {
    return Position(location.latitude, location.longitude)
}
```

**Why:** Simple code is easier to debug, test, and maintain. Complexity should be earned, not assumed.

---

## 3. Think Before Coding

Do not write code immediately. First brainstorm and validate the approach.

**Requirements:**
- Before implementing any feature, explain the approach in comments or pseudocode
- Consider at least 2 alternative approaches before picking one
- If unsure about language features or patterns, search for the idiomatic way to do it in Kotlin
- Ask "Is there a simpler way?" before finalizing
- For algorithms, sketch the logic flow before writing code

**Process:**
1. Understand what needs to be done
2. Think about how to do it
3. Research if unsure about best practices
4. Explain the chosen approach
5. Then write the code

**Examples:**
```kotlin
// Step 1: Think through the approach
// Need to find nearest point on polyline to user location
// 
// Approach A: Brute force - check every segment (O(n))
// Approach B: Spatial index - KD-tree for candidates, then check segments (O(log n))
// 
// Choosing B because we have thousands of points and this runs every 100ms
// 
// Implementation:
// 1. Query spatial index for points within radius
// 2. For each candidate, find nearest point on that segment
// 3. Return the closest one

fun findNearestPoint(location: Position): ProjectedPoint {
    // ... implementation follows the plan
}
```

**Why:** Planning prevents rewrites. A few minutes of thinking saves hours of refactoring.

---

## 4. Modular Code Organization

Keep related code together in modules. Only extract to common when truly shared.

**Requirements:**
- Each module (location, geo, api, etc.) should be self-contained
- Types, utils, and constants specific to a module stay in that module
- Only move to `common/` if used by 3+ modules
- No circular dependencies between modules

**Structure:**
```
sdk/
├── location/
│   ├── LocationProvider.kt
│   ├── LocationFilter.kt
│   ├── LocationTypes.kt        # Types specific to location
│   └── LocationUtils.kt        # Utils specific to location
│
├── geo/
│   ├── SpatialIndex.kt
│   ├── CheapRuler.kt
│   ├── GeoTypes.kt             # Position, ProjectedPoint, etc.
│   └── GeoUtils.kt             # Distance, bearing calculations
│
├── api/
│   ├── NavGuruApiClient.kt
│   ├── ApiTypes.kt             # Request/Response types
│   └── ApiUtils.kt             # API-specific helpers
│
└── common/                     # ONLY truly shared utilities
    ├── utils/
    │   └── CollectionUtils.kt  # Generic array/list helpers
    └── types/
        └── Result.kt           # Generic result wrapper
```

**Decision rule for common/:**
- Is this used by 3+ modules? → Move to common
- Is this generic (not domain-specific)? → Move to common
- Otherwise → Keep in the module

**Examples:**
```kotlin
// ❌ Wrong: Domain-specific type in common
// common/types/Position.kt
data class Position(val lat: Double, val lng: Double)

// ✅ Correct: Domain type in its module
// geo/GeoTypes.kt
data class Position(val lat: Double, val lng: Double)


// ❌ Wrong: Specific util in common
// common/utils/DistanceUtils.kt
fun haversineDistance(p1: Position, p2: Position): Double

// ✅ Correct: Geo util in geo module
// geo/GeoUtils.kt
fun haversineDistance(p1: Position, p2: Position): Double


// ✅ Correct: Truly generic util in common
// common/utils/CollectionUtils.kt
fun <T> List<T>.findIndexed(predicate: (Int, T) -> Boolean): Pair<Int, T>?
```

**Why:** Modules with clear boundaries are easier to understand, test, and modify independently.

---

## Summary Checklist

Before writing any code, verify:

- [ ] Did I search the official documentation for the library/API I'm using?
- [ ] Is this the simplest solution that works?
- [ ] Did I think through the approach before coding?
- [ ] Is this code in the right module?
- [ ] If moving to common/, is it used by 2+ modules?