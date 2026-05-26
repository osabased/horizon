## Sources

- **Meta-Harness: End-to-End Optimization of Model Harnesses** — primary source for treating harness logic as optimizable code/config, preserving full execution traces, using prior candidate history, and optimizing over multi-objective frontiers.  
  https://arxiv.org/abs/2603.28052

- **DSPy: Compiling Declarative Language Model Calls into Self-Improving Pipelines** — source for treating LM systems as composable programs that can be optimized against explicit metrics instead of hand-tuned prompts.  
  https://arxiv.org/abs/2310.03714

- **GEPA: Reflective Prompt Evolution Can Outperform Reinforcement Learning** — source for trajectory reflection, candidate evolution, and Pareto-style optimization over compound AI systems.  
  https://arxiv.org/abs/2507.19457

- **SWE-bench** — source for issue-resolution benchmark fixtures, reproducible patch evaluation, and SWE-bench-style benchmark adapters.  
  https://www.swebench.com/  
  https://github.com/princeton-nlp/SWE-bench

- **Terminal-Bench** — source for terminal-based agent evaluation, useful for Horizon’s validation routing, tool-use, replay, and terminal-behavior benchmark providers.  
  https://www.tbench.ai/

- **Agent-as-a-Judge** — source for step-level diagnostic evaluation of agent trajectories. Horizon may use this for labels and review hints, but durable policy updates should still require executable replay, causal ablation, and leverage gates.  
  https://arxiv.org/abs/2410.10934
