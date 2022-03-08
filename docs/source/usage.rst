Usage
=====
.. _introduction:
.. _programs:
.. _workflows:


Introduction
------------

CONTROL-CORE is a framework for peripheral neuromodulation control systems, which consist of models of organs called physiological models (PMs) and controllers that interact with PMs. The controller and the PM are represented by workflow diagrams (.graphml) that indicate how they are connected together. Nodes in the workflow diagram are programs that are written using the ``concore`` protocol. Edges in the workflow diagram show how the nodes are interconnected. To create a system with ``concore``, you need to write at least two programs (or use existing programs) that are referrenced in the workflow diagram. These programs need to use the ``concore`` methods to receive and transmit neurostimulation data and response.

To develop workflows in the CONTROL-CORE framework, one must go through two steps. First, to develop the programs following the ``concore`` protocol. Second, is to create a workflow from the created programs. 


Programs
------------
.. role:: raw-html(raw)
   :format: html
``concore`` protocol requires the developer (the application developer who develops programs to run on the CONTROL-CORE framework) to split one :raw-html:`<font color="blue">program</font>` into separate :raw-html:`<font color="green">controller</font>` and :raw-html:`<font color="red">PM</font>` programs.

.. image:: images/split-sample.png
  :width: 500
  :alt: Splitting into controller and PM programs
  
  
Now, let's look into how to split an existing program to use ``concore`` as specified above, with a minimal example.  
 
Adapting your program to use ``concore`` protocol
------------ 
 
First, let's consider the below simple program, that does not adhere to the ``concore`` protocol.

.. role:: raw-html(raw)
   :format: html
:raw-html:`<font color="blue">import numpy as np</font><br>`
:raw-html:`<font color="green">ysp = 3.0</font><br>`
:raw-html:`<font color="green">def controller(ym): </font><br>`
:raw-html:`<font color="green">  if ym[0] < ysp:</font><br>`
:raw-html:`<font color="green">     return 1.01 * ym</font><br>`
:raw-html:`<font color="green">  else:</font><br>`
:raw-html:`<font color="green">     return 0.9 * ym</font><br>`
:raw-html:`<font color="red">def pm(u):</font><br>`
:raw-html:`<font color="red">  return u + 0.01</font><br>`
:raw-html:`<font color="blue">ym = np.array([[0.0]]) </font><br>`
:raw-html:`<font color="blue">u = np.array([[0.0]])</font><br>`
:raw-html:`<font color="blue">for i in range(0,150):</font><br>`
:raw-html:`<font color="green">  u = controller(ym)</font><br>`
:raw-html:`<font color="red">  ym = pm(u)</font><br>`
:raw-html:`<font color="blue">  print(" u="+str(u)+ " u="+str(ym))</font><br>`


The above simple code represents your existing program that does not adhere to ``concore`` protocol. That means, it consists of :raw-html:`<font color="green">controller</font>` and :raw-html:`<font color="red">PM</font>` methods in a single integrated program.

Now, let's see how to break this into two different ``concore`` programs, each representing :raw-html:`<font color="green">controller</font>` and :raw-html:`<font color="red">PM.</font>` You must have noticed we have been conistently using colors in our code samples. They have a meaning.

Code segments that represent the :raw-html:`<font color="green">controller</font>` methods are in :raw-html:`<font color="green">green</font>`.

Code segments that represent the :raw-html:`<font color="red">PM</font>` methods are in :raw-html:`<font color="red">red</font>`.

Code segments that are specific to your application, and not specific to your PM or controller are in :raw-html:`<font color="blue">blue</font>`. These segments will likely end up in your both ``concore`` PM and controller programs as we will see shortly.

Let's convert the above program to use ``concore`` now. ``concore`` specific code segments are in black in the two ``concore`` programs displayed below.


The respective ``concore`` controller program:

.. role:: raw-html(raw)
   :format: html
:raw-html:`<font color="blue">import numpy as np</font><br>`
:raw-html:`import concore<br>`
:raw-html:`<font color="green">ysp = 3.0</font><br>`
:raw-html:`<font color="green">def controller(ym): </font><br>`
:raw-html:`<font color="green">  if ym[0] < ysp:</font><br>`
:raw-html:`<font color="green">     return 1.01 * ym</font><br>`
:raw-html:`<font color="green">  else:</font><br>`
:raw-html:`<font color="green">     return 0.9 * ym</font><br>`
:raw-html:`concore.default_maxtime(<font color="blue">150</font>)<br>`
:raw-html:`concore.delay = 0.02<br>`
:raw-html:`init_simtime_u = "[0.0, <font color="blue">0.0</font>]"<br>`
:raw-html:`init_simtime_ym = "[0.0, <font color="blue">0.0</font>]"<br>`
:raw-html:`u = <font color="blue">np.array([</font>concore.initval(init_simtime_u<font color="blue">)]).T</font><br>`
:raw-html:`while(concore.simtime<concore.maxtime):<br>`
:raw-html:`    while concore.unchanged():<br>`
:raw-html:`        ym = concore.read(1,"ym",init_simtime_ym)<br>`
:raw-html:`    ym = <font color="blue">np.array([</font>ym<font color="blue">]).T</font><br>`    
:raw-html:`<font color="green">    u = controller(ym)</font><br>`
:raw-html:`    print(str(concore.simtime) + <font color="blue">    " u="+str(u) + "ym="+str(ym)</font>);<br>`
:raw-html:`    concore.write(1,"u",<font color="blue">list(u.T[0])</font>,delta=<font color="green">0</font>)<br>`
    

``concore`` methods 
########################



Workflows
------------

CONTROL-CORE leverages `DHGWorkflow <https://github.com/controlcore-project/DHGWorkflow>`_ to create such workflows graphically. DHGWorkflow is a browser-based lightweight workflow composer, which lets us to visually create directed hypergraphs (DHGs) and save them as GraphML files. ``concore`` consists of a parser that would interpret the GraphML files created by DHGWorkflow into workflows consisting of ``concore`` programs that interact with each other in a DHG.

.. image:: images/dhg-sample.png
  :width: 400
  :alt: DHG Sample
