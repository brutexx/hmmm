# LLM Guidance for Tonto

This folder contains the Tonto guidance set for agentic IDEs that load project instructions from repository files.

## Workflow for Using Specialized Guidance Files

1. Analyze the user's request and identify the primary intent.
2. Use the matching guidance file in this folder:
   - Creating or extending ontology elements: `tonto-llm-create-new-elements.md`
   - Terminology analysis: `tonto_llm_terminology_analysis_guide.md`
   - Understanding or summarization: `tonto_llm_understanding_and_summarization_guide.md`
   - Documentation or translation: `tonto_llm_documentation_guide.md`
3. Use `tonto-guidance.md` for grammar, syntax, and project structure questions.
4. Use `tonto-cardinality-guidance.md` for relation cardinalities and multiplicity constraints.
5. When a request changes the ontology, inspect the existing model first, explain the intended modeling approach, and call out important assumptions before editing.
