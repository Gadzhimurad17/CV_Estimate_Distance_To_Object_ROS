# Style Guide

This document defines the coding style for the **ROS2 Computer Vision** project.

The project follows the [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html) as its baseline, with the exceptions and additions listed below.

For Git workflow, commit format, and branching rules see [GitFlow.md](GitFlow.md).

---

## Naming conventions

The main difference from Google Style: public functions use **PascalCase**, internal functions use **camelCase**.

| Scope              | Style          | Example                                    |
| ------------------ | -------------- | ------------------------------------------ |
| Public functions   | **PascalCase** | `ExtractFeatures()`, `ProcessFrame()`      |
| Internal functions | **camelCase**  | `computeDescriptor()`, `normalizeImage()`  |
| Classes            | **PascalCase** | `FeatureExtractor`, `KeyFrame`             |
| Structs            | **PascalCase** | `CameraParams`, `MatchResult`              |
| Variables          | **snake_case** | `key_points`, `frame_id`                   |
| Class fields       | **snake_case_**| `max_features_`, `frame_count_`            |
| Constants          | **UPPER_CASE** | `MAX_FEATURES`, `DEFAULT_FPS`              |
| Namespaces         | **snake_case** | `cv_slam`, `feature_extraction`            |
| Files (C++)        | **snake_case** | `feature_extractor.cpp`, `key_frame.hpp`   |
| Files (Python)     | **snake_case** | `feature_extractor.py`, `slam_node.py`     |
| ROS2 nodes         | **snake_case** | `feature_extractor_node`                   |
| ROS2 topics        | **snake_case** | `/camera/raw_image`, `/slam/map_points`    |
| ROS2 parameters    | **snake_case** | `max_keypoints`, `use_gpu`                 |