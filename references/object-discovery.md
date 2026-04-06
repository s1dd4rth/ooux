# Object Discovery — Detailed Guide

## Noun Foraging Techniques

### Source Materials to Mine
1. **User research transcripts** — Highlight every noun participants use
2. **Product requirements docs (PRDs)** — Extract entity nouns
3. **Existing UI audit** — Screenshot every page, list what "things" appear
4. **Database schemas** — Table names are often objects
5. **API documentation** — Endpoint resources map to objects
6. **Support tickets** — What "things" do users complain about?
7. **Competitor analysis** — What objects do competitors expose?
8. **Domain expert interviews** — What do subject matter experts call things?
9. **Marketing copy** — What nouns does the business use to describe itself?
10. **Legal/compliance docs** — Regulated entities are always objects

### The Object Litmus Test

Ask these questions about each noun. The more "yes" answers, the more likely it's an object:

| Question | If Yes → Object | If No → Probably Attribute |
|---|---|---|
| Does it have its own detail page? | ✓ | |
| Can a user create one? | ✓ | |
| Can a user search for it? | ✓ | |
| Can a user bookmark/save it? | ✓ | |
| Does it have a lifecycle (created, modified, deleted)? | ✓ | |
| Does it have its own permissions? | ✓ | |
| Does it have multiple attributes? | ✓ | |
| Can it exist independently of another object? | ✓ | |
| Would a developer create a database table for it? | ✓ | |
| Does it have nested objects of its own? | ✓ | |

### Identifying Broken Objects

A "broken object" is an entity that behaves inconsistently across the product:
- **Shapeshifters**: Called "project" in one place, "workspace" in another
- **Frankenobjects**: Two different things merged into one confusing entity
- **Ghost objects**: Referenced in the UI but with no actual detail view or consistent structure
- **Zombie objects**: Dead features still cluttering the interface

### Object vs. Attribute vs. Metadata

| Type | Characteristics | Example |
|---|---|---|
| **Object** | Has detail page, own lifecycle, own CTAs, browsable | "Project", "User", "Invoice" |
| **Attribute** | Describes an object, no independent existence | "Project Name", "Due Date", "Status" |
| **Metadata** | System-level info, rarely user-facing | "Created timestamp", "Database ID" |

### Handling Synonyms and Language Conflicts

When different stakeholders use different words for the same thing:
1. Document ALL synonyms
2. Research which term users actually use (from transcripts)
3. Pick ONE canonical name — this becomes the shared language
4. Create an alias table: "The thing Marketing calls 'Campaign' and Engineering calls 'Blast' is canonically called 'Campaign'"
5. Propagate the canonical name everywhere: code, UI, docs, conversations

## AI-Assisted Noun Foraging

When using an AI agent for noun foraging:

```
Prompt pattern:
"Here is [document type]. Extract every noun that could represent 
a distinct entity/object in a digital system. For each noun, note:
1. The exact quote/context where it appeared
2. Whether it seems like an object (has own lifecycle) or attribute (describes another thing)
3. Any synonyms or alternative names used for the same concept"
```

After extraction, consolidate into a deduplicated list and run each through the Object Litmus Test.
