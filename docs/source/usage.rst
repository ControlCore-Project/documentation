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
 
Adapting a code to use ``concore`` protocol
------------ 
 
First, let's consider the below simple program, that does not adhere to the ``concore`` protocol.

.. role:: raw-html(raw)
   :format: html
:raw-html:`<font color="blue">import numpy as np</font>`
ysp = 3.0

def controller(ym): 

  if ym[0] < ysp:
  
     return 1.01 * ym
     
  else:
  
     return 0.9 * ym
     
def pm(u):

  return u + 0.01
  
ym = np.array([[0.0]]) 

u = np.array([[0.0]])

for i in range(0,150):

  u = controller(ym)
  
  ym = pm(u)
  
  print(" u="+str(u)+ " u="+str(ym))

  
  
  
``concore`` methods 
########################



Workflows
------------

CONTROL-CORE leverages `DHGWorkflow <https://github.com/controlcore-project/DHGWorkflow>`_ to create such workflows graphically. DHGWorkflow is a browser-based lightweight workflow composer, which lets us to visually create directed hypergraphs (DHGs) and save them as GraphML files. ``concore`` consists of a parser that would interpret the GraphML files created by DHGWorkflow into workflows consisting of ``concore`` programs that interact with each other in a DHG.

.. image:: images/dhg-sample.png
  :width: 400
  :alt: DHG Sample
