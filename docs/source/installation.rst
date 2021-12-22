Installation
=====


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

You may just click *Enter* for all of the above. Now you have created the keys, if you did not have already.

Now that you have the keys, start the ssh agent.

.. code-block:: console

 $ eval "$(ssh-agent -s)"
 > Agent pid 59566

Add your SSH private key to the ssh-agent and store your passphrase in the keychain. If you created your key with a different name, or if you are adding an existing key that has a different name, replace id_ed25519 in the command with the name of your private key file.

First finding the name:

.. code-block:: console

 $ ls -al ~/.ssh

Then,

.. code-block:: console

 $ ssh-add -K ~/.ssh/id_ed25519
 
Now that you have added your key, you can proceed to the next step of adding the key to your GitHub profile. In the upper-right corner of any page, click your profile photo, then click Settings. 

 
Open and copy the contents of ~/.ssh/id_ed25519.pub (or a similar .pub file from the .ssh directory).

.. code-block:: console

 $ cat ~/.ssh/id_ed25519.pub
  
In the upper-right corner of any page in GitHub after you have logged, click your profile photo, then click Settings. 

.. image:: https://docs.github.com/assets/cb-34573/images/help/settings/userbar-account-settings.png
  :width: 400
  :alt: GitHub Settings
  
In the user settings sidebar, click SSH and GPG keys. 

.. image:: https://docs.github.com/assets/cb-17145/images/help/settings/settings-sidebar-ssh-keys.png
  :width: 400
  :alt: SSH and GPG keys
  
Click New SSH key or Add SSH key. 

.. image:: https://docs.github.com/assets/cb-11964/images/help/settings/ssh-add-ssh-key.png
  :width: 400
  :alt: Add SSH key
  
In the "Title" field, add a descriptive label for the new key. Paste your key (the content of ~/.ssh/id_ed25519.pub or a similar file that you copied in the previous step) into the "Key" field. 

.. image:: https://docs.github.com/assets/cb-24835/images/help/settings/ssh-key-paste.png
  :width: 400
  :alt: Paste SSH key

Click Add SSH key. Click Add SSH key. Now, you are ready to checkout the private GitHub repository with the below commands, as long as you are already added to the respective repository.

.. code-block:: console

 $ git clone git@github.com:ControlCore-Project/concore20.git
 
 $ cd concore20
 
 $ pip install -r requirements.txt
