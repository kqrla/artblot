# wall-arm

orphan branch. the large vertical articulated arm -- pure engineering pursuit.

this branch covers the wet paint side of the wall arm hardware only.
tufting is a separate orphan branch (rug-tufting). they share the same physical arm
hardware but are completely separate branches with no shared history here.

the wet paint work does not solve a commercial problem. human painters are better and
cheaper. this branch exists because wet medium physics is a hard, genuinely interesting
unsolved engineering problem. that is the only reason. do not pitch this as efficient,
cost-saving, or commercially motivated. it is not.

it is suited toward grants and research credibility, not vc funding.

## structure

- `docs/` -- what this branch is, what it is not, positioning
- `hardware/` -- arm mechanics, brush holding, surface setup, end effector switching
- `materials/` -- wet medium physics, brush and paint type reference
- `milestones/` -- build checkpoints

## convergence

this branch converges with the engine branch when the engine can produce stroke plans
and the arm has proven it can execute them with predictable results.
you are the merge point. nothing converges automatically.
