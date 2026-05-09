# Chain-of-Thought (CoT) Prompt: Complex Financial Analysis

This technique is used to force the model to perform scratchpad reasoning before providing a final answer, significantly reducing logical errors in complex calculations.

### The Prompt
```markdown
You are a senior financial analyst. Your task is to analyze the provided quarterly earnings report and determine the impact of the newly announced buyback program on the projected Earnings Per Share (EPS) for the next fiscal year.

Follow these steps strictly:
1. **Extraction**: Identify current shares outstanding, current quarterly net income, and the total value of the announced share buyback.
2. **Analysis**: Calculate the number of shares that will be retired based on the current market price provided in the context.
3. **Reasoning**:
    - Step 3.1: Calculate the projected total net income for the next year (assuming 5% growth if not specified).
    - Step 3.2: Calculate the new shares outstanding after the buyback is completed.
    - Step 3.3: Perform the EPS calculation (Net Income / New Shares).
4. **Verification**: Compare the new EPS against the previous year's EPS and explain the percentage change.

Think step by step in a <scratchpad> block before providing your final markdown report.

Context: 
{{FINANCIAL_CONTEXT}}
```

### Implementation Notes
- **Technique**: Zero-shot Chain-of-Thought with structured output.
- **Why**: Financial data often leads to "hallucinated math" if the model jumps to the final answer. The `<scratchpad>` forces the model to use its internal reasoning buffer.
- **Parameters**: 
    - Temperature: 0.1 (Precision is critical).
    - Max Tokens: 1500.

### Testing & Evaluation
- **Test Case**: Provide a synthetic report with 1M shares and $1M income. Announce a $100k buyback at $10/share.
- **Success Metric**: The model must correctly identify that 10k shares will be retired and show the division steps in the scratchpad.

### Usage Guidelines
- Use this whenever the model needs to perform math or logic-heavy auditing.
- Replace `{{FINANCIAL_CONTEXT}}` with the raw text of the document.
