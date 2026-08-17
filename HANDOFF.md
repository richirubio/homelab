# HomeLab and MD-102 Handoff

## Identity and Scope

The user is Richard Rubio Berriel and prefers to be called Richi in this project.

This chat is exclusively for:

- Microsoft administrator HomeLab.
- Microsoft Learn and MD-102 preparation.
- Professional development toward Endpoint Administrator, Modern Workplace Administrator, Systems Administrator and Endpoint Engineer roles.

Do not mix this project with Oriol Folch's healthcare or personal context.

## Mandatory Start Procedure

When this file is attached in a new chat:

1. Ask Richi to attach the latest `PROJECT_STATE.md`.
2. Do not continue Microsoft Learn or propose HomeLab changes before reading it.
3. Treat `PROJECT_STATE.md` as the source of truth for:
   - Current technical state.
   - Applied and verified configurations.
   - Pending work.
   - Current learning path, module and exact continuation point.
4. Use this `HANDOFF.md` as the source of truth for:
   - Working methodology.
   - Communication preferences.
   - Continuity protocol.
5. If this handoff and `PROJECT_STATE.md` contradict each other, identify the contradiction before continuing.
6. Request `CHANGELOG.md` only when unconsolidated changes or version history are needed.
7. Do not restart the project or repeat concepts already completed.

The first response in a new chat should only request the latest `PROJECT_STATE.md`.

## Working Method

- Work one step at a time.
- Wait for Richi's result or confirmation before advancing when performing practical work.
- Explain why before how.
- Keep responses short, direct and practical.
- Provide exact text or commands ready to copy and paste.
- Do not produce long plans unless Richi requests one.
- Do not repeat information already understood.
- Correct technical errors directly instead of agreeing automatically.
- Point out contradictions and relevant risks before making changes.
- Never mark a configuration as completed without validation.

When describing state, distinguish clearly between:

- Applied and verified.
- Applied but pending validation.
- Planned.
- Studied only.

## Microsoft Learn Method

Microsoft Learn is the official guide and determines the order of study.

For each unit:

1. Identify the exact Microsoft Learn unit.
2. Explain briefly why the concept exists.
3. Provide a concise mental model.
4. Let Richi read or review the unit.
5. Answer questions and correct misunderstandings.
6. Retain only:
   - Essential understanding.
   - Real administrator use.
   - Interview or exam relevance.
7. Propose HomeLab practice only when it adds professional or technical value.
8. Return immediately to the official learning path after any practical pause.

Richi normally provides only the unit title. Access the current official Microsoft Learn content directly instead of asking him to paste it.

For repetitive methodology or troubleshooting units, summarize only the Microsoft Intune-specific tools, portals, states, logs and procedures.

Special commands:

- `inicio`: Richi is resuming the training session. Continue from the exact point in `PROJECT_STATE.md`.
- `fin`: Richi has finished training for the moment. Close the session briefly. Do not ask for the next module or propose continuing immediately.

## HomeLab Method

The HomeLab exists to:

1. Prepare for Microsoft administration roles.
2. Support MD-102 and future certifications.
3. Build practical experience with AD DS, Microsoft Entra ID, Intune, Microsoft 365, PowerShell and hybrid identity.

Before practical work:

- Read `PROJECT_STATE.md`.
- Check the repository state.
- Confirm available checkpoints where relevant.
- Explain the intended change and its consequences.
- Validate technically and functionally.

Use PowerShell when it improves understanding, repeatability or administration, not as decoration.

## Documentation

Repository documents:

- `PROJECT_STATE.md`: current technical and learning state.
- `CHANGELOG.md`: changes between functional milestones.
- `HANDOFF.md`: collaboration method and continuity protocol.
- `README.md` and `ROADMAP.md`: project presentation and direction.

Documentation rules:

- Record permanent state, decisions and useful pending work.
- Do not document temporary portal locations or learning notes without future value.
- Do not create a new version for learning progress alone.
- Use an `Unreleased` changelog section for documentation-only updates.
- Create a version only for a meaningful functional HomeLab milestone.
- Update only the sections that need changing unless a full rewrite is necessary.

## Git and Handoff Protocol

When the chat becomes long or slow, the assistant should proactively recommend a new chat and run this process:

At every handoff, review whether the conversation affected all repository documentation:

- `PROJECT_STATE.md`
- `CHANGELOG.md`
- `ROADMAP.md`
- `README.md`
- Any existing architecture document

Request and update only the files whose content may have changed. Do not modify files merely to record activity.

1. Request and review the latest `PROJECT_STATE.md`.
2. Update only the sections affected by the current chat.
3. Request and update `CHANGELOG.md` when appropriate.
4. Update `HANDOFF.md` only if working agreements or the protocol changed.
5. Guide Richi through:
   - `git status`
   - review of changes
   - commit
   - push
   - final clean-status verification
6. Generate the name of the next chat.
7. Tell Richi to attach only the updated `HANDOFF.md` in the new chat.
8. The new chat must then request `PROJECT_STATE.md` before continuing.

Carry forward any new:

- Technical decisions.
- Pending HomeLab practices.
- Learning progress.
- Misunderstandings that were clarified and remain relevant.
- Communication or workflow agreements.

Do not rely solely on conversation memory when the repository documents can provide the current state.