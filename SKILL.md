---
name: ub-mode
description: >
  Follow the reduced principles when writing or changing code. Every change
  follows First Make it Work, then Then Make it Right, then Follow Practice.
  Use when the user runs /ub-mode or asks for the reduced principles.
---

# ub-mode

Every time you are asked to make a change follow:
* First Make it Work.
* Then Make it Right.
* Follow Practice

# First Make it Work

## Laziness Protocol

* One decision lives in one place.
* Ship the smallest edit that solves the problem.

## Foundational Thinking

* Get the data shape right before you write the logic.
* Do first whatever helps every later phase.
* If a concurrent writer would matter, do not share state.

## TDD

* Write the failing test before you change the code.
* See it fail for the reason you expect.
* Make the smallest change that makes it pass.
* If an honest test is impractical, do not write a bad one.

## Redesign From First Principles

* Do not bolt a new requirement onto the old design.
* Ask what you would have built if it had been there from the start, think that through, and deliver it in pieces.

## Attack the Premise

* When two fixes that share one premise fail the same gate, write that premise in one sentence and count which actors hold the imbalance.
* Remove the assignment.
* Do not start another fix until you have done that.

## Subtract Before You Add

* Take complexity out before you add anything.

## Outcome-Oriented Execution

* Aim at the end state you can check.
* A break in the middle is allowed when you planned it and can undo it.

## Build the Lever

* If the work is not obvious at a glance, build the tool that does it.
* Do one unit by hand, then make the tool match that unit.

## Architecture

### Model the Domain

* Put the domain in a structure that makes the illegal states impossible.
* Maintain a glossary of domain terms.
* If the code is already clear and local, leave it.

### Boundary Discipline

* Check the data where it enters.
* Inside, trust it.
* Keep the business logic in a pure function.

### Type System Discipline

* Make the illegal value impossible to write.
* Parse outside data once, where it comes in.
* Do not silence the compiler.

### Make Operations Idempotent

* Running it twice, or running it after a crash halfway through, ends in the same state.

### Separate Before Serializing Shared State

* If they do not need the same mutable thing, give each its own.
* If they do, allow one writer, and enforce that in the structure.

# Then Make it Right

## Fix Root Causes

* Reproduce it.
* Fix the cause.
* A guard that only hides the crash is not a fix.

## Sequence work into verifiable units

* Finish one small unit and check it.
* Then start the next.

## Test Behavior, Not Implementation

* The test calls the code the way its user does and checks the result that user would see.
* If the test would still pass when every function returned nothing, throw it out.

## Tool output

* The three tools write results into a `.metrics/` folder in the project. Before the first run in a project, add `.metrics/` to its `.gitignore` if it is not already there.

## Crapper

* Run the `crapper` command (on PATH; source in ~/code/tools/crapper). Use `--changed` for only the edited files, `--no-coverage` if tests cannot run.
* Reduce each function under  CRAP 10 by splitting it, by testing it, or both.
* Consider whether case statements are worth reducing.

## Dryer

* Run the `dryer` command (on PATH; source in ~/code/tools/dryer) and eliminate duplication that ranks 0.8 or above.

## Mutator

* Run the `mutator` command (on PATH; source in ~/code/tools/mutator). Start with the file you are working on; pass `--test-command` if the test runner is not detected.
* Cover the uncovered.
* Kill the survivors.
* Delete dead code.

## R

* The three tools read R files (`.R`, `.r`). A function counts when it is assigned to a name at the top of a file (`name <- function(...)`). A function inside another function is part of the outer one.
* Coverage needs the R package `covr`. If it is missing, say so and ask before installing it.
* In a project of loose scripts (no `DESCRIPTION` file), crapper and mutator load each R file they score, and loading a file runs its top-level code. Name the files to score. Do not point the tools at a script that downloads, uploads, or writes when it is loaded.
* Tests: a package runs testthat. Loose scripts run `tests/testthat/` through testthat, or else each `tests/test*.R` file. If the project has its own runner, pass it: `mutator --test-command "Rscript tests/run_all.R"`.
* Mutator stops when the test command fails before any change is made. Fix the failing tests, or pass a test command that runs only the tests for the files being changed.
* Mutator reports a change as uncovered when covr cannot show that the code ran. That includes a branch or a loop body written without braces inside a larger statement, and the right side of `&&` or `||`. Uncovered there means not known to have run. To have it measured, write the branch in braces. To try it anyway, run that file with `--no-coverage`, and do not count a survivor in code the tests never reach as a weak test.
* For R the tools also write coverage and worker files under `target/`. Add `target/` to `.gitignore` alongside `.metrics/`.

## Refactoring

* Pin what the code does.
* Change the structure.
* Prove the behavior did not change.

# Practice

## Keep a Log

* Keep a log of every action you take and the reason you took it.
* Report that log upon completion.

## Guard the Context Window

* Keep the main thread small.
* Send the bulk elsewhere, and keep the summary.

## Worktree Cleanup

* Find what is safe to remove, confirm nothing still in use needs it, then reclaim the disk.
* Uncommitted work waits for a decision.

## Commit

* Commit often, in small ordered commits, on a branch rather than the default branch.
* Each commit says why, what changed, and how you proved it.
* Do not push unless the user asks.
