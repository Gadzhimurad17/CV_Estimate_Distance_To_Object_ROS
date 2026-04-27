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

### C++ example

```cpp
namespace cv_slam {

class FeatureExtractor {
 public:
  std::vector<cv::KeyPoint> ExtractKeypoints(const cv::Mat& image);
  cv::Mat ComputeDescriptors(const cv::Mat& image,
                              std::vector<cv::KeyPoint>& keypoints);

 private:
  cv::Mat normalizeImage(const cv::Mat& input);
  void applyMask(cv::Mat& image, const cv::Rect& roi);

  int max_features_;
  float scale_factor_;
};

}  // namespace cv_slam
```

### Python example

```python
class FeatureExtractor:

    def __init__(self, max_features: int = 500):
        self._max_features = max_features
        self._orb = cv2.ORB_create(nfeatures=max_features)

    def ExtractKeypoints(self, image: np.ndarray) -> list[cv2.KeyPoint]:
        gray = self._convertToGray(image)
        return self._orb.detect(gray, None)

    def _convertToGray(self, image: np.ndarray) -> np.ndarray:
        if len(image.shape) == 2:
            return image
        return cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
```

---

## Formatting

### C++

| Rule              | Value          |
| ----------------- | -------------- |
| Indentation       | 2 spaces       |
| Line length       | 100 characters |
| Braces            | same line      |
| Pointer/reference | `int* ptr`, `const cv::Mat& img` |

Use `clang-format` with a `.clang-format` file in the repository root. Recommended base: Google.

Braces are always required, even for single-line `if`/`for`:

```cpp
if (frame.empty()) {
  return;
}
```

### Python

| Rule              | Value          |
| ----------------- | -------------- |
| Indentation       | 4 spaces       |
| Line length       | 100 characters |
| Quotes            | double quotes  |
| Trailing commas   | yes, in multi-line structures |

Use `black` or `ruff format` to enforce formatting automatically.

---

## Comments and documentation

### C++ (Doxygen)

Use `///` for all public classes and public methods:

```cpp
/// Extracts ORB keypoints from a grayscale or BGR image.
///
/// @param image Input image (grayscale or BGR).
/// @param mask  Optional mask to restrict detection area.
/// @return Vector of detected keypoints.
std::vector<cv::KeyPoint> ExtractKeypoints(const cv::Mat& image,
                                            const cv::Mat& mask = cv::Mat());
```

Private methods do not require Doxygen unless the logic is complex.

### Python (Google-style docstrings)

```python
def ExtractKeypoints(self, image: np.ndarray) -> list[cv2.KeyPoint]:
    """Extract ORB keypoints from a grayscale or BGR image.

    Args:
        image: Input image as a NumPy array (grayscale or BGR).

    Returns:
        List of detected keypoints.

    Raises:
        ValueError: If image is empty.
    """
```

### Inline comments

- Write in English.
- Comment **why**, not **what**.
- No commented-out code in the repository.
- TODO format: `// TODO(author): description`.

---

## What to avoid

- `std::cout` / `print()` for logging — use `RCLCPP_INFO` / `self.get_logger().info()`.
- Debug output or commented-out code in commits.
- Mixing tabs and spaces.
- Files longer than 500 lines — split into smaller modules.
- Functions longer than 50 lines — extract helpers.
- Magic numbers — use named constants.

```cpp
// bad
if (distance < 0.7) { ... }

// good
constexpr float LOWE_RATIO_THRESHOLD = 0.7f;
if (distance < LOWE_RATIO_THRESHOLD) { ... }
```