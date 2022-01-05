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
``concore`` protocol requires the developer (the application developer who develops programs to run on the CONTROL-CORE framework) to split one program into separate controller and PM programs.

.. image:: images/split-sample.png
  :width: 400
  :alt: Splitting into controller and PM programs
  
  
``concore`` methods 
########################



Workflows
------------

CONTROL-CORE provides a workflow composer, known as DHGWorkflow, to create such workflows graphically.

.. image:: images/dhg-sample.png
  :width: 400
  :alt: DHG Sample
