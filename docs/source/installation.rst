Installation
=====
To use CONTROL-CORE, first install its dependencies using pip:

.. code-block:: console
 
 $ pip install -r requirements.txt

At this point, as we rapidly develop CONTROL-CORE, it remains a private repository in GitHub, although we aim to clean up the source code and make it public soon. Therefore, certain steps must be made to checkout the source code, since GitHub has discontinued password-based cloning.

First, check whether your computer has SSH configured.

.. code-block:: console

 $ ls -al ~/.ssh

If you have already configured, you will see files similar to the below.

.. code-block:: console

 id_rsa.pub
 id_ecdsa.pub
 id_ed25519.pub

If not, make sure to configure SSH and create new keys before proceeding to the next step.

If no keys found in the above step, please follw these steps.

.. code-block:: console

 $ ssh-keygen -t ed25519 -C "your_email@example.com"
 > Generating public/private algorithm key pair.
 > Enter a file in which to save the key (/Users/you/.ssh/id_algorithm): [Press enter]
 > Enter passphrase (empty for no passphrase): [Type a passphrase]
 > Enter same passphrase again: [Type passphrase again]

You may just click _Enter_ for all of the above. Now you have created the keys, if you did not have already.

Now that you have the keys, start the ssh agent.

.. code-block:: console

 $ eval "$(ssh-agent -s)"
 > Agent pid 59566

Add your SSH private key to the ssh-agent and store your passphrase in the keychain. If you created your key with a different name, or if you are adding an existing key that has a different name, replace id_ed25519 in the command with the name of your private key file.

.. code-block:: console

 $ ssh-add -K ~/.ssh/id_ed25519
 
Now that you have added your key, you can proceed to the next step of adding the key to your GitHub profile.
 
Open and copy the ~/.ssh/id_ed25519.pub (or a similar .pub file from the .ssh directory).

In the upper-right corner of any page in GitHub after you have logged, click your profile photo, then click Settings. 


.. code-block:: console

 $ git clone git@github.com:ControlCore-Project/concore20.git
 
 $ cd concore20
 
 $ pip install -r requirements.txt
