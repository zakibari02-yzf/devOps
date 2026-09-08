Experiment 1 — Initialize a Repository with a
Professional Branching and Merge Strategy
Aim: To initialize a Git repository and apply a professional feature-branch workflow including
branching, collaborative merging and conflict resolution.
CO: CO1 (Applying – L3) | Module: 1 | Duration: 2 hours
Theory
Feature-Branch Workflow: No development happens directly on main . Every change lives on a
separate branch named feature/* , bugfix/* , or hotfix/* and reaches main only through a
merge/pull request. Merge types: fast-forward (branch is directly ahead — pointer just moves),
three-way merge (creates a merge commit), and squash merge (all branch commits collapse into one).
When two branches modify the same lines, Git cannot decide — this is a merge conflict and must be
resolved manually.
Procedure — Step by Step
Step 1 — Initialize and configure the repository
mkdir devops-lab && cd devops-lab
git init
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
Expected: Initialized empty Git repository in .../devops-lab/.git/
Step 2 — Create the base project on main
echo "# DevOps Lab Project" > README.md
git add README.md
git commit -m "chore: initial commit with README"
git branch -M main
Expected: [main (root-commit) a1b2c3d] chore: initial commit with README
Step 3 — Create the first feature branch (Developer A)
git checkout -b feature/calculator
Create file calculator.py :
CS241513 — Software DevOps and Automation Lab Manual 27/08/26, 2:39 PM
file:///Users/shashadhardas/PythonForDataScience/devops_lab.html Page 5 of 36
def add(a, b):
 return a + b
def subtract(a, b):
 return a - b
git add calculator.py
git commit -m "feat: add calculator module with add and subtract"
Step 4 — Second developer branches from main (conflict setup)
git checkout main
git checkout -b feature/calculator-v2
On this branch, edit calculator.py and change the same region:
def add(a, b):
 """Return the sum of a and b."""
 return a + b
git add calculator.py
git commit -m "feat: add docstring to add function"
Step 5 — Merge the first branch (clean merge)
git checkout main
git merge feature/calculator
Expected output:
Updating a1b2c3d..e4f5g6h
Fast-forward
 calculator.py | 5 +++++
 1 file changed, 5 insertions(+)
Step 6 — Trigger the merge conflict (SOLUTION observation)
git merge feature/calculator-v2
Expected output:
Auto-merging calculator.py
CONFLICT (content): Merge conflict in calculator.py
Automatic merge failed; fix conflicts and then commit the result.
Step 7 — Resolve the conflict (SOLUTION)
Open calculator.py . Git has inserted conflict markers:
CS241513 — Software DevOps and Automation Lab Manual 27/08/26, 2:39 PM
file:///Users/shashadhardas/PythonForDataScience/devops_lab.html Page 6 of 36
<<<<<<< HEAD
def add(a, b):
 return a + b
=======
def add(a, b):
 """Return the sum of a and b."""
 return a + b
>>>>>>> feature/calculator-v2
Solution: delete the markers and keep the combined final version:
def add(a, b):
 """Return the sum of a and b."""
 return a + b
def subtract(a, b):
 return a - b
git add calculator.py
git commit -m "merge: resolve conflict between calculator branches"
git branch -d feature/calculator feature/calculator-v2
Step 8 — Verify the history (final solution state)
git log --oneline --graph --all
Expected output:
* 9i8j7k6 merge: resolve conflict between calculator branches
|\
| * e4f5g6h feat: add docstring to add function
* | c3d4e5f feat: add calculator module with add and subtract
|/
* a1b2c3d chore: initial commit with README
Troubleshooting
Problem Solution
fatal: not a git repository You are outside the project folder — cd devops-lab
first.
Conflicted merge aborted midway Run git merge --abort to return to pre-merge state,
then retry.
error: The branch ... is not fully
merged
Use git branch -D only if certain the work is no longer
needed.
Viva Questions (with Answers)
1. What is the difference between merge and rebase?
Merge preserves history with a merge commit (two parents); rebase replays commits on top of the
target branch, producing a linear history but rewriting commit hashes.
CS241513 — Software DevOps and Automation Lab Manual 27/08/26, 2:39 PM
file:///Users/shashadhardas/PythonForDataScience/devops_lab.html Page 7 of 36
2. What is a fast-forward merge?
When the target branch has no new commits since branching, Git simply moves the branch pointer
forward — no merge commit is created.
3. Why protect the main branch?
To force code review/PR and CI checks before integration, preventing broken or unreviewed code
from entering the production-ready branch.
4. git revert vs git reset?
git revert creates a new commit undoing an old one (safe on shared branches); git reset
moves the branch pointer, rewriting history (safe only locally).