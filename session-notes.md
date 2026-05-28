# ZMK Config Session Notes - 2026-01-24

## Summary
Attempted to add nice-view-gem display customization to aurora lily58 keyboard with ZMK main branch. Encountered LVGL 9 compatibility issues.

## Branches

### `master`
- Fixed board names for Zephyr 4.1 compatibility:
  - `nice_nano_v2` → `nice_nano`
  - `seeeduino_xiao_rp2040` → `xiao_rp2040`
- Merged `update-macropad-keymap` (macropad keymap change)

### `add-nice-view-gem-pinned-zmk`
- Uses ZMK v0.2.1 (pinned) + original nice-view-gem from M165437
- Uses old board names (`nice_nano_v2`, `seeeduino_xiao_rp2040`)
- **This one works** with nice-view-gem display
- Not merged to master yet

### Deleted: `add-nice-view-gem`
- Attempted to use ZMK main + LVGL 9 compatible fork of nice-view-gem
- Fork at: `oresk/nice-view-gem` branch `lvgl9-compat`
- Did not work - firmware crashed (no USB device, blank display)

## LVGL 9 Migration Attempts (for reference)

Changes made to nice-view-gem for LVGL 9 (in `oresk/nice-view-gem` `lvgl9-compat` branch):

1. **Kconfig.defconfig**:
   - `LV_USE_IMG` → `LV_USE_IMAGE`
   - `LV_USE_ANIMIMG` → `LV_USE_ANIMIMAGE`

2. **Canvas/rotation**:
   - Changed to `LV_COLOR_FORMAT_L8` (8-bit grayscale)
   - Used `lv_draw_sw_rotate()` instead of layer-based rotation
   - Buffer type: `uint8_t cbuf[CANVAS_SIZE * CANVAS_SIZE]`

3. **Image format**:
   - Removed `.header.magic` and `.header.stride` fields
   - Changed `lv_image_dsc_t` → `lv_img_dsc_t`
   - Changed `LV_IMAGE_DECLARE` → `LV_IMG_DECLARE`

These changes compiled but caused firmware crashes. Root cause not fully determined.

## Key Findings

- ZMK v0.3.0 (Aug 2025) introduced LVGL 9 and new board names
- ZMK v0.2.1 (Mar 2025) is the latest version with LVGL 8
- Original nice-view-gem repo: https://github.com/M165437/nice-view-gem
- Nice-view-gem requires `CONFIG_ZMK_DISPLAY_STATUS_SCREEN_CUSTOM=y`

## Files Modified

- `config/west.yml` - ZMK version and nice-view-gem module
- `build.yaml` - Board/shield combinations
- `config/splitkb_aurora_lily58.conf` - Display config
