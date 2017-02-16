# Schultz Software Solutions LLC LAStools Fork #

This is a fork of LAStools (https://github.com/LAStools/LAStools) which is based on the modifications
made in another fork of LAStools (https://github.com/arsf/LAStools.git).  However, this fork is newer
and will be periodically updated as the upstream repository is modified. 

The following changes were made to the makefiles:

* Added an 'install' target in order to install the headers, library and the open source tools (e.g., `las2las`, `las2txt`) to target directory specified.
* Build as a shared library, renamed as libLASlib to avoid conflicts with libLAS.

For more details about LAStools see the included [README](README.txt).

To install run:

```
make
make install PREFIX=/usr/local
```
Setting the `PREFIX` as required.

