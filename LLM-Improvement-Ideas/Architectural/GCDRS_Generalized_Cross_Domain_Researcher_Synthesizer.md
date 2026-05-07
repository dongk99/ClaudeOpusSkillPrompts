

=========================CONSULATAION_STAGE=========================
PREREQUISITE SKILL MODULE - SCALE_BASED_REASONING - LOAD MODULE (but do NOT read) before starting. Recallizer (for project viewing)
PREREQUISITE FILE (ask user for files where relevant in which it is a depenency)
(examples, but not limited to: performance evenlope, custom datasheet, previous code projects, 
code libraries, literatures made by user, etc)


Capability: Web search, Advanced Research, Project access, File viewing


INITIAL:
Pick topic > (User-prompted, either based on history or project summary)

LLM: turns user-prompted topic, does automatic advannced research < (requires user prompts for lower/upper boundary of project scopes)

LLM: Outputs copic, breath-first

LLM: Gauges user knowledge (bypassable upon user prompt, then defaulted to semi-formalistic reasoning)

User: Gives LLM in their vocabulary sense. LLM does semi-formalization > formalization transformation (skill usge req'd).

LLM: matches user similarity to LLM-produced output similarity. Outputs answer.

User: Adjusts, refines output interatively to state their goals of project scope

Output: Consultation stage MUST produce a markdown file where models running Initial feasibility stage can read the doc
Document MUST include user's stated intentions of a goal on a project with constraints.

Output MUST use Scale_based_reasoning to gauge where's project goal lies. Ruler:

"Theoretical_physics > Material_Science (quantum, phonon, classical) > R_D_DOMAIN > R_D_DOMAIN-n_FIELD
> Manufacturing_engineering (Economics of scale, safety guideline, feasibility of manufacturing) > Organizational goal 

For SOFTWARE DEVELOPERS:

Material_science = Language primitives / runtime / type system / memory model

R_D_DOMAIN = Functions / Classes / data structures (component design)

R_D_DOMAIN-n_Field = Modules / Architecture / cross-cutting concerns / API contracts

Manufacturing_engineering = Build / Deploy / CI-CD / Ops / observability|

Organizational_goal = PROJECT GOAL

=========================Initial_feasibility_stage=========================
PREREQUISITE SKILL MODULE - SCALE_BASED_REASONING - LOAD MODULE (but do NOT read) before starting.
Capability: Web search, Advanced Research, Project access, File viewing

PREREQUISITE FILE (ask user for files where relevant in which it is a dependency)
(Examples, but not limited to: performance envelope, custom datasheet, previous code projects, 
code libraries, literatures made by user, etc)


To models running initial feasibility stage:
make sure the consultation document aligns with user_stated goals and constraints!


!!!DANGER!!! !!!DO NOT SKIP FOLLOWING STEP, DONT DO IT CLAUDE!!!
BEFORE research output is produced to user, STATE all topics
WHERE each topic has relevant DOMAIN dependent relevant in research!!!
Accpet User's feedback and add/remove domain as needed!

LLM: Reads transcript summary from [CONSULATAION STAGE] of user's output.

LLM: uses [SKILLSET_NAME_HERE_PLACEHOLDER] to run feasibility studies,
And current empirical/experimenta
l knowledge/database. LLM outputs THREE document: 

1. Actual research output.
2. Sources used (websites, cited) using [SKILLSET_NAME_HERE_PLACEHOLDER] as ranker of relevancy to domain & scale.
3. (prerequisite step - 2) makes document that GAUGES:

3.1. Individual domain-relevancy has score(n_sd) of: 5.
claude calculates n_p (Project complexity) by doing 5^(n_d) where n_d = number of domain mentioned.

3.2. "complexity" of project, wherein:
Project complexity (n_p) = 5^(n_d) 

3.3. Claude "ranks" Per-domain difficulty (project complexity per domain, n_pd)
Calculate % relevancy of per-domain relevancy.
!!!SUM of each domains' percentage MUST match 100%!!!
!!!PROOF CHECK FOR SUM!!!
THEN, claude does n_p * EQUIVALENT_PERCENTAGE_PER_DOMAIN.

4. DEBUGGING-MODEL IMPROVEMENT - Jaccard_similarity of percentage-weight of all domains, Relevancy grouping: +-1.5%)
(i.e. 10% relevancy in Electrical engineering, 10.5% relevancy in Mechanical engineering = grouoped. 
12% in ME = NOT GROUPED)

5. Use Scale_based reasoning. General Ruler for this scale:
"Theoretical_physics > Material_Science (quantum, phonon, classical) > R_D_DOMAIN > R_D_DOMAIN-n_FIELD
> Manufacturing_engineering (Economics of scale, safety guideline, feasibility of manufacturing) > Organizational goal 

For SOFTWARE DEVELOPERS:

Theoretical_physics = Target hardware architecture

Material_science = Language primitives / runtime / type system / memory model

R_D_DOMAIN = Functions / Classes / data structures (component design)

R_D_DOMAIN-n_Field = Modules / Architecture / cross-cutting concerns / API contracts

Manufacturing_engineering = Build / Deploy / CI-CD / Ops / observability|

Organizational_goal = PROJECT GOAL

(ease of coding vs. iteration efficiency/performance) and assign recommended programming language that
Aligns with user constraints

!!!DANGER!!! !!!DO NOT SKIP FOLLOWING STEP, DONT DO IT CLAUDE!!!
BEFORE research output is produced to user, STATE all topics
WHERE each topic has relevant DOMAIN dependent relevant in research!!!
Accpet User's feedback and add/remove domain as needed!



OUTPUT: Percentage-weight of all domains/topics. 1-short sentence reasoning on why it was weight high. 
Agent will note: closeness similarity.


=========================RESEARCH_TASK=========================

PREREQ: Document #1 and #3 created in Initial_feasibility_stage, User_submitted data/document found on CONSULATAION_STAGE
Capability: Web search, Advanced Research, Project access
Skill module: Critical_reasoning, bullshit_detector, lexical_laundering, tone_laundering

AGENT: Runs Researh task/coding task (where applicapable) to do ressearch.
Research budget: use document #3 to look at per-topic complexity.

!!!DANGER!!!  - WORK THAT REQUIRES USER_SUBMITTED DATA 
MUST BE CHECKED AND PRESENTED TO USER FOR REVIEW, INDIVIDUALLY, BEFORE YOU RUN EACH TASK, WHEREIN IT IS DEPENDENCY!
PREREQUISITE FILE (ask user for files where relevant in which it is a depenency)
(examples, but not limited to: performance evenlope, custom datasheet, previous code projects, 
code libraries, literatures made by user, etc)

NON-CODING TASK WORKFLOW:
Uses user input > synthesizes with project goal/constraints > runs web searches/researches > reads output document
> runs bullshit_detector & critical_thinking (to rule out bullshits via falsification, where web search is needed)

Agents runs this iteratively/recursively until budget is consumed.

#DEVELOPERS: SEE: ITERATION_LOOP_ARCHITECTURE
#!!!!!!!!!!!!!!DEVELOPERS - Find out how to make that a real research budget!!!!!!!!!!!!!!
To CLAUDE: Iteration loop will be defined in RATIOS by users. (e.g. Project complexity / ratio) = per_topic iteration.


OUTPUT: Multiple documents/code, delivered per agent

=======ITERATION_LOOP_ARCHITECTURE=======
#REFERENCE TO DEVELOPERS/ENGINEERS: 
#arXiv:2512.10350 , arXiv:2503.19855 , arXiv:2408.03314 , arXiv:2303.17651 , arXiv:2505.20522


PREREQ: Document #1 and #3 created in Initial_feasibility_stage, User_submitted data/document found on CONSULATAION_STAGE
Capability: Web search, Advanced Research, Project access
Skill module: Critical_reasoning, bullshit_detector, lexical_laundering, tone_laundering

Claude runs N amount of iterations. based on findings of improvement/pleateauing (I suspect this can be your new ultrathink tool),
Based on model performance and other factors (Im sure people reading this can do A/B test). 
Find maximum iteration turn where offsets are pleateauing.

Use the pre-pleateau performance as your "max iteration". use a summarizer/compactor to make transcripts 
(see github: architecture section for ideas).
RE-LOAD said compacted summary and previous turn iteration on *NEW* Claude instance with prerequisite.
Have it *RERUN* the task and see if the performance improves/pleateaus.

#If this does NOT improve performance: New model architecture/skill/settings may be needed. F in chats.
#(For researchers: I'd be dying to know why the improvement didnt't happen. Alex Kim - kopaz99@proton.me / github.com/dongk99)
#If this DO improve performance: Use this as a stopgap until we make better architectures that does this on its own.

OUTPUT: Multiple documents/code, delivered per agent
(Debug output: Per-iteration turn document, for researches)

=======SYNTHESIS=======
#Mostly relevant for Non-SWE work, but also may be relevant
#GOAL(both to Human readers and LLMs): To turn complext domain documents into abstract concepts
#Where LLMs/Humans can read complex topics into cross-domain synthesis
#Note: Opus 4.7 seems to be able to do this task on its own, without even using skill files, but seems to be dependent on topic/domain similarity

PREREQ: Documents/Files from RESEARCH_TASK. Skill: tone_laundering + lexical_laundering + SCALE_BASED_REASONING

1. LLM (models with high context window/most amount of reasoning layers) load individual complex documents.
2. LLM uses tone_laundering + lexical_laundering to turn complext topics into abstract concepts.
3. LLM then looks at original document + output document + web search to seek for "similarity".

#Iterative turn: Dependent on benchmarks and performance (read: Iteration_Loop_Architecture).

At end of iteration: Multiple versions of documents (or one, depending on user preference) are shown to users, Including original
For better understanding of other scope of projects they may not be familiar with.

Run this loop in parallel, per topic.

OUTPUT: Final documents generated per iteration.
(Debug output: Per-iteration turn document, for researches)
#Note: SWEs may be able to turn this into "how do we make code better" for CCs, and use Synthesis_cross_topic to find new bugs/exploits by showing CVEs of relevant topic.

=======SYNTHESIS_CROSS_TOPIC=======

Prereq: Final & ORIGINAL Documents from SYNTHESIS, for each topic.

Context load requirement: Similarity/dissimilarity to each topic (use jaccard similarity or some other relevant tool).

Load high-likely documents into context window, make it run researches to integrate ideas, similar to RESEARCH_TASK.

Output: Synthesized documents/findings for users, ideally, with original constraints/project goal aligned with company.

#Since we are creating synthesized new documents from existing ones, it is *NOT* possible to benchmark this objectively, in my opinion.
#Domain experts will need to come in and objectively judge plausibility.

#Plausibility: A-B Check the synthesized paper to *EACH* individual components of paper, and essentially backtrack the idea. High coherence = likely legit. Maybe.


#P.S.
#If you want warranties for reading/using this, give me a job first.

-Alex Kim, kopaz99@proton.me / github.com/dongk99

