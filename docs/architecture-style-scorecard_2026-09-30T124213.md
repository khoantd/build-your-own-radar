# Architecture style scorecard\_2026-09-30 12:42

> AI-generated Richards architecture style scorecard for **ThoughtWorks Radar**. Review and refine before treating as a decision record.
> Selection advice only — capture the chosen style as an ADR when ready.

# Architecture Style Scorecard — ThoughtWorks Radar  
  
## Drivers (ranked)  
1. Cost — Understanding and minimizing operational expenses is crucial for sustainability.  
2. Simplicity — A simpler architecture facilitates onboarding and reduces cognitive load on the team.  
3. Testability — Ensuring changes can be tested in isolation helps maintain code quality.  
4. Deployability — The ability to release updates frequently without significant risk is vital for responsiveness.  
  
## Candidates considered  
- Layered: Suitable for small teams, but may suffer in testability and deployability.  
- Modular Monolith: A good balance for 5–20 devs, but has potential challenges in scalability.  
- Service-Based: Offers a middle ground but may complicate deployment and increase costs.  
  
## Scoring (driver columns only)  
  
| Style                | Cost | Simplicity | Testability | Deployability | Total | Feasibility |  
|---------------------|------|------------|-------------|---------------|-------|-------------|  
| Layered             | ★★★★★ | ★★★★★     | ★★          | ★★            | N/A   | ✅          |  
| Modular Monolith    | ★★★★★ | ★★★★      | ★★★         | ★★★           | N/A   | ✅          |  
| Service-Based       | ★★★★  | ★★★       | ★★★★        | ★★★★          | N/A   | ⚠️          |  
  
## Shortlist  
  
### 1. Modular Monolith  
- \*\*Wins on:\*\* Cost, Simplicity, Testability  
- \*\*Trades away:\*\* May face challenges in adopting microservices later.  
- \*\*Feasibility:\*\* Fits neatly within team size; basic ops maturity supports modest CI/CD practices. Budget constraints kept in check with lower infrastructure costs.  
- \*\*Regret scenario:\*\* Failure may stem from unexpected complexity when scaling if the architecture is not designed with future extraction in mind.  
  
### 2. Layered  
- \*\*Wins on:\*\* Cost, Simplicity  
- \*\*Trades away:\*\* Poorer Testability and Deployability might hinder rapid iterations.  
- \*\*Feasibility:\*\* Well-suited for the current team size and skills; low operational demands favor budget within limits.  
- \*\*Regret scenario:\*\* If project complexity increases, transitioning from a Layered architecture could lead to significant rework.  
  
## Recommendation  
If minimizing costs and ensuring simplicity are critical, opt for the Modular Monolith model. However, if operational expenses are tightly constrained and simplicity is paramount over future scalability, the Layered style is safer.   
  
## Next step  
Hand off to \`software-architecture-mastery\` to capture the chosen style as an ADR.
