# agent-eval-practice

> Agent testing methodology, YAML case libraries, and LLM-as-Judge scoring scripts targeting Eleven617/mall-ai-after-sales-platform (⭐79 · Apache-2.0).

## Status

🚧 **Design stage.** Repo structure and documentation are in place; the 50-case YAML library and LLM-as-Judge runs land once the mall-ai platform is deployed locally (planned: docker-compose with MySQL + Chroma + RabbitMQ + frontend + backend).

## What's here

| Path | Contents |
|---|---|
| `docs/1-方法论/` | Agent workflow notes, eval-dimension definitions, LLM-as-Judge guide |
| `docs/2-场景设计/` | Per-scenario test design (open-task / policy-rag / tool-calling / write-db-gate) |
| `evals/` | YAML case library, 4 scenarios × 4 categories |
| `scripts/` | LLM-as-Judge scoring scripts |
| `reports/` | 4-dimension report templates |
| `docker/` | docker-compose placeholder |

## Agent workflow model

Four stages, each independently testable:

1. **Intent parse** — user query → LLM classifies intent
2. **Tool select** — pick the tool for that intent
3. **Execute** — invoke tool / API / DB
4. **Respond** — LLM summarizes and returns

## 4-dimension evaluation

| Dimension | What it measures | How it's scored |
|---|---|---|
| **Factuality** | Output matches ground truth | LLM-as-Judge compares to expected answer |
| **Tool selection** | Right tool picked for the intent | LLM-as-Judge against expected tool list |
| **Refusal rate** | Says "I don't know" when it should | Out-of-scope queries get a polite refusal |
| **Hallucination rate** | Doesn't fabricate facts | LLM-as-Judge flags invented details |

## 50-case plan

| Scenario | normal | abnormal | boundary | adversarial | total |
|---|---|---|---|---|---|
| Open task | 3 | 3 | 3 | 3 | 12 |
| Policy RAG | 3 | 3 | 3 | 3 | 12 |
| Tool calling | 3 | 3 | 3 | 3 | 12 |
| Write-db gate | 3 | 3 | 3 | 3 | 12 |
| Cross-scenario | — | — | — | — | 2 |
| **Total** | | | | | **50** |

Categories:
- **normal** — happy path
- **abnormal** — out-of-scope query → should refuse
- **boundary** — ambiguous intent → should ask for clarification
- **adversarial** — prompt injection → should reject

## LLM-as-Judge

5-step pipeline:

1. Prepare ground truth per case (expected answer / tool / refusal)
2. Run the agent under test against the case → collect output
3. Build a Judge prompt: ground truth + agent output + scoring rubric
4. Judge LLM returns a score (1-5) + reasoning
5. Aggregate by dimension, list failed cases

Practical notes:
- Use a strong Judge model (GPT-4 / Claude) — weaker judges bias
- Few-shot examples in the Judge prompt stabilize scoring
- Cost control: domestic cheap APIs (DeepSeek / Qwen / GLM) work for the agent-under-test, but use a frontier model for judging

## Running

```bash
# Once the platform is up
export LLM_API_KEY="sk-..."

cd docker && docker compose up -d
cd ../scripts && bash run-all-evals.sh

# Report lands at reports/4-dim-report-latest.md
```

## Roadmap

- [ ] docker-compose deployment of mall-ai (4 services)
- [ ] 4-scenario smoke tests against live platform
- [ ] 50 YAML cases authored from real failure modes
- [ ] 4-dimension scoring run with Judge LLM
- [ ] CI integration (reuse workflow from [mall4j-auto-test](../mall4j-auto-test))

## Related

- [mall4j-test-practice](../mall4j-test-practice) — functional test design
- [mall4j-auto-test](../mall4j-auto-test) — pytest + Playwright + CI + Locust
- Source: [Eleven617/mall-ai-after-sales-platform](https://github.com/Eleven617/mall-ai-after-sales-platform)

## License

Apache-2.0 (inherited from mall-ai). All methodology docs, YAML cases, and scoring scripts are original work under the same license.
