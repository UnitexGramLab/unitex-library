.. _persistence:

========================================
Persistance des ressources linguistiques
========================================

Les ressources linguistiques (graphes ``.fst2``, dictionnaires ``.bin`` et fichiers
``.inf``, et dans une bien moindre mesure les fichiers ``Alphabet.txt``) prennent
beaucoup de temps pour être chargées. Les dictionnaires sont gros, les graphes sont
complexes. Pour économiser ce temps à chaque lancement d'un outil, il est possible
de pré-charger ces ressources avant d'appeler les outils Unitex.

Pour cela, il existe pour chaque type de ressource (dictionnaire, graphe et alphabet)
une fonction de pré-chargement. Cette fonction retourne le nom de la ressource
persistante (dans le buffer ``persistent_filename_buffer`` de taille ``buffer_size``
qui devra être fourni par l'appelant en C, comme valeur de retour des fonctions Java).

.. note::

  Les fichiers dictionnaires utilisés doivent rester présents et non modifiés,
  car la technique de mappage de fichiers en mémoire peut être utilisée.

.. warning::

  La règle de constitution du nom de la ressource persistante pouvant évoluer
  et dépendre du type de librairie de persistance installée, c'est bien ce nom
  qui devra être fourni comme paramètre aux outils Unitex correspondant
  (``Locate``, ``Dico``...), et éventuellement aux fonctions de déchargement
  correspondant.


.. index::
    pair: Persistance des ressources; C

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
    pair: Persistance des ressources; Java

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
