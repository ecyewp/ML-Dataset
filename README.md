## How to Split Dataset (structured amd unstructured) for ML training

If you have a single dataset (e.g., all images are in one folder) and you want to split it into train and validation sets automatically, here's how you can do it in Python.

This will work if your dataset is unstructured and needs to be divided based on random splitting or a certain percentage (like 80% for training and 20% for validation).

#### 1. Steps to Split Dataset (unstructured) into train/ and val/ Folders:
Prepare Directory Structure: You need to create a train/ and val/ folder inside the parent folder.

Randomly Split Images: We'll randomly assign each image to either the train/ or val/ folder.

#### 🧑‍💻 Python Code Example to Split Dataset:

```
import os
import shutil
import random

def split_dataset(source_dir, train_dir, val_dir, val_size=0.2):

    """
    Splits dataset into train/val directories.

    Args:
    - source_dir: Path to the source directory containing all images.
    - train_dir: Path to the directory where training images will go.
    - val_dir: Path to the directory where validation images will go.
    - val_size: Fraction of dataset to use for validation.
    """
    
    # Create directories if they don't exist
    if not os.path.exists(train_dir):
        os.makedirs(train_dir)
    if not os.path.exists(val_dir):
        os.makedirs(val_dir)

    # List all files in the source directory
    files = [f for f in os.listdir(source_dir) if os.path.isfile(os.path.join(source_dir, f))]
    
    # Shuffle files to randomize the split
    random.shuffle(files)
    
    # Calculate the number of validation samples
    num_val = int(len(files) * val_size)
    
    # Split the files into training and validation sets
    val_files = files[:num_val]
    train_files = files[num_val:]
    
    # Move files to the appropriate directories
    for file in train_files:
        shutil.move(os.path.join(source_dir, file), os.path.join(train_dir, file))
    
    for file in val_files:
        shutil.move(os.path.join(source_dir, file), os.path.join(val_dir, file))

    print(f"Split {len(files)} files into {len(train_files)} training and {len(val_files)} validation files.")

# Example usage
source_directory = 'dataset/images'  # Your dataset folder with all images
train_directory = 'dataset/train'
val_directory = 'dataset/val'

split_dataset(source_directory, train_directory, val_directory)
```

#### 🔍 How It Works:
Source Directory: source_directory should contain all the images you want to split. These images are assumed to be unclassified (not in any subfolders).

Train/Validation Directories: The script will create train/ and val/ directories and move the images into them.

Random Splitting: The images are shuffled randomly, and a percentage of them (default 20% for validation) are moved to the val/ directory. The rest go into the train/ directory.

Directory Structure After Split:

dataset/
├── train/
│   ├── img1.jpg
│   ├── img2.jpg
│   └── ...
├── val/
│   ├── img3.jpg
│   ├── img4.jpg
│   └── ...

#### 📊 Customizing the Split:
Validation Size: You can change the val_size argument to any percentage. For example, val_size=0.2 means 20% of the images go to the validation set, and the rest (80%) go to the training set.

Folder Names: The code doesn't handle the creation of subfolders for class-based labels (like in your earlier question). If you want to split into class-based folders (e.g., cat/, dog/), you’ll need to modify the script to create these subdirectories based on existing classes in the filenames.

#### 2. Steps for splitting Class-based Folder Structure
If your dataset has subfolders with images already classified, you can adjust the above script slightly:
