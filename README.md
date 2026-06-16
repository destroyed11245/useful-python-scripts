# Useful Python Scripts

A small collection of handy Python scripts to automate everyday tasks.

## 📁 File Organizer

Automatically sorts files in a folder into subfolders by file type (Images, Documents, Videos, Music, Archives, Others).

### Features
- No external dependencies (works with standard Python)
- Safe — does not delete any files
- Easy to run and customize

### How to use

```bash
python organize_files.py /path/to/your/folderExample:bash

python organize_files.py ~/Downloads

Supported file typesImages: .jpg, .jpeg, .png, .gif, .bmp, .svg, .webp
Documents: .pdf, .docx, .txt, .xlsx, .pptx, .csv
Videos: .mp4, .avi, .mkv, .mov
Music: .mp3, .wav, .flac
Archives: .zip, .rar, .7z, .tar, .gz
Everything else goes to Others

RequirementsPython 3.6+

LicenseMIT License

Нажми **Commit changes** (зелёная кнопка внизу).

### Шаг 3: Добавь сам скрипт

1. В своём репозитории нажми **Add file** → **Create new file**.
2. В поле имени файла напиши: `organize_files.py`
3. Вставь вот этот код полностью:

```python
import os
import shutil
from pathlib import Path

def organize_files(directory_path):
    """Organizes files in the given directory into categorized subfolders."""
    
    if not os.path.exists(directory_path):
        print(f"Error: Directory '{directory_path}' does not exist.")
        return

    # Define file categories
    categories = {
        'Images': ['.jpg', '.jpeg', '.png', '.gif', '.bmp', '.svg', '.webp'],
        'Documents': ['.pdf', '.docx', '.txt', '.xlsx', '.pptx', '.csv', '.doc'],
        'Videos': ['.mp4', '.avi', '.mkv', '.mov', '.wmv'],
        'Music': ['.mp3', '.wav', '.flac', '.aac'],
        'Archives': ['.zip', '.rar', '.7z', '.tar', '.gz'],
    }

    # Create category folders if they don't exist
    for category in categories:
        folder_path = os.path.join(directory_path, category)
        if not os.path.exists(folder_path):
            os.makedirs(folder_path)

    others_folder = os.path.join(directory_path, 'Others')
    if not os.path.exists(others_folder):
        os.makedirs(others_folder)

    # Move files
    for filename in os.listdir(directory_path):
        file_path = os.path.join(directory_path, filename)

        # Skip directories and the script itself
        if os.path.isdir(file_path) or filename == 'organize_files.py':
            continue

        # Get file extension
        file_extension = Path(filename).suffix.lower()

        # Find the right category
        moved = False
        for category, extensions in categories.items():
            if file_extension in extensions:
                destination = os.path.join(directory_path, category, filename)
                try:
                    shutil.move(file_path, destination)
                    print(f"Moved: {filename} → {category}/")
                    moved = True
                    break
                except Exception as e:
                    print(f"Error moving {filename}: {e}")

        # If file doesn't match any category
        if not moved:
            destination = os.path.join(directory_path, 'Others', filename)
            try:
                shutil.move(file_path, destination)
                print(f"Moved: {filename} → Others/")
            except Exception as e:
                print(f"Error moving {filename}: {e}")

    print("\n✅ Organization complete!")

if __name__ == "__main__":
    import sys
    
    if len(sys.argv) > 1:
        target_dir = sys.argv[1]
    else:
        target_dir = input("Enter the path to the folder you want to organize: ").strip()
    
    organize_files(target_dir)
## Maintainer
This repository is actively maintained. New scripts and improvements are added regularly.

## How to contribute
Feel free to open an issue or submit a pull request with new useful scripts!
