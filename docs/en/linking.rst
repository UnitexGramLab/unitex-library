.. _linking:

========================
Linking your application
========================

Unitex could be compiled into a shared library which exports all the engine
high-level functions, and then that shared library linked when compiling an 
application. It is also possible to compile Unitex into a JNI library,
and using it from Java.

.. note::
    A JNI is a shared-object library (on Windows, a DLL) that contains 
    entry points reachable in Java via the JVM. JNI lets you call Java 
    methods from C functions and implement Java classes in C.

The exported functions can be called like any other Unitex command-line
tool.

Every command-line tool has an exported function. The most frequently used 
tools, which are adapted and optimized for use as library, are those 
dedicated to the execution of graphs, compared to those for their creation:

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
    pair: Library linking; C

.. _C:

C
#

The following two functions call a Unitex tool from the Unitex library, the 
used syntax is similar to call the ``main()`` function from C.

.. code-block:: cpp

    #include "UnitexTool.h"
    int UnitexTool_public_run(int argc,char* const argv[],int* p_number_done,struct pos_tools_in_arg* ptia);
    int UnitexTool_public_run_one_tool(const char*toolname,int argc,char* const argv[]);
	
Starting Unitex 3.1.4012-beta, it is possible to call a function from a 
character buffer using ``UnitexTool_public_run_string()`` as follows:

.. code-block:: cpp

    int UnitexTool_public_run_string(const char* cmd_line);
    int UnitexTool_public_run_string_ret_infos(const char* cmd_line, int* p_number_done, struct pos_tools_in_arg* ptia);

For instance:

CheckDic
********

  .. code-block:: cpp

    const char *CheckDic_Argv[] = {"CheckDic","c:\\foo\\bar.dic","DELAF"};
    int ret = UnitexTool_public_run_one_tool("CheckDic",3,CheckDic_Argv);

    // or
	
    int ret = UnitexTool_public_run_string("UnitexTool CheckDic");
	
Tokenize
********

  .. code-block:: cpp

    const char* Tokenize_Argv[]={"UnitexTool","Tokenize","-a","*english/Alphabet.txt",UfoSntFileVFN};
    int retTok = UnitexTool_public_run(5,Tokenize_Argv,NULL,NULL);

    // or
    
    int retTok = UnitexTool_public_run_string("UnitexTool Tokenize -a \"*english/Alphabet.txt\"");

.. index::
    pair: Library linking; Java

.. _Java:

Java
####

``execUnitexTool``
******************


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

For example:

.. code-block:: java

    UnitexJni.execUnitexTool(new String[] {"UnitexToolLogger","Normalize",PFX+txt, "-r", dirRes+"Norm.txt"});
