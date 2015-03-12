.. _silent:

===============================
Suppression des sorties console
===============================

Dans un but d'optimisation, de performance et de compatibilité multithread
(pour éviter un mélange de sortie des outils s'exécutant simultanément),
il est conseillé (hors debuggage) de supprimer les sorties console d'Unitex.

.. index::
    pair: Suppression des sorties console; C

.. _C:

C
#

.. code-block:: cpp

  /* There is a set of callbacks for rerouting stdin, stdout and stderr IO */
  /* t_fnc_stdOutWrite (for stdout and stderr) and t_fnc_stdIn (for stdin) define
     the callback.
     the callback must return the number of char processed (the actual size if operating normally)
  */

  enum stdwrite_kind { stdwrite_kind_out=0, stdwrite_kind_err } ;

  typedef size_t (ABSTRACT_CALLBACK_UNITEX *t_fnc_stdOutWrite)(const void*Buf, size_t size,void* privatePtr);
  /* SetStdWriteCB sets the callback for one of the two (stdout or stderr) output streams
     if trashOutput == 1, fnc_stdOutWrite must be NULL and the output will just be ignored
     if trashOutput == 0 and fnc_stdOutWrite == NULL and the output will be the standard output
     if trashOutput == 0 and fnc_stdOutWrite != NULL the callback will be used

     the callback is called with a zero size in only one case: when SetStdWriteCB is called, the preceding
     callback is called a last time (with its associated privatePtr) with a zero size.
     This may allow you, for instance, to close a file or free some memory...

     privatePtr is a private value which is passed as the last parameters of a callback
     GetStdWriteCB sets the current value on *p_trashOutput, *p_fnc_stdOutWrite and **p_privatePtr

     returns 1 if successful and 0 if an error occurred on SetStdWriteCB or GetStdWriteCB
     */

  UNITEX_FUNC int UNITEX_CALL SetStdWriteCB(enum stdwrite_kind swk, int trashOutput,
                                          t_fnc_stdOutWrite fnc_stdOutWrite,void* privatePtr);
  UNITEX_FUNC int UNITEX_CALL GetStdWriteCB(enum stdwrite_kind swk, int* p_trashOutput,
                                          t_fnc_stdOutWrite* p_fnc_stdOutWrite,void** p_privatePtr);


L'implémentation C permet de plus de rediriger les sorties vers une fonction callback.

.. code-block:: cpp

  #include "AbstractFilePlugCallback.h"

  SetStdWriteCB(stdwrite_kind_out, 1, NULL,NULL);
  SetStdWriteCB(stdwrite_kind_err, 1, NULL,NULL);

.. index::
    pair: Suppression des sorties console; Java

.. _Java:

Java
####

.. code-block:: java

  /**
   * allow ignore (flushMode is TRUE) or emit normal message to stdout
   */
  public native static boolean setStdOutTrashMode(boolean flushMode);

  /**
   * allow ignore (flushMode is TRUE) or emit error message to stderr
   */
  public native static boolean setStdErrTrashMode(boolean flushMode);

Pour supprimer les sorties console d’Unitex, on pourra donc appeler :

.. code-block:: java

  UnitexJni.setStdOutTrashMode (true);
  UnitexJni.setStdErrTrashMode (true);
