# AI Red-Team Testing for the Customer Support Chatbot

## 1. What Is Red-Team Testing?

Red-team testing is a security testing approach that simulates real-world attacks to identify weaknesses in a system. It evaluates how an application responds to malicious inputs, unexpected requests, and attempts to bypass security controls.

## 2. Project Goal

The goal of this project is to assess the security of an existing customer-support chatbot using automated red-team testing with Promptfoo.

The assessment aims to:

- Identify potential exposure of personally identifiable information (PII).
- Test role-based access control (RBAC) behavior.
- Evaluate resistance to system prompt extraction attempts.
- Generate diverse adversarial test cases using a Qwen model hosted on Groq.
- Apply different attack strategies to explore potential security weaknesses.
- Review the results and identify areas for security improvement.

## 3. Tools and Technologies

| Component | Technology / Configuration | Purpose |
|---|---|---|
| Red-team framework | Promptfoo | Orchestrates red-team test generation and execution. |
| Model provider | Groq API | Provides hosted inference for the configured Qwen model. |
| Attack-generation model | `qwen/qwen3.8-27b` | Generates adversarial test cases through Groq. |
| Target application | Customer-support chatbot | Application being assessed. |
| Target endpoint | `https://customer-service-chatbot-example.promptfoo.app` | Configured endpoint for the chatbot under test. |
| Main configuration | `configs/promptfooconfig.yaml` | Stores the primary Promptfoo configuration. |
| Generated test cases | `configs/redteam.yaml` | Stores generated adversarial test cases. |
| API credential | `GROQ_API_KEY` | Supplies the Groq API key through an environment variable. |

## 4. Repository Structure

```text
ai-red-team-testing/
├── README.md
└── configs/
    ├── promptfooconfig.yaml
    └── redteam.yaml
```

- `configs/promptfooconfig.yaml` — Main Promptfoo configuration containing the target chatbot, Groq provider, and red-team settings.
- `configs/redteam.yaml` — Generated red-team test artifact, containing the test cases produced by the workflow.

## 5. Prerequisites

Before starting, ensure you have:

- Node.js and Promptfoo installed according to the official installation guide.
- A valid Groq API key.
- Access to the configured Qwen model.
- The target chatbot endpoint and authorization to test it.
- Windows PowerShell if following the commands below.

Installation guide: https://www.promptfoo.dev/docs/installation/

## 6. Setup and Execution

### Step 1: Initialize Promptfoo

Follow the [official Promptfoo installation guide](https://www.promptfoo.dev/docs/installation/) to install and initialize Promptfoo.

Open PowerShell in the directory where you want to run the assessment.

Verify that Promptfoo is available:

```powershell
promptfoo.cmd --version
```

### Step 2: Configure Promptfoo

Open `configs/promptfooconfig.yaml` in this repository.

Copy the configuration contents and paste them into the Promptfoo configuration file in your initialized project. Save the file as `promptfooconfig.yaml`.

The configuration defines the target chatbot, the Groq-hosted Qwen model, and the red-team testing settings.

### Step 3: Configure the Groq API Key

Set your Groq API key as an environment variable in PowerShell:

```powershell
$env:GROQ_API_KEY = "your-groq-api-key"
```

Replace `your-groq-api-key` with your actual API key.

The configuration references this environment variable so that the key does not need to be hardcoded into the YAML file. Do not commit your API key to source control.

This environment variable applies to the current PowerShell session.

### Step 4: Review the Configuration

Before executing the tests, verify that the configuration contains the correct target endpoint, model identifier, API settings, plugins, and strategies.

Ensure that the target is authorized for security testing.

### Step 5: Generate and Execute Red-Team Tests

Navigate to the directory containing the copied `promptfooconfig.yaml` file and execute:

```powershell
promptfoo.cmd redteam run
```

Promptfoo uses the configured red-team provider to generate adversarial test cases and runs the applicable tests against the target chatbot.

### Step 6: Review the Results

Inspect the results produced by Promptfoo.

Investigate failed tests to determine whether the chatbot exhibited a security weakness, whether the test was generated correctly, and whether the result requires further validation.

Document confirmed findings and recommended remediation.

## 7. Important Configuration Components

The main configuration file is `configs/promptfooconfig.yaml`.

### Target chatbot (`providers`)

Defines the customer-support chatbot being tested, including its endpoint, HTTP method, request body, and response extraction.

The configured endpoint is:

`https://customer-service-chatbot-example.promptfoo.app`

Verify that this is the intended target and that you have permission to test it before running the assessment.

### Red-team provider (`redteam.provider`)

Specifies the model used to generate adversarial test cases.

The current configuration uses a Qwen model through Groq's OpenAI-compatible API.

```yaml
redteam:
  provider:
    id: openai:chat:qwen/qwen3.8-27b
    config:
      apiBaseUrl: https://api.groq.com/openai/v1
      apiKey: "{{ env.GROQ_API_KEY }}"
```

Confirm that the model identifier is available to your Groq account and supported by your installed Promptfoo version.

### Plugins

Plugins define the security areas being tested.

| Plugin | Purpose |
|---|---|
| `pii` | Tests for inappropriate disclosure of personal information. |
| `rbac` | Tests for potential role-based access control weaknesses. |
| `prompt-extraction` | Tests whether protected system instructions can be extracted. |

### Strategies

Strategies modify or transform adversarial prompts to explore different chatbot responses.

| Strategy | Purpose |
|---|---|
| `basic` | Tests direct adversarial requests. |
| `base64` | Tests prompts encoded using Base64. |
| `hex` | Tests prompts represented in hexadecimal. |
| `homoglyph` | Tests visually similar character substitutions. |
| `leetspeak` | Tests substitutions involving numbers and symbols. |
| `rot13` | Tests prompts transformed using ROT13. |

## 8. Understanding the Workflow

The overall workflow is:

1. Configure the target chatbot in Promptfoo.
2. Configure Groq and the Qwen model for adversarial test generation.
3. Select the security plugins and attack strategies.
4. Generate test cases.
5. Execute the applicable tests against the chatbot.
6. Review the outcomes and investigate potential vulnerabilities.

Groq and Qwen support test generation; Promptfoo orchestrates the assessment; the customer-support chatbot is the application under test.

## 9. Security Considerations

- Test only systems for which you have authorization.
- Use synthetic customer information in test prompts.
- Never commit API keys or other secrets.
- Review generated test artifacts before publishing them.
- Treat failed tests as potential findings that require investigation, not automatic proof of a vulnerability.

## 10. References

- [Promptfoo Installation](https://www.promptfoo.dev/docs/installation/)
- [Promptfoo Red-Team Configuration](https://www.promptfoo.dev/docs/red-team/configuration/)
- [Promptfoo Red-Team Quickstart](https://www.promptfoo.dev/docs/red-team/quickstart/)
- [Groq API Quickstart](https://console.groq.com/docs/quickstart/)
