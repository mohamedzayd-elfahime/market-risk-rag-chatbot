# Response Guardrails

The chatbot remains generative, but its answer is checked before it reaches the user.

## Main Risks

Market risk chatbots are vulnerable to:

- invented metric values;
- confusion between VaR and Expected Shortfall;
- treating a violation rate as a p-value;
- overconfident financial language;
- personalized buy/sell/allocation advice;
- unsupported causal explanations;
- copying raw context dumps into the response.

## Guardrail Strategy

```text
Raw LLM answer
  -> terminology repair
  -> metric consistency checks
  -> advice detection
  -> numeric support check
  -> targeted correction or refusal
  -> final answer
```

## Examples

### Unsupported Number

If the answer includes a number that is not present in the dashboard state, structured files, or retrieved context, the system should either remove it or add a fallback correction.

### Financial Advice

If the user asks whether to buy, sell, short, allocate, or trade, the chatbot should refuse personalized advice and redirect to risk interpretation.

### VaR / ES Confusion

The assistant should preserve the distinction:

- VaR is a threshold loss level.
- Expected Shortfall is the average loss beyond the VaR threshold.

### Backtest Metric Confusion

Expected violation rate is a theoretical expected VaR violation frequency. It is not a Kupiec or Christoffersen p-value.

## Design Choice

Guardrails should be targeted. The system should avoid replacing every answer with deterministic templates. The LLM can still explain, but critical risk semantics are validated.
