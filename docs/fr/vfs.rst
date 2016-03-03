.. _vfs:

=================================================
Accès aux fichiers et système de fichiers virtuel
=================================================

Les outils Unitex utilisent beaucoup d'accès fichier pour s'échanger des
informations. Afin d'économiser les accès disque, il est possible d'utiliser
des fichiers virtuels (:abbr:`VFS(Virtual File System)`), qui sont écrits en mémoire.

Les fichiers virtuels se reconnaissent à un préfixe :

==========  ============================================================================== ============
**Prefix**  **Implementation**                                                             **License**
==========  ============================================================================== ============
``$:``      système de fichiers virtuel intégré à partir Unitex 3.0.                       LGPLv2
``*:``      système de fichiers virtuel optimisé Ergonotics pour Unitex 2.1 ou 3.0         Propriétaire
==========  ============================================================================== ============

Il existe des fonctions d'accès simplifiées aux fichiers, qui sont compatibles
à la fois avec le système de fichiers virtuel et les fichiers du système de
fichiers standard.

.. index::
    pair: Système de Fichiers Virtuel; C

.. _C:

C
#

.. note::

  Pour toutes les fonctions qui manipulant les fichiers avec des buffers binaires,
  c'est à l'application appelante de gérer la problématique d'encodage Unicode
  (UTF8 ou UTF16).

.. hint::

  Le système de fichier virtuel n’ayant pas de notion de répertoire, les
  caractères ``:`` (deux-points), ``/`` (barre oblique) et ``\`` (barre oblique
  inversée) sont traités comme les autres, ce qui permet, in fine, d'avoir un
  comportement compatible.

``UnitexAbstractPathExists``
----------------------------

``UnitexAbstractPathExists`` permet de savoir si un préfixe correspond à un
système de fichiers virtuel.

.. code-block:: cpp

    int UnitexAbstractPathExists(const char* path);

Ainsi, l'exemple suivant retournera le système de fichier virtuel le plus
optimisé disponible sous Unitex 3.0 :

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

``GetUnitexFileReadBuffer`` permet d'obtenir un pointeur en lecture seule sur
le contenu d'un fichier. Ce pointeur sera valide jusqu'à l'appel de la fonction
``CloseUnitexFileReadBuffer`` correspondant, 

.. warning::

    Le fichier ne doit en aucun cas être modifié (et à plus forte raison supprimé) 
    entre l'appel de ``GetUnitexFileReadBuffer`` et de ``CloseUnitexFileReadBuffer``.

.. code-block:: cpp

    void GetUnitexFileReadBuffer(const char*  name, UNITEXFILEMAPPED** amf,
                                 const void** buffer, size_t* size_file);

``WriteUnitexFile``
-------------------

``WriteUnitexFile`` permet de créer un fichier à partir d'un ou deux buffers
binaires (si un seul buffer est utile, il suffira de positionner ``buffer_suffix``
à ``NULL`` et ``size_suffix`` à ``0``).

.. code-block:: cpp

    int WriteUnitexFile(const char* name, const void* buffer_prefix, size_t size_prefix,
                                         const void* buffer_suffix,size_t size_suffix);

``AppendUnitexFile``
--------------------

``AppendUnitexFile`` permet d'ajouter du contenu à la fin d'un fichier.

.. code-block:: cpp

    int AppendUnitexFile(const char* name,const void* buffer_data,size_t size_data);

``RemoveUnitexFile``
--------------------

``RemoveUnitexFile`` permet de supprimer un fichier.

.. code-block:: cpp

    int RemoveUnitexFile(const char* name);

``RenameUnitexFile``
--------------------

``RenameUnitexFile`` permet de renommer un fichier.

.. code-block:: cpp

    int RenameUnitexFile(const char* oldName,const char* newName);

``CopyUnitexFile``
------------------

``CopyUnitexFile`` permet de copier un fichier. Notons que cette function est
capable de copier un fichier entre le système de fichiers virtuel et le système
de fichiers standard (dans les 2 sens).

.. code-block:: cpp

    int CopyUnitexFile(const char* srcName,const char* dstName);

``CreateUnitexFolder``
----------------------

``CreateUnitexFolder`` n'agit que pour le système de fichier standard et permet de
créer un répertoire.

.. code-block:: cpp

    int CreateUnitexFolder(const char* name);

``RemoveUnitexFolder``
----------------------

``RemoveUnitexFolder`` permet de supprimer un répertoire (dans le système de fichiers standard) ou de supprimer tous les fichiers avec un préfixe donné (dans le système de fichier virtuel).

.. code-block:: cpp

    int RemoveUnitexFolder(const char* name);

.. index::
    pair: Système de Fichiers Virtuel; Java

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
