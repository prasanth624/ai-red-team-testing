# AI Red-Team Testing

## What is AI Red-Team Testing?

AI red-team testing is the process of deliberately testing an AI system with adversarial inputs to identify security vulnerabilities, weaknesses, and unsafe behaviors before they can be exploited.

Unlike traditional software testing, which often checks whether an application works as expected, AI red-team testing examines how an AI model or application behaves under unexpected, manipulative, or malicious inputs.

For example, a customer-support chatbot might be tested to determine whether it can be manipulated into:

- Revealing another customer's personal information.
- Accessing orders or billing records without proper authorization.
- Ignoring its intended security rules.
- Exposing hidden system instructions.
- Producing responses that violate the application's security requirements.

The objective is to discover weaknesses, understand their impact, and improve the system's security and reliability.

## Why is AI Red-Team Testing Important?

AI applications introduce security risks that may not be detected by ordinary functional testing. A chatbot can produce a normal response during routine usage but behave differently when confronted with carefully crafted prompts.

AI red-team testing helps organizations:

**1. Identify security vulnerabilities**

Discover potential weaknesses such as sensitive information disclosure, prompt injection, unauthorized data access, and attempts to bypass security controls.

**2. Validate security controls**

Check whether the application protects sensitive information, enforces access permissions, and follows its intended security boundaries.

**3. Test adversarial scenarios at scale**

Generate and execute many variations of attack prompts instead of relying entirely on manual testing.

**4. Improve AI application reliability**

Use test results to identify weaknesses, implement mitigations, and repeat tests to determine whether the fixes work.

**5. Establish repeatable security assessments**

Maintain test configurations, generated prompts, evaluation results, and findings so that security assessments can be repeated after application changes.

## What Does This Project Do?

This repository demonstrates an automated AI red-team testing workflow using **Promptfoo and Groq-hosted Qwen models**.

Promptfoo provides the red-team testing framework, while Groq provides access to the configured Qwen model used for generating adversarial test prompts.

The workflow focuses on testing a customer-support chatbot for security weaknesses.

The process consists of three main components:

1. **Test generation:** Qwen, accessed through Groq, generates adversarial prompts based on the configured application context, security plugins, and attack strategies.
2. **Target application:** Promptfoo sends the generated prompts to the customer-support chatbot and collects its responses.
3. **Evaluation:** Promptfoo evaluates the responses against the applicable security criteria and records the results for investigation.

## What Security Areas Can Be Tested?

Depending on the configured plugins and strategies, the project can explore areas such as:

- **Personally identifiable information (PII):** Attempts to disclose sensitive customer information.
- **Role-based access control (RBAC):** Attempts to bypass permissions or access restrictions.
- **Prompt extraction:** Attempts to reveal hidden system instructions.
- **Adversarial input variations:** Tests that vary the representation or wording of attack prompts, including Base64, hexadecimal, ROT13, homoglyphs, and leetspeak where supported.

The selected plugins and strategies determine the scope of each assessment. They do not guarantee that every possible vulnerability will be detected.

## Expected Outcome

The goal is to create a repeatable workflow for discovering and investigating potential AI security weaknesses.

Test results help identify cases that need further review. A reported failure is not automatically proof of an exploitable vulnerability, and a passing test does not guarantee that the application is secure. Findings should be reviewed, reproduced where appropriate, and addressed through application-level security improvements.

This project is intended for authorized security testing, experimentation, and learning about the practical application of automated red-team testing to AI-powered applications.
