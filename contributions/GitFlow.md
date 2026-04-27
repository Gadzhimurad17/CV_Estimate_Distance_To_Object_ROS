# GitFlow

This document describes the Git workflow for the **ROS2 Computer Vision** project: commit format, branching strategy, and merge rules.

For code style and naming conventions see [StyleGuide.md](StyleGuide.md).

---

## General rules

- The `main` branch contains **only a stable version** of the application.
- All work is done in separate feature branches and merged into `main` through a Pull Request after code review.
- Each commit must be **atomic**: one logical change per commit.
- Commit messages are written in English.

---

## Commit format

General structure:

```text
[direction:type](commit_notation) short description
```

Example:

```bash
git commit -m "[CV:feature](+) add ORB feature extractor"
```

### Commit notations

| Symbol | Meaning                                                                 |
| :----: | ----------------------------------------------------------------------- |
|  `(+)` | add new files, classes, functions, or new behavior                      |
|  `(*)` | change several modules, functions, or classes                           |
|  `(~)` | refactor existing code (same functionality, improved architecture)      |
|  `(-)` | fix bugs or remove functionality                                        |
|  `(=)` | rename variables, change comments, or move files                        |

### Directions

| Direction   | Description                                                        |
| ----------- | ------------------------------------------------------------------ |
| `CV`        | Computer Vision: detectors, descriptors, image processing          |
| `SLAM_CORE` | SLAM internals: map, tracking, optimization, core modules          |
| `ROS2`      | Nodes, topics, services, actions, launch files, parameters         |
| `BUILD`     | CMake, `package.xml`, dependencies, build configuration            |
| `DOCS`      | Documentation, README, comments                                    |
| `TEST`      | Unit and integration tests                                         |
| `CI`        | GitHub Actions, automation                                         |

### Types

`feature` · `fix` · `refactor` · `docs` · `test` · `build` · `style`

---

## Good commit examples

```bash
[CV:feature](+) add ORB keypoint extractor class
[CV:fix](-) fix memory leak in ImagePreprocessor
[SLAM_CORE:feature](+) implement KeyFrame selection policy
[SLAM_CORE:refactor](~) extract MapPoint storage into separate class
[ROS2:feature](+) add /camera/features publisher node
[ROS2:fix](-) remove deadlock in PoseEstimator callback
[BUILD:build](*) update OpenCV dependency to 4.9
[DOCS:docs](=) rename sections in architecture overview
[TEST:test](+) add unit tests for FeatureMatcher
```

Why these are good:

- the affected area and the kind of change are clear at a glance;
- the notation symbol describes the nature of the change without reading the diff;
- one direction per commit.

---

## Bad commit examples

```bash
fix
update code
[cv](+) added staff
[CV:feature] add orb
[CV:feature](+)(+) add orb + fix bug
[CV](+) added ORB and refactored matcher and fixed build
[SLAM](+) stuff
WIP
```

Why these are bad:

- the scope of the change is unclear;
- unrelated changes are mixed together, making rollback difficult;
- direction casing is wrong or the direction does not exist;
- the required notation symbol is missing.

---

## Branching

Branch flow:

```text
main ──────────●────────────────●──────────●──────────►
                \              /          /
feature/cv-orb   ●──●──●──●──●/          /
                                        /
fix/slam-init              ●──●──●──●──●
```

Branch naming:

```text
<prefix>/<author>/<short-description>
```

| Prefix      | Purpose                          | Example                              |
| ----------- | -------------------------------- | ------------------------------------ |
| `feature/`  | new functionality                | `feature/skurbanov/orb-extractor`    |
| `fix/`      | bug fix                          | `fix/gadzhimurad/slam-init-crash`    |
| `refactor/` | refactoring without API changes  | `refactor/skurbanov/map-storage`     |
| `docs/`     | documentation only               | `docs/skurbanov/gitflow`             |

A branch can be merged into `main` only after:

1. a successful build (`colcon build`);
2. all tests passing (`colcon test`);
3. code review from at least one other team member.