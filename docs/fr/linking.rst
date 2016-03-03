.. _linking:

===============================
Invoquer la bibliothèque Unitex
===============================

Unitex peut être compilé en tant que bibliothèque dynamique standard,
exportant des fonctions C, appelable à partir de votre logiciel, ou
en tant que bibliothèque JNI, appelable à partir d’un programme Java.

.. note::
    L'interface JNI est une bibliothèque dynamique standard du
    système, qui exporte en plus des fonctions C des fonctions JNI pour Java.
    `Qui peut le plus peut le moins` : on peut appeler les fonctions C à partir
    de la bibliothèque JNI.

Ensuite, chaque outil Unitex est appelé avec des paramètres similaires à
ceux utilisés avec les outils en ligne de commande.

Chaque outil Unitex est susceptible d'être appelé à partir de la bibliothèque.
Les outils les plus testés, adaptés et optimisés à une utilisation en
bibliothèque sont ceux dédiés à l'exécution de graphes, en comparaison à ceux
destinés à leur création :

* CheckDic
* Compress
* Concord
* Dico
* Fst2Txt
* Locate
* Normalize
* SortTxt
* Tokenize

.. index::
    pair: Bibliothèque de Liaison; C

.. _C:

C
#

Les deux fonctions suivantes permettent d’appeler un outil de la bibliothèque
Unitex, avec une syntaxe proche du traditionnel ``main()`` du C.

.. code-block:: cpp

    #include "UnitexTool.h"
    int UnitexTool_public_run(int argc,char* const argv[],int* p_number_done,struct pos_tools_in_arg* ptia);
    int UnitexTool_public_run_one_tool(const char*toolname,int argc,char* const argv[]);
	
A partir de la révision 4012 d'Unitex 3.1 beta, il est possible d'utiliser UnitexTool_public_run_string
en utilisant une simple chaine de caractère avec les commandes:

.. code-block:: cpp

    int UnitexTool_public_run_string(const char* cmd_line);
    int UnitexTool_public_run_string_ret_infos(const char* cmd_line, int* p_number_done, struct pos_tools_in_arg* ptia);
	

Par exemple :

CheckDic
********

  .. code-block:: cpp

    const char *CheckDic_Argv[] = {"CheckDic","c:\\foo\\bar.dic","DELAF"};
    int ret = UnitexTool_public_run_one_tool("CheckDic",3,CheckDic_Argv);

    // ou
	
    int ret = UnitexTool_public_run_string("UnitexTool CheckDic");
	
Tokenize
********

  .. code-block:: cpp

    const char* Tokenize_Argv[]={"UnitexTool","Tokenize","-a","*english/Alphabet.txt",UfoSntFileVFN};
    int retTok = UnitexTool_public_run(5,Tokenize_Argv,NULL,NULL);

    // ou
	
    int retTok = UnitexTool_public_run_string("UnitexTool Tokenize -a \"*english/Alphabet.txt\"");
		
.. index::
    pair: Bibliothèque de Liaison; Java

.. _Java:

Java
####

.. code-block:: java

    import fr.umlv.unitex.jni.UnitexJni;

    /**
     * Function to run UnitexTool with string or string array, like java exec in
     * java runtime
     * you can combine several tool using { }
     * (see UnitexTool in Unitex manual for more information)
     *
     * String [] strArrayCmds={"UnitexTool","{","Normalize","corpus.txt",
             "-r", "Norm.txt","}","{","Tokenize","corpus.txt", "-r", "Alphabet.txt","}"};
     *
     * UnitexLibAndJni.execUnitexTool(strArrayCmds);
         *
     *
     * @return value : the return value of the tools (0 for success)
     */
    public native static int execUnitexTool(String[] cmdarray);


    /**
     * Function to run UnitexTool with string or string array, like java exec in
     * java runtime
     * you can combine several tool using { }
     * (see UnitexTool in Unitex manual for more information)
     *
     * UnitexLibAndJni.execUnitexTool("UnitexTool Normalize \"corpus.txt\" -r \"Norm.txt\"");
     *
     * UnitexLibAndJni.execUnitexTool("UnitexTool Tokenize \"corpus.txt\" -a \"Alphabet.txt\"");
     *
     * UnitexLibAndJni.execUnitexTool("UnitexTool { Normalize \"corpus.txt\" -r \"Norm.txt\" }" +
     *                                        " { Tokenize \"corpus.txt\" -a \"Alphabet.txt\" }");
     *
     *
     * @return value : the return value of the tools (0 for success)
     */
    public native static int execUnitexTool(String cmdline);

Par example :

.. code-block:: java

    UnitexJni.execUnitexTool(new String[] {"UnitexToolLogger","Normalize",PFX+txt, "-r", dirRes+"Norm.txt"});

