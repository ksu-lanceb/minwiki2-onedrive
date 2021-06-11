This is a version of minwiki2 that's designed to store files on OneDrive.

- Files are pulled from OneDrive using rclone prior to viewing. If the file doesn't yet exist, nothing happens, so the file is created.
- Files are pushed to OneDrive using rclone after editing.
- This ensures that the latest versions of all your notes are always available on all machines.
- OneDrive will render .md files online.