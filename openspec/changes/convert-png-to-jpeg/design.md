## Context

The project contains 44 university logo images stored as PNG files. PNG format supports transparency channels (alpha channel), which can cause display issues when converted to JPEG since JPEG does not support transparency. When converting transparent PNG to JPEG without proper handling, transparent areas default to black, which is visually unacceptable for logos.

**Current State:**
- 44 PNG logo files in project root directory
- Some logos may have transparent backgrounds
- No external code references these images (standalone image repository)

**Constraints:**
- Must preserve image quality during conversion
- Must handle transparency correctly (white background, not black)
- Should be a one-time migration process

## Goals / Non-Goals

**Goals:**
- Convert all PNG images to JPEG format with high quality
- Replace transparent regions with solid white background
- Remove original PNG files after successful conversion
- Provide a reproducible conversion script

**Non-Goals:**
- Image editing or enhancement beyond format conversion
- Color space conversion beyond what JPEG requires
- Building a permanent image conversion service
- Maintaining PNG files alongside JPEG copies

## Decisions

### Tool Choice: Python with Pillow (PIL)

**Rationale:** Python's Pillow library is the most mature and well-documented image processing library for Python. It provides robust transparency handling through alpha channel composition.

**Alternatives Considered:**
- **ImageMagick command-line**: Powerful but adds external dependency with different syntax across platforms
- **Node.js sharp**: Excellent but requires Node.js runtime
- **Online converters**: Not automatable and lacks quality control

### JPEG Quality Setting: 95

**Rationale:** JPEG quality of 95 provides near-lossless compression while maintaining reasonable file sizes. Lower values introduce visible artifacts; higher values (near 100) provide diminishing returns with larger files.

### White Background Composition Strategy

**Rationale:** Create a new white RGBA image and use `alpha_composite()` to blend the PNG on top. This ensures:
- Transparent pixels become white
- Semi-transparent pixels blend appropriately with white
- Opaque pixels remain unchanged

**Alternative rejected:** Direct conversion without composition would result in black backgrounds.

## Risks / Trade-offs

| Risk | Mitigation |
|------|-----------|
| JPEG compression artifacts on logos | Use high quality (95) setting; logos are typically simple with limited gradients |
| Loss of transparency for future use | This is intentional - JPEG does not support transparency |
| Conversion script failure mid-process | Implement error handling and only delete PNG after all conversions succeed |
| Some logos may already have black backgrounds | Manual review option provided to identify problematic conversions |

## Migration Plan

1. **Pre-convention**: Python script scans directory for all `.png` files
2. **Conversion**: For each PNG:
   - Load image with alpha channel
   - Create white background canvas of same dimensions
   - Composite original image onto white canvas
   - Save as JPEG with quality=95
3. **Verification**: Script confirms all JPEGs created successfully
4. **Cleanup**: Delete original PNG files (only after successful verification)

## Open Questions

None identified - this is a straightforward image format conversion.
