# User Prompt — Example (GVI+IMG+LOC+VEG, Northern Hemisphere)

The user prompt is constructed dynamically per image by `build_user_prompt()` in `notebooks/03_inference_batch.ipynb`.
Below is a representative example for the selected configuration (GVI+IMG+LOC+VEG).

---

**[Image attached as Gemini File API URI]**

```
[Summer Street View Image Metadata]
- Summer GVI: 31.24%
- Location: Toronto, Canada (coordinates: 43.6532, -79.3832)
- Vegetation class composition:
  - tree: 24.80%
  - grass: 6.44%

Predict GVI for:
(a) April
(b) November
(c) February
```

---

## Input flag combinations (paper Table 3)

| Config | USE_IMAGE | USE_LOC | USE_VEG | Header shown |
|--------|-----------|---------|---------|--------------|
| GVI only | ✗ | ✗ | ✗ | `[Summer Street View Image Metadata]` |
| GVI+IMG | ✓ | ✗ | ✗ | `[Summer Street View Image]` |
| GVI+IMG+LOC | ✓ | ✓ | ✗ | `[Summer Street View Image Metadata]` |
| GVI+IMG+VEG | ✓ | ✗ | ✓ | `[Summer Street View Image Metadata]` |
| **GVI+IMG+LOC+VEG** | ✓ | ✓ | ✓ | `[Summer Street View Image Metadata]` ← **selected** |

Target months are set per hemisphere; see `CONFIG` cell in `03_inference_batch.ipynb` for adjustable values.
