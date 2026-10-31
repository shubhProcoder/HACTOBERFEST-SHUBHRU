Hacktoberfest 2026 – Hack Day Sonipat
Team Details
- Team name: [ENTER TEAM NAME]
- Team members:
  - [MEMBER 1 – Shubh Mehrotra]
  
Selected Problem Statement
Problem 02 – Evidence-Based Open Source Issue and Pull Request Triage

We will build a tool that helps open-source maintainers assess the technical claims made in GitHub issues and pull requests. The system will extract individual claims, inspect the relevant repository source code and documentation, and produce an evidence-backed assessment. It will identify supporting evidence, contradictions, missing evidence, and uncertainty rather than making unsupported conclusions.

The output will give maintainers a concise explanation with references to the relevant files and code or documentation excerpts. The initial implementation will focus on static repository inspection and a limited set of public repositories. It will not execute arbitrary contributor code.
AI Models Used
Model	Runtime use	Reason for selection
gemma4:12b (Ollama, local)	Extract structured technical claims from issue/PR text and generate evidence-grounded assessments	This model is already available locally through Ollama. It allows the application to use an open-weight model without depending on a closed-model API. We will instruct it to base each assessment on the retrieved repository evidence and return Uncertain when the evidence is insufficient.


Model execution: The application will call the local Ollama model. No OpenAI, Claude, Gemini, or other closed-model API will be used at runtime. The cloud-tagged models in the local Ollama inventory (gpt-oss:120b-cloud and glm-5.2:cloud) are not part of this project's runtime pipeline.

Retrieval approach: The initial version will use lexical retrieval (BM25) over repository files and documentation. BM25 is a retrieval algorithm, not an additional AI model. We will add another model only if it is installed, integrated, and documented accurately.ß
High-Level Technical Approach
1. Accept a GitHub issue or pull request URL, or allow the user to provide its text and repository URL.
2. Collect the relevant issue/PR text and public repository context.
3. Use gemma4:12b to extract discrete, verifiable technical claims in a structured format.
4. Search indexed source files and documentation for evidence relevant to each claim using lexical retrieval.ßß
5. Compare each claim with the retrieved evidence and classify it as Supported, Contradicted, or Uncertain.
6. Generate a maintainer-facing report containing the claim, assessment, explanation, evidence excerpts, and file references.
7. Keep the evidence traceable to the public repository so that maintainers can independently review the result.
Initial Implementation Scope
The first version will prioritize a working end-to-end flow for a limited number of public repositories and supported file types. It will focus on claim extraction, repository evidence retrieval, transparent assessment, and traceable output. Automated execution or reproduction of untrusted contributor code is outside the initial scope; the system will clearly state when a claim has not been reproduced.