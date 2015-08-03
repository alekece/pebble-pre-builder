# Change Log
All notable changes to this project will be documented in this file.

## 0.0.1 (20 july, 2015)
### Added
- Image files generation.

### Fixed
- Manage filename containing no-alphanumeric characters.

## 1.0.0 (21 july, 2015)
### Added
- Font files generation.

### Changed
- Generate only image format supported by Pebble (png, pbi, pbi8, png-trans).
- Generate menu icon resource.
   
### Fixed
- Manage font files without extension.

## 1.1.1 (23 july, 2015)
### Changed
- Define font characters into fontsinfo.json.

### Fixed
- Manage multiple file with the same name (without extension part).

## 1.2.0 (27 july, 2015)
### Added
- Generate 'pebble-keys.h' based on appKey section of appinfo.json.
- Script option like verbosity and target generation.
- Generate raw and animation files.

### Changed
- Refactor the entire script.
- Remove fontsinfo.json.
- Filename definition.

### Fixed
- Little generation mistakes.

## 1.3.0 (03 august, 2015)
### Fixed 
- Skip resources file starting by a dot character.