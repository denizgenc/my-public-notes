# Chapter 5 - How programmers work
This chapter covers
- The different phases of the Software Development Life Cycle (SDLC):
  - Phase 1: Design and Analysis
  - Phase 2: Coding
    - Version control systems (distributed vs centralised)
    - Branching and merging
  - Phase 3: Testing
    - Unit testing, continuous integration (CI)
    - Test environments
  - Phase 4: Release
    - Different ways of deploying applications (PaaS, containers)
    - Build processes
    - Database migration
  - Phase 5: Post-release testing and observation
    - Penetration testing
    - Monitoring and logging
- Managing dependencies

## Phase 1: Design and Analysis
The SDLC doesn't start with code being written, but doing research and coming up with ideas about
what the software/website will accomplish. Figuring out aims before deploying a piece of software
is important, because it's harder to make changes to something after it's deployed instead of
before - even if it's a website (because you might have to maintain backwards compatibility, etc).

You need to figure out the requirements of the project. This either means meeting with stakeholders
(whether they're internal or external) and coming up with a list of goals that the project should
accomplish once it's complete. Once the coding begins, all the team members should be able to track
progress towards meeting these requirements/goals, and the project will be finished once all those
goals are met.

You can use issue/bug trackers, especially with an existing project.

The amount of time spent on this phase depends on this project. Critical infrastructure will
probably spend more time here than a web app.

## Phase 2: Writing Code
Make sure you put any code that isn't a small helper script into a source/version control system
(VCS), like Git.

Large companies tend to host their own servers for source control.

### Branching and Merging Code
Developers can work on a new feature or bugfix away from the main/trunk codebase by creating a new
branch, and then merge it back in after it is ready. Some teams may have a feature branch that
itself is branched, if it's a particularly big feature, etc.

Before a release happens, multiple merges occur to make sure that all the new features and bug fixes
are in place. If the same file has been changed in different ways in the same place, a merge
conflict can occur, and would need to be resolved.

Before merging, it's important to do a code review. The "four eyes principle" states that all code
changes requires two separate people to review it before it is merged into the trunk. The motivation
for this is to reduce the likelihood of potential security vulnerabilities, bugs, and so on.
- GitHub (and other Git-based source control hosting services) formalises this process by creating
  the concept of a "pull request", where a code change in a particular branch requires the approval
  of some other contributor.
  - These services can also make it so that the PR can only be merged after passing a suite of
    automated integration tests, which again can improve the reliability of a code change

## Phase 3: Pre-Release Testing
Code should only be released after it is tested thoroughly. A good testing strategy is key to
reducing bugs and security vulnerabilities.

At the very least, before a code change occurs, the author (and reviewers) should manually test the
functionality of the code (e.g. a website's interface) to make sure it works as expected.

Unit testing is also important - they're small scripts that make assertions about how the code
operates, by running small parts of the code and evaluating the output. Unit tests should be part of
the build process, and are particularly important for sensitive or frequently changing areas of the
codebase. They can also serve as documentation on *how* the code should work.

Unit tests should be simple and only test one thing at a time. Complex unit tests that test multiple
pieces of functionality at the same time are brittle, and could return false positives or false
negatives.

### Coverage and Continuous Integration
A unit test evaluates some function within your codebase. The ratio of the functions tested via unit
tests to the total number of functions in your codebase is referred to as your test coverage. It
might be good to aim for 100% coverage, but note:
- There may be some parts of the code that are pretty well understood and don't require the effort
  to write and maintain tests
- You should write multiple unit tests for the same function, if necessary - to cover different
  valid and invalid inputs. This doesn't increase your coverage however.

So it's possible that a relatively low test coverage could still be pretty solid.

A good rule of thumb is that if you find a bug, write a unit test to assert the correct behaviour of
the function involved in the issue, and then fix the bug. 

Given enough test coverage, setting up a continuous integration (CI) server is a good next step. CI
servers connect to your Git repo, downloads the latest version of your code whenever a change is
made, and then runs your unit tests against them. If there's a failure, you can set up alerts to
notify the team that there's a problem with the code. Again, this makes it easier to spot mistakes
before they're released into the wild.

## Test Environments
