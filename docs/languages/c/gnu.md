### Low level file routines

- working with binary files may be easier using low-level routines.
- low-level file routines do not use text streams; instead, the connection they open to your file is an integer called a `file descriptor`.
- You pass the file descriptor that designates your file to most low-level file routines
- to discover which error or what kind of error has occurred, you must frequently refer to the system variable `errno`
- low-level file functions return some kind of error flag if they cannot perform the action you request:
- `EACCES` - The program is not permitted to search within one of the directories in the file name.
- `ENAMETOOLONG` - Either the full file name is too long, or some component is too long.
- `ENOENT` - Either some component of the file name does not exist, or some component is a symbolic link whose target file does not exist.
- `ENOTDIR` - One of the file name components that is supposed to be a directory is not a directory.
- `ELOOP` - Too many symbolic links had to be followed to find the file
- You can open a file, or create one if it does not already exist, with the `open` command, which creates and returns a new file descriptor for the file name passed to it.
- If the file is successfully opened, the file position indicator is initially set to zero

### Opening files at lower level

- The first parameter of open is a string containing the filename of the file you wish to open. The second parameter is an integer argument created by the bitwise OR of the following file status flags
-
- these flags are defined in macros in the GNU C Library header file fcntl.h, so remember to insert the line `#include <fcntl.h>`
- `O_RDONLY` - Open the file for read access.
- `O_WRONLY` - Open the file for write access.
- `O_RDWR` - Open the file for both read and write access. Same as O_RDONLY | O_WRONLY.
- `O_READ` - Same as `O_RDWR`. GNU systems only.
- `O_WRITE` - Same as `O_WRONLY`. GNU systems only.
- `O_EXEC` - Open the file for executing. GNU systems only.
- `O_CREAT` - The file will be created if it doesn't already exist.
- `O_EXCL` - If `O_CREAT` is set as well, then open fails if the specified file exists already. Set this flag if you want to ensure you will not clobber an existing file.
- `O_TRUNC` - Truncate the file to a length of zero bytes. This option is not useful for directories or other such special files. You must have write permission for the file, but you do not need to open it for write access to truncate it (under GNU).
- `O_APPEND` - Open the file for appending. All write operations then write the data at the end of the file. This is the only way to ensure that the data you write will always go to the end of the file, even if there are other write operations happening at the same time.
- `open` can set `errno` to the following values
- `EACCES` - The file exists but is cannot be does not have read or write access (as requested), or the file does not exist but cannot be created because the directory does not have write access.
- `EEXIST` - Both `O_CREAT` and `O_EXCL` are set, and the named file already exists. To open it would clobber it, so it will not be opened.
- `EISDIR` - Write access to the file was requested, but the file is actually a directory.
- `EMFILE` - Your program has too many files open.
- `ENOENT` - The file named does not exist, and `O_CREAT` was not specified, so the file will not be created.
- `ENOSPC` - The file cannot be created, because the disk is out of space.
- `EROFS` - The file is on a read-only file system, but either one of `O_WRONLY`, `O_RDWR`, or `O_TRUNC` was specified, or `O_CREAT` was set and the file does not exist.

### Writing files

- You can write a block of information to a file with the `write` function.
- It takes three parameters. The first is the file descriptor of the file you wish to write to. The second is a buffer, of type `void *`, that contains the data you wish to write.
- The third parameter is of type `size_t`, and specifies the number of bytes that are to be written.
- The return value of this function is of type `ssize_t`, and indicates the number of bytes actually written
- you should always call `write` in a loop, and iterate the loop until all data has been written. If there is an error, write returns -1
- The write function will return the following error codes in the system variable errno, as well as the usual file name errors.
- `EBADF` - The file descriptor specified is invalid, or is not open for writing.
- `EFBIG` - If the data were written, the file written to would become too large.
- `EIO` - There has been a hardware error.
- `EINTR` - The write operation was temporarily interrupted.
- `ENOSPC` - The device containing the file is full.
- You can call the `fsync` routine to ensure that all data is written to the file; this usage is roughly analogous to the high-level file routine `fflush`.
- The `fsync` routine takes a single parameter, the file descriptor to synchronise. It does not return a value until all data has been written. If no error occurred, it returns a 0; otherwise, it returns -1 and sets the system variable errno to one of the following values:
- `EBADF` - The file descriptor specified is invalid.
- `EINVAL` - No synchronisation is possible because the system does not implement it.
- If you want to find a particular file position within a file, using a low-level file routine, you can call the `lseek` function
- The `lseek` function specifies the file position for the next read or write operation
- The `lseek` function takes three parameters. The first parameter is the file descriptor. The second is of type `off_t` and specifies the number of bytes to move the file position indicator. The third is a constant that specifies whether the offset is relative to the beginning of the file (`SEEK_SET`), to the current file position (`SEEK_CUR`), or to the end of the file (`SEEK_END`).
- If `SEEK_CUR` or `SEEK_END` is used, the offset specified can be positive or negative
- The return value of `lseek` is of type `off_t` and normally contains the resulting file position, as measured in bytes from the beginning of the file.

- If there was an error, `lseek` returns a -1 and sets the system variable errno to one of the following values:
- `EBADF` - The file descriptor specified is invalid.
- `EINVAL` - Either the third parameter of `lseek` is invalid, or the file offset is invalid.
- `ESPIPE` - The file descriptor corresponds to an object that cannot be positioned, such as a terminal device.
- - If you want to delete a file, you can use the low-level file routine `unlink`, as declared in the file `unistd.h`. Simply pass this routine the name of the file you wish to delete.
- The `unlink` function returns 0 if the file or file name was successfully deleted. If there was an error, unlink returns -1
- In addition to the usual file name errors, `unlink` can set errno to the following values:
- `EACCES` - Your program does not have permission to delete the file from the directory that contains it.
- `EBUSY` - The file is currently being used by the system and cannot be deleted.
- `ENONENT` - The file name to be deleted does not exist.
- `EPERM` - Your program tried to delete a directory with `unlink`; this is not permitted under GNU
- `EROFS` - The file name is on a read-only file system and cannot be deleted.

### Delete a directory

- If you wish to delete a directory rather than an ordinary file, use the `rmdir` function. Simply pass it the name of an empty directory you wish to delete. It acts like `unlink` in most respects, except that it can return an extra error code in the system variable `errno`:
- `ENOTEMPTY` - The directory was not empty, so cannot be deleted. This code is synonymous with `EEXIST`, but GNU always returns `ENOTEMPTY`.

### Rename a file

- If you want to rename a file, you can use the `rename` function, which takes two parameters. The first parameter is a string containing the old name of the file, and the second is a string containing the new name
- If `rename` fails, it will return -1. In addition to the usual file name errors, unlink can set errno to the following values
- `EACCES` - Either one of the directories in question (either the one containing the old name or the one containing the new name) refuses write permission, or the new name and the old name are directories, and write permission is refused for at least one of them.
- `EBUSY` - One of the directories used by the old name or the new name is being used by the system and cannot be changed.
- `ENOTEMPTY` - The directory was not empty, so cannot be deleted. This code is synonymous with EEXIST, but GNU always returns ENOTEMPTY.
- `EINVAL` - The old name is a directory that contains the new name.
- `EISDIR` - The new name is a directory, but the old name is not.
- `EMLINK` - The parent directory of the new name would contain too many entries if the new name were created.
- `ENOENT` - The old name does not exist.
- `ENOSPC` - The directory that would contain the new name has no room for another entry, and cannot be expanded.
- `EROFS` - The rename operation would involve writing on a read-only file system.
- `EXDEV` - The new name and the old name are on different file systems.
