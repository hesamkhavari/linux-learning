# Filesystem Maintenance and Repair

## 1. Core Concepts

- `Inode` : Stores file metadata such as **permissions, ownership, timestamps, and pointers to data blocks**. Filenames are stored in directories and reference inode numbers.

- `Journaling` : Records filesystem metadata changes in a journal to help restore filesystem consistency after an unexpected shutdown. It is not a backup mechanism.

- `Filesystem Check` : Detects and may repair filesystem inconsistencies.

- `Mount / Unmount` : Attaches or detaches a filesystem from the directory tree.

