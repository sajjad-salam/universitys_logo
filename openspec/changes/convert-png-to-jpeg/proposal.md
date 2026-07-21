## Why

The project currently contains 44 university logo images in PNG format. PNG images with transparency can cause display issues in certain contexts and JPEG is a more universally supported format for web and print applications. Converting to JPEG will improve compatibility while maintaining visual quality.

## What Changes

- Convert all 44 PNG images to JPEG (.jpg) format
- Replace transparent backgrounds with solid white backgrounds (preventing default black background issue)
- Preserve image quality during conversion
- Remove original PNG files after successful conversion

## Capabilities

### New Capabilities

- `image-conversion`: Capability to handle bulk image format conversion from PNG to JPEG with transparency handling

### Modified Capabilities

- None

## Impact

- **Image Assets**: 44 PNG files will be converted to JPEG format
- **File System**: Original PNG files will be removed after successful conversion
- **References**: This is a standalone image repository with no external code references to update
- **Tools Required**: Image processing library (Pillow/PIL) for Python or equivalent
