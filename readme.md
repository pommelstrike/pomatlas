# BG3 Icon Manager

[![Python Version](https://img.shields.io/badge/python-3.12-blue.svg)](https://www.python.org/downloads/release/python-3120/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.8.0-brightgreen.svg)](https://github.com/yourusername/bg3-icon-manager/releases)

A graphical user interface (GUI) tool built with PyQt6 for managing icon atlases in Baldur's Gate 3 (BG3) mods. This tool simplifies the process of loading, editing, adding, replacing, and resizing icons in `.lsx` atlas files and associated `.dds` textures. It supports both standalone atlas files and mod project structures, with features like multi-size previews, context menus, and automated resizing for game-compatible formats.

## Features

- **Load and Preview Atlases**: Load `.lsx` atlas files and preview the full atlas texture with interactive tooltips showing MapKeys on hover.
- **Add/Replace Icons**: Easily add new icons or replace existing ones in the atlas, with automatic UV mapping and DDS updates.
- **Resize Icons**: Automatically resize PNG icons to BG3-required sizes (72px, 144px, 192px, 380px) for items or skills, exporting to appropriate game folders.
- **Create New Atlases**: Generate empty or populated atlases from a folder of PNGs, with options for prefixes, auto-resizing, and canvas sizes (512x512 or 1024x1024).
- **Context Menu Actions**: Right-click icons in the preview for quick actions like full-size preview (multi-tab for all sizes), replace, copy MapKey, or delete (with cleanup of resized versions).
- **Mod Project Support**: Detect and work with BG3 mod structures (e.g., Public/Mods folders), including mod selection dropdown.
- **Save Options**: Save updated atlases directly or as ZIP archives, with DDS compression using texconv (BC3_UNORM or BC7_UNORM).
- **Preferences**: Customizable paths for BG3 data, temp files, output, ZIP exports, logging, and texconv executable.
- **Logging and Console Viewer**: Detailed logging with configurable levels, file output, and a real-time console viewer dialog.
- **Alpha Preservation**: Optimized resizing with alpha dithering to reduce compression artifacts in glows/shadows.

## Screenshots

![Main Interface](https://via.placeholder.com/800x600?text=Main+Interface+Screenshot)  
*Main tab showing atlas preview with interactive hover tooltips.*

![Create Atlas Tab](https://via.placeholder.com/800x600?text=Create+Atlas+Tab+Screenshot)  
*Create Atlas tab for generating new atlases from PNG folders.*

![Preview Dialog](https://via.placeholder.com/800x600?text=Multi-Size+Preview+Dialog)  
*Multi-size preview dialog showing all available icon resolutions.*

*(Replace placeholders with actual screenshots from your repository.)*

## Installation

1. **Clone the Repository**:
   ```
   git clone https://github.com/yourusername/bg3-icon-manager.git
   cd bg3-icon-manager
   ```

2. **Install Dependencies**:
   The tool requires Python 3.12+ and several libraries. Install them via pip:
   ```
   pip install -r requirements.txt
   ```
   *requirements.txt example (create this file in your repo):*
   ```
   PyQt6
   Pillow
   numpy
   colorama
   ```

3. **Download texconv.exe**:
   - The tool uses Microsoft's `texconv.exe` for DDS conversions (from DirectXTex).
   - Download the latest release from [Microsoft/DirectXTex](https://github.com/microsoft/DirectXTex/releases) and place it in the `texconv/` subfolder or add to your system PATH.
   - Alternatively, the tool can auto-download it via the Preferences tab.

4. **Optional: Install Additional Libraries**:
   - For advanced features (e.g., color output in console): Already included in requirements.

## Usage

1. **Run the Tool**:
   ```
   python v6_1_icon_manager.py
   ```
   - The GUI will launch with tabs for Main operations, Create Atlas, and Preferences.

2. **Main Tab**:
   - **Load Atlas**: Browse for a `.lsx` file (supports mod project paths).
   - **Select Icon**: Use the dropdown or right-click in the preview to interact.
   - **Replace/Add**: Select a PNG to replace the selected icon or add a new one (prompts for MapKey).
   - **Save**: Updates `.lsx` and `.dds` files, with optional ZIP export.
   - **Resize**: Select a PNG and resize for item/skill formats, exporting to game folders.

3. **Create Atlas Tab**:
   - Select mod, canvas size, and import options.
   - Optionally import from a PNG folder with prefixes and auto-resizing.
   - Generate new `.lsx` and `.dds` files in mod structure.

4. **Preferences Tab**:
   - Set BG3 data path, temp/output directories, logging options, and texconv path.
   - Save changes for persistent settings.

5. **Console Viewer**:
   - Click "Show Console" to view real-time logs in a scrollable dialog.

**Tips**:
- For mod projects, set your BG3 Data path in Preferences to auto-detect mods.
- Use right-click in the atlas preview for quick actions.
- Logging is enabled by default; check `logs/` for session files.

## Troubleshooting

If you encounter issues while using the tool, here are some common problems and solutions:

### 1. **texconv.exe Not Found or Download Fails**
   - **Symptoms**: Error messages about texconv not being found, or DDS conversions falling back to Pillow (which may lack full support for BC7_UNORM).
   - **Solutions**:
     - Ensure `texconv.exe` is placed in the `texconv/` folder next to the script or added to your system PATH.
     - Use the Preferences tab to specify a custom path to texconv or trigger an auto-download.
     - If download fails, manually download from [Microsoft/DirectXTex Releases](https://github.com/microsoft/DirectXTex/releases) and verify the file is not blocked by antivirus.
     - On non-Windows OS, you may need Wine to run texconv.exe.
     - **Add texconv folder to PATH on Windows 10/11 via PowerShell** (permanent, user-level):
       1. Open PowerShell as Administrator (right-click Start > Windows PowerShell (Admin) or Terminal (Admin)).
       2. Replace `C:\Path\To\Your\texconv` with the actual directory containing `texconv.exe` (e.g., script_dir/texconv):
          ```powershell
          $texconvPath = "C:\Path\To\Your\texconv"
          [Environment]::SetEnvironmentVariable("Path", $env:Path + ";$texconvPath", [EnvironmentVariableTarget]::User)
          ```
       3. Restart PowerShell, your IDE (e.g., VS Code), or the computer for changes to take effect.
       4. Verify: `texconv` (should show usage if in PATH).
       - For system-wide (all users, requires admin): Use `[EnvironmentVariableTarget]::Machine` instead of `User`.
       - Note: The tool also auto-detects local `texconv/texconv.exe` without needing PATH.

### 2. **PyQt6 Not Installed or GUI Fails to Launch**
   - **Symptoms**: ImportError for PyQt6 or the application exits immediately.
   - **Solutions**:
     - Run `pip install PyQt6` (ensure Python 3.12+).
     - If using a virtual environment, activate it before running the script.
     - Check for conflicting Qt installations or environment variables like `QT_QPA_PLATFORM`.

### 3. **Atlas Loading Errors (e.g., XML Parse Error, DDS Not Found)**
   - **Symptoms**: "Could not parse atlas_size or tile_size" or "No atlas path found".
   - **Solutions**:
     - Verify the `.lsx` file is valid XML and follows BG3 atlas structure.
     - Ensure the associated `.dds` file is in the expected path (relative to the `.lsx` or in mod project folders like Public/Mods).
     - Set the correct BG3 Data path in Preferences for mod projects.
     - Check console/logs for detailed errors (use "Show Console" button).

### 4. **Resizing or Conversion Failures**
   - **Symptoms**: Non-square images skipped, banding in alphas, or fallback to Pillow.
   - **Solutions**:
     - Input PNGs must be square; crop/resize them manually if needed.
     - For alpha issues, ensure texconv is used (better compression than Pillow).
     - Verify output paths exist and are writable.
     - If using mod projects, confirm mod folders are correctly structured.

### 5. **Logging Issues (No Logs or Cleanup Fails)**
   - **Symptoms**: No log files in `logs/` or errors deleting old logs.
   - **Solutions**:
     - Check Preferences for logging enabled and correct directory.
     - Ensure the directory is writable; run as administrator if needed.
     - Adjust max log files if cleanup is too aggressive.

### 6. **OS Compatibility Problems**
   - **Symptoms**: Tool runs but DDS conversions fail on Linux/macOS.
   - **Solutions**:
     - Use Wine for texconv.exe on non-Windows systems.
     - Test with Pillow fallback, but note it uses DXT5 instead of BC7_UNORM.
     - Report OS-specific issues in GitHub Issues.

### 7. **General Tips**
   - Always check the console viewer or log files for detailed debug info.
   - If the tool crashes, run from command line: `python v6_1_icon_manager.py` to see traceback.
   - Update dependencies: `pip install -r requirements.txt --upgrade`.
   - For BG3-specific issues, ensure your game data path is correct and mods are properly installed.

If your issue isn't covered, search the [Issues](https://github.com/yourusername/bg3-icon-manager/issues) or open a new one with logs and steps to reproduce.

## Requirements

- **Python**: 3.12.3+
- **Libraries**: PyQt6, Pillow (PIL), numpy, colorama.
- **External Tool**: texconv.exe (DirectXTex) for DDS handling (auto-download supported).
- **OS**: Tested on Windows; may work on Linux/macOS with Wine for texconv.
- **Game**: Baldur's Gate 3 installation (for mod paths).

## Contributing

Contributions are welcome! Please follow these steps:
1. Fork the repository.
2. Create a feature branch (`git checkout -b feature/YourFeature`).
3. Commit changes (`git commit -m 'Add YourFeature'`).
4. Push to the branch (`git push origin feature/YourFeature`).
5. Open a Pull Request.

Report issues or suggest features via [GitHub Issues](https://github.com/yourusername/bg3-icon-manager/issues).

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
