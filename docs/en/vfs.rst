.. _vfs:

==============================================
File I/O and Unitex Virtual File System access
==============================================

Unitex tools have a high I/O workload. To improve the I/O performance
it is possible to use the Unitex Virtual File System (:abbr:`VFS(Virtual File System)`)
access layer which, at an application-level, behaves like a RAM drive
and locate files on it.

Virtual files are identified by a prefix string:

==========  ============================================================================== ============
**Prefix**  **Implementation**                                                             **License**
==========  ============================================================================== ============
``$:``      Open source Virtual File System included starting Unitex 3.0                   LGPLv2
``*:``      Ergonotics Virtual File System for Unitex 2.1 and higher                       Proprietary
==========  ============================================================================== ============

The Unitex shared library export some common utility functions for
file handling, these function are compatible with both the virtual
file system and the standard file system.

.. index::
    pair: Virtual File System; C

.. _C:

C
#

.. note::

  
  For those functions using file I/O with binary buffers, it is the caller
  of the function the one in charge of implementing the proper Unicode handling
  (UTF8 ou UTF16).

.. hint::

  The Unitex VFS system is a flat file system, with no notion of directories/folders.
  Some characters like ``:`` (colon), ``/`` (slash) and ``\`` (backslash) are treat as
  the same as any other character, thus allowing a consistent behavior with a
  hierarchical file system.
  

``UnitexAbstractPathExists``
----------------------------

``UnitexAbstractPathExists`` Checks whether a prefix corresponds to a virtual
file system.

.. code-block:: cpp

    int UnitexAbstractPathExists(const char* path);

For example, to check the best VFS layer available you could implement the
following function:
 
.. code-block:: cpp

    const char* getVirtualFilePrefix() {
      if (UnitexAbstractPathExists("*:") != 0) {
        return "*:";
      }

      if (UnitexAbstractPathExists("$:") != 0) {
        return "$:";
      }

      return NULL;
    }


``GetUnitexFileReadBuffer``
---------------------------

``GetUnitexFileReadBuffer`` Returns the contents of the specified file as a
read-only pointer. This  pointer will remain valid until call ``CloseUnitexFileReadBuffer``.

.. warning::

    Do not modify or remove the pointed file while it is being used, i.e.
    between the call of ``GetUnitexFileReadBuffer`` and ``CloseUnitexFileReadBuffer``

.. code-block:: cpp

    void GetUnitexFileReadBuffer(const char*  name, UNITEXFILEMAPPED** amf,
                                 const void** buffer, size_t* size_file);

``WriteUnitexFile``
-------------------

``WriteUnitexFile`` Creates a file using 1 or 2 binary buffers. If you need only
1 buffer, you need to set ``buffer_suffix`` to ``NULL`` and ``size_suffix`` to
``0``

.. code-block:: cpp

    int WriteUnitexFile(const char* name, const void* buffer_prefix, size_t size_prefix,
                                          const void* buffer_suffix,size_t size_suffix);

``AppendUnitexFile``
--------------------

``AppendUnitexFile`` Appends text to an existing file, or to a new file
if the specified file does not exist.

.. code-block:: cpp

    int AppendUnitexFile(const char* name,const void* buffer_data,size_t size_data);

``RemoveUnitexFile``
--------------------

``RemoveUnitexFile`` Deletes the specified file.

.. code-block:: cpp

    int RemoveUnitexFile(const char* name);

``RenameUnitexFile``
--------------------

``RenameUnitexFile`` Causes the filename referred to by *oldName* to be changed
to *newName*.

.. code-block:: cpp

    int RenameUnitexFile(const char* oldName,const char* newName);

``CopyUnitexFile``
------------------

``CopyUnitexFile`` Copies an existing file to a new file. This function could be
used to copy a file (in both senses) between the virtual and the standard file system.

.. code-block:: cpp

    int CopyUnitexFile(const char* srcName,const char* dstName);

``CreateUnitexFolder``
----------------------

``CreateUnitexFolder`` Creates a new directory under the standard file system.
If the underlying file system is virtual, the function does nothing.

.. code-block:: cpp

    int CreateUnitexFolder(const char* name);

``RemoveUnitexFolder``
----------------------

``RemoveUnitexFolder`` Removes folder or directory structures. If the underlying
file system is virtual, the function remove all files containing the given prefix
in their name.

.. code-block:: cpp

    int RemoveUnitexFolder(const char* name);

.. index::
    pair: Virtual File System; Java

.. _Java:

Java
####

``numberAbstractFileSpaceInstalled``
------------------------------------

.. code-block:: java

    /**
     * function to known how many abstract file system are installed
     *
     * @return the number of Abstract file system installed in Unitex
     */
    public native static int numberAbstractFileSpaceInstalled();

``writeUnitexFile``
-------------------
.. code-block:: java

    /**
     * writeUnitexFile* function create file to be used by Unitex.
     */
    /**
     * create a file from a raw binary char array
     */
    public native static boolean writeUnitexFile(String fileName,
                                                 char[] fileContent);

    /**
     * create a file from a raw binary byte array
     */
    public native static boolean writeUnitexFile(String fileName,
                                                 byte[] fileContent);

    /**
     * create a file from a string using UTF16LE encoding with BOM (native
     * Unitex format)
     */
    public native static boolean writeUnitexFile(String fileName,
                                                 String fileContent);

    /**
     * create a file from a string using UTF8 encoding without BOM
     */
    public native static boolean writeUnitexFileUtf(String fileName,
                                                    String fileContent);

    /**
     * create a file from a string using UTF8 encoding with or without BOM
     */
    public native static boolean writeUnitexFileUtf(String fileName,
                                                    String fileContent,
                                                    boolean isBom);


``appendUnitexFile``
--------------------

.. code-block:: java

    /**
     * append to a file a raw binary byte array
     */
    public native static boolean appendUnitexFile(String fileName,
        byte[] fileContent);


``getUnitexFileDataChar``
-------------------------

.. code-block:: java

    /**
     * read a file to a raw binary char array representation
     */
    public native static char[] getUnitexFileDataChar(String fileName);


``getUnitexFileData``
---------------------

.. code-block:: java

    /**
     * read a file to a raw binary byte array representation
     */
    public native static byte[] getUnitexFileData(String fileName);


``getUnitexFileString``
-----------------------

.. code-block:: java

    /**
     * read and decode a file to a string.
     */
    public native static String getUnitexFileString(String fileName);

``removeUnitexFile``
--------------------

.. code-block:: java

    /**
     * remove a file
     */
    public native static boolean removeUnitexFile(String fileName);

``createUnitexFolder``
----------------------

.. code-block:: java

    /**
     * create a folder, if needed
     */
    public native static boolean createUnitexFolder(String folderName);

``removeUnitexFolder``
----------------------

.. code-block:: java

    /**
     * remove a folder and the folder content
     */
    public native static boolean removeUnitexFolder(String folderName);

``renameUnitexFile``
--------------------

.. code-block:: java

    /**
     * rename a file
     */
    public native static boolean renameUnitexFile(String fileNameSrc,
        String fileNameDst);

``copyUnitexFile``
------------------

.. code-block:: java

    /**
     * copy a file
     */
    public native static boolean copyUnitexFile(String fileNameSrc,
        String fileNameDst);

``unitexAbstractPathExists``
----------------------------

.. code-block:: java

    /**
     * tests whether a path is already present in Unitex's abstact file space
     */
    public native static boolean unitexAbstractPathExists(String path);

Example:

.. code-block:: java

    public String getVirtualFilePrefix() {
      if (UnitexJni.unitexAbstractPathExists("*")) {
        return "*";
      }

      if (UnitexJni.unitexAbstractPathExists("$:")) {
        return "$:";
      }

      return null;
    }

``getFileList``
---------------

.. code-block:: java

    /**
     * retrieve array of file in abstract space
     */
    public native static String[] getFileList(String path);
