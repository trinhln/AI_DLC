Analyze the entire current project and generate the three core Kiro steering files:

- `.kiro/steering/product.md`
- `.kiro/steering/structure.md`
- `.kiro/steering/tech.md`

Base everything strictly on the actual current project. Do not reuse assumptions, conventions, architecture, or rules from any other project.

For each file:

- `product.md`: describe the product purpose, main domain, key capabilities, and high-level development context that AI needs to understand.
- `structure.md`: document the actual project structure, folder responsibilities, dependency boundaries, file organization, naming patterns, and where new code should be placed.
- `tech.md`: document the actual technology stack, framework versions, libraries, build/test tooling, coding patterns, and technical constraints.

Requirements:

- Inspect the source code and configuration files before generating the steering files.
- Infer only what is clearly supported by the existing project.
- Do not invent conventions that do not yet exist.
- Keep the steering concise and high-signal to minimize AI context/token usage.
- Do not duplicate generic Angular/framework best practices that belong in skills.
- Focus only on stable, project-specific context that should guide future AI work.
- Clearly mark anything that cannot yet be determined instead of guessing.

After generating the files, briefly explain what was captured in each steering file.
