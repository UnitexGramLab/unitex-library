.. _persistence:

============================
Persistent data access layer
============================

Unitex language-related resources such as graphes ``.fst2``, dictionaries
``.bin`` and ``.inf`` files, may become too large or complex, thus slow
to load. To minimize the load time, the Unitex shared library provides some
useful functions to preload these resources explicitly and reference them
when calling other functions.

For each type of resource (dictionary, graph or alphabet), there exists a
preload function. These functions return, in the buffer pointed to
by ``persistent_filename_buffer``, the name of the persistent resource. The
number of bytes to return in the buffer (i.e., the buffer size) need to be
equal to ``buffer_size``.

.. warning::

  After to preload a dictionary file, do not modify or remove it.
  Under the propietary VFS implementation, a file memory-mapping strategy 
  might being used. As result, your program may produces unpredictable 
  results or stop to working.

.. note::

  The conventions used to create named persistent resources may change and
  depend upon the type of the persistent library installed. You should
  therefore be consistent in using the same file prefix (``$:`` or ``*:``)
  across all function calls.

.. index::
    pair: Persistent resources; C

.. _C:

C
#

``persistence_public_load_dictionary``
--------------------------------------

.. code-block:: cpp

    int persistence_public_load_dictionary(const char* filename,
                                           char* persistent_filename_buffer,
                                           size_t buffer_size);

``persistence_public_is_persisted_dictionary_filename``
-------------------------------------------------

.. code-block:: cpp

    int persistence_public_is_persisted_dictionary_filename(const char*filename);
    
``persistence_public_unload_dictionary``
----------------------------------------

.. code-block:: cpp

    void persistence_public_unload_dictionary(const char* filename);

``persistence_public_load_fst2``
--------------------------------

.. code-block:: cpp

    int persistence_public_load_fst2(const char* filename,
                                     char* persistent_filename_buffer,
                                     size_t buffer_size);

``persistence_public_is_persisted_fst2_filename``
-------------------------------------------------

.. code-block:: cpp

    int persistence_public_is_persisted_fst2_filename(const char*filename);

``persistence_public_unload_fst2``
----------------------------------

.. code-block:: cpp

    void persistence_public_unload_fst2(const char* filename);

``persistence_public_load_alphabet``
------------------------------------

.. code-block:: cpp

    int persistence_public_load_alphabet(const char* filename,
                                         char* persistent_filename_buffer,
                                         size_t buffer_size);

``persistence_public_is_persisted_alphabet_filename``
-------------------------------------------------

.. code-block:: cpp

    int persistence_public_is_persisted_alphabet_filename(const char*filename);

``persistence_public_unload_alphabet``
--------------------------------------

.. code-block:: cpp

    void persistence_public_unload_alphabet(const char* filename);

.. index::
    pair: Persistent resources; Java

.. _Java:

Java
####

``loadPersistentDictionary``
--------------------------------------

.. code-block:: java

    public native static String loadPersistentDictionary(String filename);

``freePersistentDictionary``
--------------------------------------

.. code-block:: java

    public native static void freePersistentDictionary(String filename);

``loadPersistentFst2``
--------------------------------------

.. code-block:: java

    public native static String loadPersistentFst2(String filename);


``freePersistentFst2``
--------------------------------------

.. code-block:: java

    public native static void freePersistentFst2(String filename);


``loadPersistentAlphabet``
--------------------------------------

.. code-block:: java

    public native static String loadPersistentAlphabet(String filename);

``freePersistentAlphabet``
--------------------------------------

.. code-block:: java

    public native static void freePersistentAlphabet(String filename);
