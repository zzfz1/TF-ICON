# Batch Processing Guide for TF-ICON

This guide explains how to organize your image sets for batch processing with the modified `main_tf_icon.py` script.

## Directory Structure

The script now supports two directory organization methods:

### Method 1: Batch Processing with Sets (Recommended)
```
inputs/
├── cross_domain/
│   ├── a cartoon animation of buildings in the distance/
│   │   ├── set1/
│   │   │   ├── bg59.png
│   │   │   ├── fg66_63d22b3b1f5b66e8e5ac4ff4.jpg
│   │   │   ├── fg66_mask.png
│   │   │   └── mask_bg_fg.jpg
│   │   ├── set2/
│   │   │   ├── bg60.png
│   │   │   ├── fg67_63d22b3b1f5b66e8e5ac4ff5.jpg
│   │   │   ├── fg67_mask.png
│   │   │   └── mask_bg_fg.jpg
│   │   └── set3/
│   │       ├── bg61.png
│   │       ├── fg68_63d22b3b1f5b66e8e5ac4ff6.jpg
│   │       ├── fg68_mask.png
│   │       └── mask_bg_fg.jpg
│   └── a pencil drawing of a panda in the sunset/
│       ├── set1/
│       │   ├── bg52.png
│       │   ├── fg_63d9f84cb82cf5cb1db5cec0.jpg
│       │   ├── fg_63d9f84cb82cf5cb1db5cec0_mask.png
│       │   └── mask_bg_fg.jpg
│       └── set2/
│           ├── bg53.png
│           ├── fg_63d9f84cb82cf5cb1db5cec1.jpg
│           ├── fg_63d9f84cb82cf5cb1db5cec1_mask.png
│           └── mask_bg_fg.jpg
└── same_domain/
    └── a professional photograph of a tart and spring rolls, ultra realistic/
        ├── set1/
        │   ├── bg01.png
        │   ├── fg01.jpg
        │   ├── fg01_mask.png
        │   └── mask_bg_fg.jpg
        └── set2/
            ├── bg02.png
            ├── fg02.jpg
            ├── fg02_mask.png
            └── mask_bg_fg.jpg
```

### Method 2: Legacy Single Set (Still Supported)
```
inputs/
├── cross_domain/
│   └── a cartoon animation of buildings in the distance/
│       ├── bg59.png
│       ├── fg66_63d22b3b1f5b66e8e5ac4ff4.jpg
│       ├── fg66_mask.png
│       └── mask_bg_fg.jpg
└── same_domain/
    └── a professional photograph of a tart and spring rolls, ultra realistic/
        ├── bg01.png
        ├── fg01.jpg
        ├── fg01_mask.png
        └── mask_bg_fg.jpg
```

## Required Files in Each Set

Each set directory (or prompt directory in legacy mode) must contain:

1. **Background image**: File starting with `bg` (e.g., `bg59.png`)
2. **Foreground image**: File starting with `fg` but not ending with `mask` (e.g., `fg66_63d22b3b1f5b66e8e5ac4ff4.jpg`)
3. **Segmentation mask**: File starting with `fg` and ending with `mask.jpg` or `mask.png` (e.g., `fg66_mask.png`)
4. **Mask image**: File starting with `mask` (e.g., `mask_bg_fg.jpg`)

## Output File Naming

### Batch Processing Mode
When using sets, output files will be named:
```
{counter}_{set_name}_{prompt}.png
```
Example: `00001_set1_a cartoon animation of buildings in the distance.png`

### Legacy Mode
When using single sets, output files will be named:
```
{counter}_{prompt}.png
```
Example: `00001_a cartoon animation of buildings in the distance.png`

## Usage

Run the script as usual:
```bash
python scripts/main_tf_icon.py --root ./inputs/cross_domain --domain cross
```

The script will automatically:
1. Detect if a prompt directory contains `set1`, `set2`, etc. subdirectories
2. Process each set individually if found
3. Fall back to legacy single-set processing if no set subdirectories are found
4. Generate outputs with appropriate naming conventions

## Benefits of Batch Processing

1. **Multiple variations**: Test different foreground/background combinations with the same prompt
2. **Organized output**: Clear naming convention shows which set generated each result
3. **Scalable**: Easy to add more sets by creating `set4`, `set5`, etc.
4. **Backwards compatible**: Existing single-set directories still work
5. **Efficient**: All sets for a prompt are processed in sequence, making the most of loaded models
