# Brain-Mimicry LLM Architecture

Non-static weights have been explored in the literature. The constraint is compute and scalability.

A drafted architecture mimicking what a human brain does. Tens of billions of weights would not be enough. Training it would be ridiculously costly.

**Weight plasticity by concept type.** Fundamental concepts: static weights, or allow small shifts (e.g. ±0.2 on a [−1, 1] range). Applied / cross-domain registers: allow larger shifts.

**Different /dream timers** per concept type.

**Specialisation, then MoE.** Submodels trained on specific subjects, then assembled into a Mixture-of-Experts architecture. The submodels yabble in audiovisual format (including text), and a principalized model stares at the chaos. The principled model is trained around reasoning, not based on domain-specific ideas.

**Biological neurons to offset cost.** Silicon sounds like a dead end. Silicon has too high an activation energy / gradient vs. carbon.
