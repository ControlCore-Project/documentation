The Concore Action Dev docs
===========================

Introduction
------------

The current implementation uses Github REST API to push code to github by authenticating with github fine grained access token (refer https://github.blog/2022-10-18-introducing-fine-grained-personal-access-tokens-for-github/)

How it works
------------

- On triggering ``contribute`` action in fri server, it executes the ``contribute.py`` script

- Let's assume the paramters provided are:
    - Study Name - test study
    - Study Path - F:\\example
    - Author Name - test user

- It will create a new branch named ``AuthorName_StudyName`` i.e test_user_test_study in the bot repository ``parteekcoder/concore`` which references the default branch in ``parteekcoder123/concore``

.. image:: images/ca-github-branch-ref.png
  :width: 700

- Now creating Github blob (refer https://docs.github.com/en/rest/git/blobs ) for each file in the directory here ``F:\\example``. After creating blobs , create a new Git tree (refer https://docs.github.com/en/rest/git/trees)

- After creating Github tree, change the refrence of the newly created branch to the that tree

.. image:: images/ca-github-working-chart.png
  :width: 700

- Now the changes have been pushed to github, and for creating pull request the  ``pull_request.yml`` workflow is runned which creates pull request to upstream repo using persoanl access token of upstream repo

``Note: You need to store the personal access token of upstream repo as github secret in bot repo``

Creating token for bot
----------------------

- Open the bot account at github and navigate as follow:

  
  settings >  Developer settings > Personal access token > Fine-grained access token > Generate New token

  Or 

  click https://github.com/settings/personal-access-tokens/new

- Fill token name,description expiration time

- Under the ``Repository access`` section select ``Only select repositories`` to select the repository for which you want to provide the access

- Under the ``Permissions`` , provide the read-write permissions for **Contents** and **Actions** in the ``Repository permission``

- Click **Generate token** button

- Then copy the generated token and hash it using this website in base64 encoding http://www.unit-conversion.info/texttools/base64/

- Place the token in contribute.py script in concore repo


Creating token for workflow
---------------------------

- Create a  Personal access token of upstream repo

- Place that token in secrets of bot repo so that the workflow 